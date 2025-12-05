---
name: dev-backend-implementation
description: Plan backend implementation with module structure, services, and business logic. Use when creating detailed backend development plans for Nest.js, Express, or similar frameworks.
---

# Backend Implementation Planning

## Purpose

Use this Skill to create detailed backend implementation plans:
- Module and service architecture
- Business logic organization
- Dependency injection structure
- Implementation task breakdown

## Inputs

The main agent should provide:
- Technical architecture document
- API specifications
- Database schema
- Authentication requirements

## Process

### 1. Module Structure

Design the module hierarchy:

```
src/
├── app.module.ts              # Root module
├── main.ts                    # Bootstrap
│
├── common/                    # Shared utilities
│   ├── decorators/           # Custom decorators
│   ├── filters/              # Exception filters
│   ├── guards/               # Auth guards
│   ├── interceptors/         # Request/response interceptors
│   ├── pipes/                # Validation pipes
│   └── utils/                # Helper functions
│
├── config/                    # Configuration
│   ├── config.module.ts
│   ├── database.config.ts
│   ├── jwt.config.ts
│   └── app.config.ts
│
├── database/                  # Database setup
│   ├── database.module.ts
│   ├── migrations/
│   └── seeds/
│
└── modules/                   # Feature modules
    ├── auth/
    ├── users/
    ├── projects/
    └── tasks/
```

### 2. Feature Module Template

For each feature module:

```
modules/projects/
├── projects.module.ts         # Module definition
├── projects.controller.ts     # HTTP handlers
├── projects.service.ts        # Business logic
├── projects.repository.ts     # Data access (optional)
│
├── dto/                       # Data transfer objects
│   ├── create-project.dto.ts
│   ├── update-project.dto.ts
│   └── project-response.dto.ts
│
├── entities/                  # Database entities
│   └── project.entity.ts
│
├── interfaces/                # TypeScript interfaces
│   └── project.interface.ts
│
└── __tests__/                 # Unit tests
    ├── projects.controller.spec.ts
    └── projects.service.spec.ts
```

### 3. Service Layer Design

Define service responsibilities:

```typescript
// projects.service.ts
@Injectable()
export class ProjectsService {
  // CRUD operations
  async create(userId: string, dto: CreateProjectDto): Promise<Project>
  async findAll(query: ProjectQueryDto): Promise<PaginatedResult<Project>>
  async findOne(id: string): Promise<Project>
  async update(id: string, dto: UpdateProjectDto): Promise<Project>
  async remove(id: string): Promise<void>
  
  // Business operations
  async addMember(projectId: string, userId: string, role: Role): Promise<void>
  async removeMember(projectId: string, userId: string): Promise<void>
  async archive(id: string): Promise<void>
  async duplicate(id: string): Promise<Project>
  
  // Query operations
  async findByOwner(userId: string): Promise<Project[]>
  async findByMember(userId: string): Promise<Project[]>
}
```

### 4. Controller Design

Map routes to handlers:

```typescript
// projects.controller.ts
@Controller('projects')
@ApiTags('Projects')
@UseGuards(JwtAuthGuard)
export class ProjectsController {
  @Get()
  @ApiOperation({ summary: 'List projects' })
  async findAll(
    @Query() query: ProjectQueryDto,
    @CurrentUser() user: User,
  ): Promise<PaginatedResponse<ProjectResponseDto>>

  @Get(':id')
  @ApiOperation({ summary: 'Get project by ID' })
  async findOne(
    @Param('id', ParseUUIDPipe) id: string,
  ): Promise<ProjectResponseDto>

  @Post()
  @ApiOperation({ summary: 'Create project' })
  async create(
    @Body() dto: CreateProjectDto,
    @CurrentUser() user: User,
  ): Promise<ProjectResponseDto>

  @Patch(':id')
  @ApiOperation({ summary: 'Update project' })
  async update(
    @Param('id', ParseUUIDPipe) id: string,
    @Body() dto: UpdateProjectDto,
  ): Promise<ProjectResponseDto>

  @Delete(':id')
  @HttpCode(HttpStatus.NO_CONTENT)
  @ApiOperation({ summary: 'Delete project' })
  async remove(
    @Param('id', ParseUUIDPipe) id: string,
  ): Promise<void>
}
```

### 5. DTO Definitions

Create validated DTOs:

```typescript
// create-project.dto.ts
export class CreateProjectDto {
  @IsString()
  @MinLength(3)
  @MaxLength(100)
  @ApiProperty({ example: 'My Project' })
  name: string;

  @IsOptional()
  @IsString()
  @MaxLength(1000)
  @ApiProperty({ required: false })
  description?: string;

  @IsEnum(ProjectVisibility)
  @ApiProperty({ enum: ProjectVisibility })
  visibility: ProjectVisibility;
}

// project-response.dto.ts
export class ProjectResponseDto {
  @ApiProperty()
  id: string;

  @ApiProperty()
  name: string;

  @ApiProperty({ type: () => UserSummaryDto })
  owner: UserSummaryDto;

  @ApiProperty()
  createdAt: Date;

  static fromEntity(project: Project): ProjectResponseDto {
    // Map entity to DTO
  }
}
```

### 6. Entity Definitions

Define ORM entities:

```typescript
// project.entity.ts
@Entity('projects')
export class Project {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ length: 100 })
  name: string;

  @Column({ type: 'text', nullable: true })
  description: string | null;

  @Column({
    type: 'enum',
    enum: ProjectVisibility,
    default: ProjectVisibility.PRIVATE,
  })
  visibility: ProjectVisibility;

  @ManyToOne(() => User, { onDelete: 'RESTRICT' })
  @JoinColumn({ name: 'owner_id' })
  owner: User;

  @Column({ name: 'owner_id' })
  ownerId: string;

  @OneToMany(() => Task, (task) => task.project)
  tasks: Task[];

  @CreateDateColumn({ name: 'created_at' })
  createdAt: Date;

  @UpdateDateColumn({ name: 'updated_at' })
  updatedAt: Date;

  @DeleteDateColumn({ name: 'deleted_at' })
  deletedAt: Date | null;
}
```

### 7. Authentication Module

Plan auth implementation:

```typescript
// Auth module structure
modules/auth/
├── auth.module.ts
├── auth.controller.ts
├── auth.service.ts
├── strategies/
│   ├── jwt.strategy.ts
│   ├── local.strategy.ts
│   └── google.strategy.ts (optional)
├── guards/
│   ├── jwt-auth.guard.ts
│   └── roles.guard.ts
├── decorators/
│   ├── current-user.decorator.ts
│   └── roles.decorator.ts
└── dto/
    ├── login.dto.ts
    ├── register.dto.ts
    └── tokens.dto.ts
```

### 8. Error Handling

Implement global exception handling:

```typescript
// http-exception.filter.ts
@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    const status = exception instanceof HttpException
      ? exception.getStatus()
      : HttpStatus.INTERNAL_SERVER_ERROR;

    const errorResponse = {
      error: {
        code: this.getErrorCode(exception),
        message: this.getMessage(exception),
        details: this.getDetails(exception),
        requestId: request.id,
      },
      timestamp: new Date().toISOString(),
      path: request.url,
    };

    response.status(status).json(errorResponse);
  }
}
```

### 9. Testing Strategy

Plan test coverage:

```typescript
// Unit test example
describe('ProjectsService', () => {
  let service: ProjectsService;
  let repository: MockType<Repository<Project>>;

  beforeEach(async () => {
    const module = await Test.createTestingModule({
      providers: [
        ProjectsService,
        { provide: getRepositoryToken(Project), useFactory: mockRepository },
      ],
    }).compile();

    service = module.get(ProjectsService);
    repository = module.get(getRepositoryToken(Project));
  });

  describe('create', () => {
    it('should create a project', async () => {
      // Test implementation
    });

    it('should throw if user not found', async () => {
      // Test error case
    });
  });
});
```

### 10. Implementation Tasks

Break down into estimatable tasks:

| Task | Module | Estimate | Dependencies |
|------|--------|----------|--------------|
| Setup Nest.js project | Core | 2h | - |
| Configure database connection | Database | 2h | Setup |
| Implement auth module | Auth | 8h | Database |
| Implement users module | Users | 6h | Auth |
| Implement projects module | Projects | 8h | Users |
| Implement tasks module | Tasks | 6h | Projects |
| Add Swagger documentation | Core | 2h | All modules |
| Write unit tests | All | 8h | All modules |
| Write e2e tests | All | 4h | All modules |

## Output Format

Produce documentation with:
- Module structure diagram
- Service interface definitions
- Controller endpoint mapping
- DTO specifications
- Entity definitions
- Auth flow documentation
- Test plan
- Task breakdown with estimates

## Best Practices

- **Single responsibility**: Each service handles one domain
- **Dependency injection**: Use constructor injection
- **Validation at boundaries**: Validate all inputs via DTOs
- **Consistent error handling**: Use exception filters
- **Test coverage**: Aim for 80%+ on business logic
- **Documentation**: Swagger for all endpoints
