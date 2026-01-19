---
description: Generate all foundational documents (PRD, spec, tests, tasks) in one go
agent: architect
---

# Generate Complete Foundational Documents

You are generating ALL foundational documents for a TDD project in a single session.

**Alternative:** For iterative document creation with review between each step, use individual commands:
- `/tdd/prd` → Review → `/tdd/spec` → Review → `/tdd/test-spec` → Review → `/tdd/agent-spec` → Review → `/tdd/tasks`

## Phase 0: Requirements Gathering

Before generating any documents, ask the user these questions:

1. **Core Problem**: What specific problem does this solve? Who experiences it?

2. **Users**: Who are the primary users? Secondary users?

3. **Key Features**: What are the 3-5 must-have features for v1?

4. **Technical Constraints**: 
   - Required tech stack (language, framework, database)?
   - Integration requirements?
   - Performance requirements?

5. **Scope Boundaries**: What is explicitly OUT of scope for v1?

**Wait for the user to answer before proceeding.**

## Phase 1: Generate Documents

After gathering requirements, generate these documents in order:

### 1. PRD (.context/prd.md)
- Minimum 300 lines (target 400-700)
- Complete user personas
- All features with acceptance criteria
- No placeholders

### 2. Technical Spec (.context/spec.md)
- **Minimum 1200 lines (target 1500-3000+)**
- **EVERY implementation detail documented**
- Complete database schemas (CREATE TABLE for EVERY table)
- Full API specifications with TypeScript types (EVERY endpoint)
- Architecture diagrams
- File structure
- **No "..." or "similar to above" shortcuts**

### 3. Test Spec (.context/test.md)
- Minimum 500 lines (target 700-1200)
- Complete, runnable test examples (not pseudocode)
- Unit, integration, E2E tests
- Test helpers and fixtures

### 4. Agent Spec (.context/agent-spec.md)
- Minimum 150 lines (target 200-300)
- Abstract principles only
- Patterns and conventions
- No specific code paths

### 5. Tasks (tasks/TDD_*.md)
- Create individual task files
- Each with full frontmatter (test_scope, existing_code_context)
- Logical dependency order
- Minimum 10 tasks for a typical project

## Phase 2: Validation

After generating, validate:
- [ ] All documents meet minimum line counts
- [ ] No TBD/TODO placeholders
- [ ] All code examples are complete
- [ ] Tasks have proper test_scope
- [ ] Dependencies are logical

## User Input

$INPUT
