# Follow-up Ticket (Fill in)

**Title:**  Improve Task API resilience, validation standards, and observability
**Priority:** (P1)
**Owner:**  Aderonke Adewuyi

## Description
What should be improved after the immediate incident is resolved?

> After resolving the immediate runtime and test failures in the Task API, several improvements remain to ensure long-term reliability, maintainability, and production readiness. This ticket focuses on strengthening validation patterns, improving observability, and preventing similar issues through better safeguards and standards.

Key areas of improvement:

- [ ] Standardize input validation across all endpoints
- [ ] Improve timestamp handling and enforce consistent formats
- [ ] Add structured error responses
- [ ] Introduce better logging and monitoring for invalid requests
- [ ] Expand automated test coverage for edge cases

## Acceptance criteria
- [ ] All API endpoints validate inputs using a consistent pattern (e.g., centralized validation or middleware)
- [ ] Timestamp handling enforces ISO 8601 format and rejects invalid formats with clear errors
- [ ] API returns structured error responses (e.g., { code, message, details })
- [ ] Logging includes clear warnings for invalid inputs without exposing sensitive data
- [ ] Add unit/integration tests for: Missing headers, Invalid timestamps and Null or malformed request bodies
- [ ] Performance baseline documented for /api/tasks endpoint
- [ ] No in-memory filtering for database-backed endpoints

## Notes / context
- [ ] Links to relevant code/areas
    - TaskEndpoints.cs
    - CreateTaskRequest model
    - EF Core query logic in Task API
    - Test project: SupportEngineerChallenge.Tests
    
- [ ]Any monitoring/alerting suggestions
    - Add alert for spikes in: 
        - 400 responses (bad requests)
        - Invalid timestamp warnings

- [ ] Log aggregation for: "Invalid or missing X-Client-Timestamp"