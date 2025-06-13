# Senior → Junior Agent Handoff Template

> Standardized task delegation format for multi-agent vibe coding workflows

## Task Header

```markdown
## Task: [ComponentName/FeatureName/EndpointName]
**Agent**: [Junior Frontend/Junior Backend/DevOps]
**Priority**: [High/Medium/Low]
**Complexity**: [Simple/Medium/Complex]
**Estimated Duration**: [30min/2h/4h/8h]
**Token Budget**: [25K/40K/60K]
**Due Date**: [YYYY-MM-DD or relative timeline]
```

## Context Section

```markdown
### Business Context
Brief explanation of what this task achieves for the user/business.
- Why this feature matters
- How it fits into the larger workflow
- Success metrics or KPIs

### Technical Context  
- Current system state
- Related components/services
- Integration requirements
- Dependencies on other tasks
```

## Requirements Section

```markdown
### Functional Requirements
- [ ] Requirement 1: Clear, testable behavior
- [ ] Requirement 2: Specific user interaction
- [ ] Requirement 3: Data handling expectation
- [ ] Requirement 4: Error handling behavior

### Non-Functional Requirements
- [ ] Performance: Response time < 2s
- [ ] Accessibility: WCAG AA compliance
- [ ] Browser support: Modern browsers (2 years)
- [ ] Mobile responsiveness: 320px+ width
```

## Technical Specifications

### For Frontend Tasks
```markdown
### Component Specifications
**File Location**: src/components/[ComponentName].svelte
**Component Type**: [Presentational/Container/Layout]

### Props Interface
```typescript
interface ComponentProps {
  required_prop: string
  optional_prop?: number
  data_prop: Array<DataType>
  callback_prop: (value: DataType) => void
}
```

### State Management
- Local state: [list reactive variables needed]
- Global state: [reference to stores if needed]
- Computed values: [reactive statements required]

### Events to Dispatch
- `event_name`: { payload_description }
- `another_event`: { payload_description }

### Styling Requirements
- Use existing component library: [Button/Input/Modal]
- Follow design system: [color scheme/spacing]
- Responsive breakpoints: [mobile/tablet/desktop specs]
```

### For Backend Tasks
```markdown
### API Specifications
**Endpoint**: [METHOD] /api/path/:param
**Authentication**: [Required/Optional/None]
**Rate Limiting**: [requests per minute]

### Request/Response Schema
```typescript
// Request
interface RequestBody {
  field1: string
  field2: number
  field3?: boolean
}

// Response  
interface ResponseBody {
  success: boolean
  data: DataType
  message?: string
}
```

### Database Operations
- Tables involved: [table1, table2]
- Query patterns: [SELECT/INSERT/UPDATE/DELETE]
- Transactions required: [yes/no]
- Indexes needed: [field names]

### Business Logic
- Validation rules: [list validation requirements]
- Error conditions: [when to return 400/404/500]
- Side effects: [notifications/logging/etc]
```

### For DevOps Tasks
```markdown
### Infrastructure Requirements
**Environment**: [Development/Staging/Production]
**Services**: [Database/CDN/Cache/Queue]
**Scaling**: [Expected load/capacity requirements]

### Configuration Changes
- Environment variables: [list new vars needed]
- Service configurations: [nginx/docker/k8s changes]
- Security updates: [firewall/SSL/auth changes]

### Deployment Strategy
- Deployment method: [blue-green/rolling/canary]
- Rollback plan: [how to revert if issues]
- Health checks: [monitoring/alerting setup]
```

## Success Criteria

```markdown
### Completion Checklist
- [ ] Core functionality works as specified
- [ ] All tests pass (unit + integration)
- [ ] Code follows project style guidelines
- [ ] Performance requirements met
- [ ] Error handling implemented
- [ ] Documentation updated
- [ ] Security review completed (if applicable)

### Acceptance Tests
1. **Test 1**: [Description of user scenario]
   - Given: [initial conditions]
   - When: [user action]
   - Then: [expected outcome]

2. **Test 2**: [Description of edge case]
   - Given: [edge condition]
   - When: [action taken]
   - Then: [expected handling]

### Integration Points
- [ ] Integrates correctly with [ComponentA]
- [ ] API contract matches frontend expectations
- [ ] Database changes applied successfully
- [ ] Deployment pipeline updated
```

## Resources and References

```markdown
### Documentation References
- Pattern guide: docs/vibe-coding/patterns/[relevant-pattern].md
- Component examples: docs/vibe-coding/examples/[similar-component]
- API standards: docs/project-specific/api-contracts.md

### Dependencies
**Requires completion of**:
- Task: [dependency-task-name] (Agent: [agent-name])
- Infrastructure: [required-setup] (Agent: DevOps)

**Blocks the following tasks**:
- Task: [dependent-task-name] (Agent: [agent-name])
- Feature: [feature-name] integration

### External Resources
- Design mockups: [link or attachment]
- API documentation: [swagger/postman collection]
- Database schema: [link to schema docs]
```

## Communication Protocol

```markdown
### Reporting Schedule
- **Initial acknowledgment**: Within 30 minutes
- **Progress updates**: Every 2 hours for complex tasks
- **Completion report**: Immediately upon completion
- **Blocker escalation**: Within 30 minutes of identification

### Escalation Triggers
- Unable to understand requirements after 30min analysis
- Technical blocker preventing progress for >1 hour
- Scope creep beyond original specifications
- Integration dependencies unavailable

### Response Format
Use the standard implementation report template:
- Status and duration
- Completed tasks and files modified
- Implementation approach and decisions
- Blockers and next steps
- Questions for clarification
```

## Example Handoffs

### Frontend Component Example
```markdown
## Task: UserCard Component
**Agent**: Junior Frontend
**Priority**: High
**Complexity**: Medium
**Estimated Duration**: 2 hours
**Token Budget**: 40K

### Business Context
Users need to view and edit their profile information. This card component will be reused across the dashboard, settings page, and user directory.

### Functional Requirements
- [ ] Display user avatar, name, email, and status
- [ ] Toggle between view and edit modes
- [ ] Save changes with validation
- [ ] Handle loading and error states

### Component Specifications
**File Location**: src/components/UserCard.svelte

### Props Interface
```typescript
interface UserCardProps {
  user: User
  editable?: boolean
  onSave: (user: User) => Promise<void>
}
```

### Success Criteria
- [ ] Component renders user data correctly
- [ ] Edit mode allows field modification
- [ ] Save triggers onSave callback with updated data
- [ ] Validation prevents invalid saves
- [ ] Loading spinner shows during save operation

### Resources
- Pattern guide: docs/vibe-coding/patterns/component-architecture.md
- Similar example: src/components/ProductCard.svelte
- Design mockup: [attached wireframe]
```

### Backend API Example
```markdown
## Task: User Profile API
**Agent**: Junior Backend
**Priority**: High  
**Complexity**: Medium
**Estimated Duration**: 3 hours
**Token Budget**: 35K

### Business Context
Frontend components need CRUD operations for user profile management. This API will serve the UserCard component and profile settings page.

### API Specifications
**Endpoints**:
- GET /api/users/:id - Fetch user profile
- PUT /api/users/:id - Update user profile
- POST /api/users/:id/avatar - Upload avatar image

### Request/Response Schema
```typescript
// PUT /api/users/:id
interface UpdateUserRequest {
  name?: string
  email?: string
  bio?: string
}

interface UserResponse {
  id: string
  name: string
  email: string
  bio: string
  avatar_url: string
  created_at: string
  updated_at: string
}
```

### Success Criteria
- [ ] All endpoints return correct status codes
- [ ] Request validation prevents invalid data
- [ ] Database updates applied atomically
- [ ] Avatar upload handles file size limits
- [ ] API documentation updated

### Resources
- API patterns: docs/project-specific/api-contracts.md
- Database schema: docs/project-specific/database-schema.md
- Similar implementation: src/api/products.js
```

### DevOps Infrastructure Example
```markdown
## Task: CDN Setup for User Avatars
**Agent**: DevOps
**Priority**: Medium
**Complexity**: Simple
**Estimated Duration**: 1 hour  
**Token Budget**: 30K

### Business Context
User avatar uploads need fast, global delivery. CDN will improve page load times and reduce server bandwidth usage.

### Infrastructure Requirements
**Service**: CloudFlare CDN
**Storage**: AWS S3 bucket for avatar images
**Cache**: 24 hour TTL for avatar images

### Configuration Changes
- Environment variables: CDN_URL, S3_AVATAR_BUCKET
- Upload service: Configure S3 upload permissions
- CDN: Point to S3 bucket with appropriate headers

### Success Criteria
- [ ] Avatar uploads store in S3 bucket
- [ ] CDN serves images with correct headers
- [ ] Cache invalidation works for updated avatars
- [ ] Monitoring alerts for upload failures

### Resources
- Infrastructure patterns: docs/vibe-coding/patterns/infrastructure.md
- AWS configuration: docs/project-specific/aws-setup.md
- Similar setup: CDN configuration for product images
```