# Documentation Guidelines

This doc defines AI behavior and documentation expectations. It is project-agnostic and intended as a base for future projects. **We keep the AI instructions and general structure;** you fill in project-specific content in the designated layers.

## Documentation expectation

- For every feature/flow change, update the relevant doc in `docs/03-architecture/` or `docs/04-behavior/` as appropriate, add a link in `docs/03-architecture/SYSTEM_MAP.md`, and capture a brief note using `docs/07-guides/templates/FEATURE_NOTE.md` (code touchpoints, envs, UI impacts, ops). Do this before considering the work "done."
- Feature requests live in the GitHub Project (label `coordination:feature-request`); spec in item body. They desync across worktrees if stored in docs.

When you apply this base to a new project, you create layers 2–5 and these docs; then this expectation applies. The structure and instructions stay the same; the content is project-specific.

## AI Agent Behavior Rules

### General Guidelines
- Be surgical. Change only what is requested.
- Avoid drive-by refactors.
- Preserve authentication, session, and user role semantics unless explicitly instructed.
- Keep frontend UX unchanged unless a change is requested.
- Use existing storage mechanisms.
- Do not introduce new storage layers without approval.
- Preserve copy and branding unless asked to change it.

### Code Quality
- Keep file sizes manageable - split large files when appropriate
- Use TypeScript types consistently
- Follow existing patterns in the codebase
- Maintain separation of concerns (backend routes, frontend components, shared types)

### Security
- Never expose sensitive data in logs or error messages
- Always validate user permissions before allowing actions
- Use parameterized queries for database operations
- Follow OWASP best practices for authentication and authorization

### Testing
- Test authentication flows thoroughly
- Verify role-based access control works correctly
- Test API endpoints with different user roles
- Verify data validation and error handling

### Documentation
- Update relevant documentation when making changes
- Document API endpoints, database changes, and UI impacts
- Keep feature notes up to date
- Update system map when adding new features
