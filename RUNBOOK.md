# Runbook — SupportEngineerChallenge

> Update this file as part of the exercise.

## Service overview
- **Service:** SupportEngineerChallenge.Api
- **Purpose:** Minimal task tracker (create + list tasks)
- **Tech Stack**: .NET 8, EF Core, SQLite
- **Data store:** SQLite (`app.db` in the API working directory)

## Common commands

**Run locally**
```bash
cd src/SupportEngineerChallenge.Api
dotnet run
```

**Run tests**
```bash
dotnet test
```

## Key endpoints
- `GET /api/tasks?userId={id}&limit={n}`
- `POST /api/tasks`

## Known Issues & Fixes
1. Create Task Fails with 500 (Timestamp Parsing) Symptoms: 500 error, FormatException in logs
Root Cause: DateTime.Parse used on missing header
Fix: Use DateTime.TryParse with fallback to UtcNow
Verification: POST without header succeeds and Invalid timestamp handled gracefully

2. Test Project Fails to Build Symptoms: FluentAssertions missing error
Fix: Add FluentAssertions package
Verification: dotnet test passes

3. Slow Task Listing
Root Cause: In-memory filtering
Fix:Use EF query with Where + Take + ToListAsync
Verification: Reduced latency

4. Invalid Request Payload
Fix:Add null + whitespace validation

Troubleshooting
Create task fails: Check logs for timestamp errors
Tasks slow: Check query and logs
Tests failing: Run restore and test again

Verification Checklist
- POST works without timestamp
- No crashes on invalid input
- GET filters correctly
- Tests pass

Rollback
- Roll back to last known good version.
- Add guardrails (e.g. input validation, error handling) to prevent unhandled exceptions.

Operational Recommendations
- Add structured logging
- Monitor latency and errors
- Add Swagger documentation