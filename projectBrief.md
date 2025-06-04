# TaskFlow Pro - Project Management Platform

## Project Overview

**Project Name**: TaskFlow Pro  
**Project Type**: Full-stack web application  
**Primary Language**: TypeScript  
**Framework**: Next.js 14 with App Router  
**Database**: PostgreSQL with Prisma ORM  
**Authentication**: NextAuth.js with OAuth providers  
**Deployment**: Vercel (frontend) + Railway (database)  
**Timeline**: 8-week development cycle  
**Team Size**: 3 developers  

### Business Context
TaskFlow Pro is a modern project management platform designed for small to medium-sized development teams. The application provides task tracking, team collaboration, time logging, and project analytics with real-time updates and mobile-responsive design.

### Key Features
- **Project Dashboard**: Real-time project status with interactive charts
- **Task Management**: Kanban boards, priority levels, and assignment workflows
- **Team Collaboration**: Comments, mentions, file attachments, and notifications
- **Time Tracking**: Built-in timer with reporting and analytics
- **API Integration**: RESTful API + GraphQL for external tool connections

## Architecture Overview

### System Architecture
```
Frontend (Next.js) → API Routes → Prisma ORM → PostgreSQL
                  → WebSocket Server → Real-time Updates
                  → External APIs (GitHub, Slack, etc.)
```

### Technology Stack
- **Frontend**: Next.js 14, TypeScript, Tailwind CSS, shadcn/ui components
- **Backend**: Next.js API routes, tRPC for type-safe APIs
- **Database**: PostgreSQL 15, Prisma ORM, Redis for caching
- **Real-time**: Socket.io for live updates
- **Authentication**: NextAuth.js with GitHub, Google OAuth
- **File Storage**: AWS S3 or Vercel Blob
- **Monitoring**: Sentry for error tracking, Vercel Analytics

### Key Architectural Decisions
1. **Monorepo Structure**: Single repository with clear separation of concerns
2. **Type Safety**: End-to-end TypeScript with strict mode enabled
3. **Database-First**: Prisma schema drives API and frontend types
4. **Component Library**: Custom design system built on shadcn/ui
5. **Progressive Enhancement**: Server-side rendering with client hydration

## Project Structure

```
taskflow-pro/
├── src/
│   ├── app/                    # Next.js 14 App Router pages
│   │   ├── (auth)/            # Authentication routes
│   │   ├── dashboard/         # Main application pages
│   │   ├── api/               # API route handlers
│   │   └── globals.css        # Global styles
│   ├── components/            # Reusable UI components
│   │   ├── ui/                # Base UI components (shadcn/ui)
│   │   ├── forms/             # Form components with validation
│   │   ├── charts/            # Data visualization components
│   │   └── layout/            # Layout and navigation components
│   ├── lib/                   # Utility functions and configurations
│   │   ├── db.ts              # Database connection and queries
│   │   ├── auth.ts            # Authentication configuration
│   │   ├── validations.ts     # Zod schemas for validation
│   │   └── utils.ts           # General utility functions
│   ├── types/                 # TypeScript type definitions
│   │   ├── api.ts             # API response types
│   │   ├── database.ts        # Database model types
│   │   └── auth.ts            # Authentication types
│   └── hooks/                 # Custom React hooks
├── prisma/
│   ├── schema.prisma          # Database schema definition
│   ├── migrations/            # Database migration files
│   └── seed.ts                # Database seeding script
├── public/                    # Static assets
├── docs/                      # Project documentation
│   ├── api.md                 # API documentation
│   ├── deployment.md          # Deployment guide
│   └── contributing.md        # Development guidelines
├── tests/                     # Test files
│   ├── __tests__/             # Unit and integration tests
│   ├── e2e/                   # End-to-end tests (Playwright)
│   └── fixtures/              # Test data and mocks
├── .env.example              # Environment variables template
├── package.json              # Dependencies and scripts
├── tailwind.config.js        # Tailwind CSS configuration
├── next.config.js            # Next.js configuration
└── tsconfig.json             # TypeScript configuration
```

---

## Writing Guidelines for AI Development Tooling

### 1. Project Overview Section

**Purpose**: Provide immediate context for AI assistants to understand the project scope, technology choices, and business requirements.

**Best Practices**:
- **Be Specific**: Include exact framework versions, not just "React" but "Next.js 14 with App Router"
- **Business Context**: Explain the "why" behind the project to help AI understand feature priorities
- **Constraints**: Mention timeline, team size, and any technical limitations
- **Success Metrics**: Define what "done" looks like for the project

**AI-Friendly Elements**:
- Use consistent terminology throughout the document
- Include technology stack with specific versions
- Mention deployment targets and hosting constraints
- List primary user personas and use cases

### 2. Architecture Overview Section

**Purpose**: Help AI assistants understand system boundaries, data flow, and technology integration points.

**Best Practices**:
- **Visual Diagrams**: Include ASCII art or mermaid diagrams for system architecture
- **Data Flow**: Clearly document how data moves through the system
- **External Dependencies**: List all third-party services and APIs
- **Scalability Considerations**: Mention performance requirements and bottlenecks

**AI-Friendly Elements**:
- Use standard architectural patterns (MVC, microservices, etc.)
- Include API contracts and data schemas
- Document authentication and authorization flows
- Specify caching strategies and database relationships

### 3. Project Structure Section

**Purpose**: Provide AI assistants with a mental map of the codebase for accurate file creation and modification.

**Best Practices**:
- **Hierarchical Organization**: Show folder structure with clear nesting
- **Purpose Documentation**: Explain what each major folder contains
- **Naming Conventions**: Document file and folder naming patterns
- **Configuration Files**: Include all config files with brief descriptions

**AI-Friendly Elements**:
- Use consistent folder naming (kebab-case, camelCase, etc.)
- Group related functionality together
- Include test file locations and naming patterns
- Document build and deployment artifact locations

### 4. Additional Recommended Sections

#### Development Environment Setup
```markdown
## Development Environment

### Prerequisites
- Node.js 18+ (recommend using fnm or nvm)
- PostgreSQL 15+ running locally or Docker
- Git for version control

### Quick Start
1. `npm install` - Install dependencies
2. `cp .env.example .env.local` - Configure environment
3. `npx prisma db push` - Setup database schema
4. `npm run dev` - Start development server
5. Visit `http://localhost:3000`

### Available Scripts
- `npm run dev` - Development server with hot reload
- `npm run build` - Production build
- `npm run test` - Run unit tests
- `npm run e2e` - Run end-to-end tests
- `npm run db:studio` - Open Prisma Studio
```

#### API Documentation
```markdown
## API Reference

### Authentication
All API endpoints require JWT authentication via `Authorization: Bearer <token>` header.

### Endpoints
- `GET /api/projects` - List user projects
- `POST /api/projects` - Create new project
- `GET /api/projects/[id]/tasks` - Get project tasks
- `POST /api/tasks` - Create new task
- `PATCH /api/tasks/[id]` - Update task status

### Error Handling
API returns standard HTTP status codes with JSON error responses:
\```json
{
  "error": "Validation failed",
  "details": ["Title is required", "Due date must be in future"]
}
```\

```

#### Coding Standards
```markdown
## Coding Standards

### TypeScript Guidelines
- Use strict mode with no implicit any
- Prefer interfaces over types for object shapes
- Use enums for fixed value sets
- Document complex types with JSDoc comments

### React Patterns
- Use functional components with hooks
- Implement proper error boundaries
- Follow React Query patterns for server state
- Use React.memo() for expensive re-renders

### Database Patterns
- Use Prisma transactions for multi-table operations
- Implement soft deletes with deletedAt timestamps
- Include createdAt/updatedAt on all models
- Use database constraints for data integrity
```

### Sample Data Examples

This project brief includes realistic sample data that demonstrates:
- **Technology Stack**: Modern, production-ready technologies
- **Folder Structure**: Logical organization following Next.js conventions
- **Feature Complexity**: Mix of CRUD operations and real-time features
- **Integration Points**: External APIs, authentication, and file storage
- **Scalability**: Caching, database optimization, and monitoring

The sample TaskFlow Pro project provides enough complexity to showcase various development patterns while remaining comprehensible for AI tooling to understand and work with effectively.

