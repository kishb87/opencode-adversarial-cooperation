# Multi-File Specification Structure

**Date**: 2026-01-18
**Problem**: Single spec.md file (1500-3000+ lines) causes LLM timeouts
**Solution**: Split spec into multiple focused files (200-800 lines each)

---

## Why Multi-File?

### Problem with Single File
```
Architect writing spec.md:
  ↓
Writes 400 lines... 800 lines... 1200 lines...
  ↓
Tool call timeout at ~1500 lines
  ↓
Partial spec generated, missing critical sections
  ↓
Can't recover without rewriting everything
```

### Solution with Multiple Files
```
Architect writing specs:
  ↓
Writes README.md (200 lines) ✅ Complete
Writes architecture.md (350 lines) ✅ Complete
Writes database.md (400 lines) ✅ Complete
Writes api.md (600 lines) ✅ Complete
  ↓
Each file completes successfully
  ↓
Total: 2000+ comprehensive lines across multiple files
```

**Benefits**:
- ✅ No timeouts (each file is manageable size)
- ✅ Restart-friendly (if one file fails, don't lose everything)
- ✅ Better organization (easier to navigate)
- ✅ Modular updates (update just API section without rewriting entire spec)
- ✅ Parallel potential (could eventually write sections concurrently)
- ✅ Real-world pattern (how large specs are actually organized)

---

## File Structure

```
.context/
├── prd.md                      # Product Requirements
├── research.md                 # Library research findings
├── spec/                       # Technical Specification (MULTI-FILE)
│   ├── README.md              # Index, overview, quick reference (150-250 lines)
│   ├── architecture.md        # System design, tech stack (200-400 lines)
│   ├── database.md            # Schemas, migrations (200-500 lines)
│   ├── api.md                 # All endpoints (400-800 lines)
│   ├── types.md               # TypeScript interfaces (200-400 lines)
│   ├── security.md            # Auth, security (200-400 lines)
│   ├── file-structure.md      # Project organization (150-250 lines)
│   ├── error-handling.md      # Errors, validation (200-300 lines)
│   ├── performance.md         # Optimization (150-250 lines) [optional]
│   └── deployment.md          # Infrastructure, CI/CD (150-250 lines) [optional]
├── test.md                     # Test Specification
└── agent-spec.md               # Agent Principles
```

**Total spec lines**: 2000-3500+ across all files

---

## File Breakdown

### 1. README.md (Index)
**Purpose**: Entry point, table of contents, quick reference
**Size**: 150-250 lines

**Contains**:
- Project overview
- Table of contents with links to other files
- Quick reference section (tech stack, DB, API base, auth method)
- Link to research.md
- Document status checklist

**Example**:
```markdown
# Technical Specification

**Project**: REST API with Real-time Features
**Tech Stack**: Fastify, Drizzle, PostgreSQL, Socket.IO

## Table of Contents
1. [Architecture](./architecture.md)
2. [Database](./database.md)
3. [API](./api.md)
...

## Quick Reference
**Database**: PostgreSQL 16
**Auth**: JWT tokens
**API Base**: /api/v1
```

### 2. architecture.md (System Design)
**Purpose**: High-level system design, tech stack decisions
**Size**: 200-400 lines

**Contains**:
- System architecture diagram (ASCII or Mermaid)
- Complete technology stack with rationale from research.md
- Integration patterns (how libraries work together)
- Architecture principles
- Data flow diagrams

**Example sections**:
- System Architecture (diagram)
- Technology Stack (all choices with research references)
- Integration Patterns (Fastify + Drizzle setup code)
- Architecture Principles (from agent-spec.md)

### 3. database.md (Schemas)
**Purpose**: Complete database schema definitions
**Size**: 200-500 lines

**Contains**:
- Entity Relationship Diagram (ERD)
- Complete CREATE TABLE for EVERY table
- Every column with type, constraints, comments
- Every index
- Every foreign key relationship
- Migration strategy

**Critical**: No shortcuts like "and 5 more tables" - write them ALL

### 4. api.md (Endpoints)
**Purpose**: Complete API specification
**Size**: 400-800 lines (largest file typically)

**Contains**:
- Base URL and versioning
- Authentication requirements
- EVERY endpoint with:
  - Description
  - Full request type (all fields documented)
  - Full response type (all fields documented)
  - Validation rules
  - All error codes
  - Example request
  - Example response

**Critical**: Most likely to cause timeout in single-file approach, so this file is most important to split out

### 5. types.md (TypeScript Definitions)
**Purpose**: All TypeScript interfaces, types, enums
**Size**: 200-400 lines

**Contains**:
- All interfaces (every field documented)
- All type aliases
- All enums
- Validation schemas (Zod, Yup, etc.)

**No shortcuts**: Every field must be listed, no "extends BaseType" without showing what BaseType contains

### 6. security.md (Auth & Security)
**Purpose**: Authentication, authorization, security practices
**Size**: 200-400 lines

**Contains**:
- Authentication flow (diagrams + code)
- JWT token configuration
- Password hashing (bcrypt configuration from research)
- Authorization model (roles, permissions)
- Security best practices from research.md
- CORS configuration
- Rate limiting
- Input sanitization

### 7. file-structure.md (Project Organization)
**Purpose**: Complete project file/folder structure
**Size**: 150-250 lines

**Contains**:
- Complete directory tree
- Purpose for EVERY file and folder
- Module organization patterns
- Naming conventions

### 8. error-handling.md (Errors)
**Purpose**: Error codes, validation, error responses
**Size**: 200-300 lines

**Contains**:
- ALL error codes enumerated (not "errors 001-050")
- Error response format
- Validation approach
- Error handling middleware
- Logging strategy

### 9. performance.md (Optional)
**Purpose**: Optimization and scaling
**Size**: 150-250 lines

**Contains**:
- Caching strategy
- Database optimization (indexes, queries)
- Connection pooling
- Load balancing
- CDN setup (if applicable)

### 10. deployment.md (Optional)
**Purpose**: Infrastructure and CI/CD
**Size**: 150-250 lines

**Contains**:
- Deployment architecture
- CI/CD pipeline
- Environment configuration
- Monitoring and logging
- Backup strategy

---

## How Architect Generates

### Writing Order

The Architect writes files in this specific order:

1. **README.md** (index first)
2. **architecture.md** (high-level design)
3. **database.md** (data model)
4. **api.md** (endpoints)
5. **types.md** (type definitions)
6. **security.md** (auth and security)
7. **file-structure.md** (project organization)
8. **error-handling.md** (errors)
9. **performance.md** (if applicable)
10. **deployment.md** (if applicable)

**Each file is written completely before moving to the next.**

### Example Architect Process

```
User: /tdd/spec "Build REST API with authentication"
  ↓
@architect:
  1. Does research (creates research.md)
  2. Creates .context/spec/ directory
  3. Writes README.md
     - Table of contents
     - Quick reference
     - Links to other files
  4. Writes architecture.md
     - System diagram
     - Tech stack from research
     - Integration patterns
  5. Writes database.md
     - ERD
     - users table (complete CREATE TABLE)
     - sessions table (complete CREATE TABLE)
     - All indexes
  6. Writes api.md
     - POST /api/auth/register (complete spec)
     - POST /api/auth/login (complete spec)
     - POST /api/auth/logout (complete spec)
     - [All other endpoints]
  7. Writes types.md
     - User interface (all fields)
     - Session interface (all fields)
     - Request/response types
  8. Writes security.md
     - JWT configuration
     - bcrypt setup
     - Auth flow
  9. Writes file-structure.md
     - Complete directory tree
  10. Writes error-handling.md
      - All error codes
      - Error response format

  ✅ Complete - no timeouts!
```

---

## How Agents Reference Specs

### Actor References

When Actor implements a task, they can reference specific files:

```markdown
## Task: Implement User Registration

### Relevant Specs
@.context/spec/database.md#users - User table schema
@.context/spec/api.md#post-apiauthregister - Registration endpoint
@.context/spec/types.md#User - User interface
@.context/spec/security.md#password-hashing - bcrypt setup
```

**Benefit**: Actor only loads relevant sections, not entire 3000-line spec

### Critic References

When Critic validates, they reference specific sections:

```markdown
## Validation Context

Checking implementation against:
- @.context/spec/api.md#post-apiauthregister - Expected API contract
- @.context/spec/security.md#password-hashing - Security requirements
```

**Benefit**: Focused validation against specific requirements

---

## Migration Guide

### Old Single-File Approach

```
.context/
├── spec.md  (1500-3000 lines, causes timeouts)
```

### New Multi-File Approach

```
.context/
├── spec/
│   ├── README.md
│   ├── architecture.md
│   ├── database.md
│   ├── api.md
│   ├── types.md
│   ├── security.md
│   ├── file-structure.md
│   └── error-handling.md
```

### For Existing Projects

If you have an existing `spec.md`, you can:

1. Run `/tdd/spec` again - Architect will create spec/ folder
2. Manually split spec.md into appropriate files
3. Or keep spec.md and create spec/ for new work (both can coexist)

**Actor/Critic work with both formats** - they can read `@.context/spec.md` or `@.context/spec/api.md`

---

## Benefits Summary

| Aspect | Single File | Multi-File |
|--------|-------------|------------|
| **Timeouts** | ❌ Frequent at 1500+ lines | ✅ None (each file ~200-800) |
| **Recovery** | ❌ Lose everything on timeout | ✅ Keep completed files |
| **Organization** | ❌ Hard to navigate 3000 lines | ✅ Clear sections |
| **Updates** | ❌ Rewrite entire file | ✅ Update just one section |
| **References** | ❌ Load entire spec | ✅ Load specific section |
| **Parallel** | ❌ Must write sequentially | ✅ Could parallelize (future) |
| **Real-world** | ❌ Not how specs are organized | ✅ Industry standard pattern |

---

## Line Count Distribution

Typical project (REST API with auth):

```
README.md          :  200 lines (index)
architecture.md    :  300 lines (tech stack + diagrams)
database.md        :  400 lines (5 tables with indexes)
api.md             :  700 lines (12 endpoints fully documented)
types.md           :  300 lines (interfaces for all entities)
security.md        :  350 lines (auth flow + JWT + bcrypt)
file-structure.md  :  200 lines (complete project tree)
error-handling.md  :  250 lines (30 error codes)
─────────────────────────────────────────────────────
Total              : 2700 lines (comprehensive spec)
```

**Before**: Would timeout at ~1500 lines
**After**: All 2700 lines generated successfully

---

## Summary

The multi-file spec structure solves the timeout problem by:

1. **Preventing timeouts**: Each file is small enough to write in one call
2. **Better organization**: Logical separation by concern
3. **Modular updates**: Change one section without affecting others
4. **Easier navigation**: Jump to specific section via README
5. **Real-world pattern**: How professional specs are organized

**Result**: Comprehensive, complete technical specifications without LLM timeouts.
