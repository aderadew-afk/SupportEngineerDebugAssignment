INCIDENT REPORT — SupportEngineerChallenge API

## ISSUE 1 — Missing Tool (FluentAssertions)

# What happened/ Error
The project tried to use a testing tool called FluentAssertions, but it was not installed.
/Users/aderonkeadewuyi/SupportEngineerDebugAssignment/tests/SupportEngineerChallenge.Tests/TaskApiTests.cs(4,7): error CS0246: The type or namespace name 'FluentAssertions' could not be found (are you missing a using directive or an assembly reference?) [/Users/aderonkeadewuyi/SupportEngineerDebugAssignment/tests/SupportEngineerChallenge.Tests/SupportEngineerChallenge.Tests.csproj]

# Root Cause
The test project referenced FluentAssertions, but the NuGet package was missing.

# Fix
> I installed the missing FluentAssertions package in the test project.

# Result
Tests now run successfully and the build works again.

## ISSUE 2 — App Crashed When Date Was Missing

# What happened/ Error
The app tried to read a date using DateTime.Parse, but sometimes the value was empty or missing.
fail: Microsoft.AspNetCore.Server.Kestrel[13] Connection id "0HN...", Request id "0HN...:00000001": An unhandled exception was thrown by the application. System.FormatException: String '' was not recognized as a valid DateTime.at System.DateTime.Parse(String s) At SupportEngineerChallenge.Api.Endpoints.TaskEndpoints.<>c.AnonymousMethod__1(HttpContext ctx, CreateTaskRequest req, AppDbContext db, ILogger`1 logger) in .../TaskEndpoints.cs:line 44

# Root Cause
DateTime.Parse was used without checking if the input was valid.
DateTime createdAt = DateTime.Parse(clientTimestamp);

# Fix
> Replaced DateTime.Parse with DateTime.TryParse and added a fallback to DateTime.UtcNow.
> DateTime createdAt;
> if (!DateTime.TryParse(clientTimestamp, out createdAt))
> {
>    logger.LogWarning("Invalid or missing X-Client-Timestamp: {Value}", clientTimestamp);
>    createdAt = DateTime.UtcNow;
>}

# Result
The app no longer crashes when the timestamp is missing or invalid.

## ISSUE 3 — Missing Input Checks (Null Request)

# What happened
If the request body is missing, invalid JSON or wrong content-type, then ASP.NET will try to read from the invalid req, and this line throws: NullReferenceException

# Root Cause
No validation existed for missing or null request data.
if (string.IsNullOrWhiteSpace(req.UserId) || string.IsNullOrWhiteSpace(req.Title))
               return Results.BadRequest(new { message = "userId and title are required" });

# Fix
> Added a check to return a Bad Request response when required fields are missing.
> if (req is null || string.IsNullOrWhiteSpace(req.UserId) || string.IsNullOrWhiteSpace(req.Title))
>           {
>               return Results.BadRequest(new { message = "userId and title are required" });
>           }

# Result
The API now safely rejects bad input instead of crashing.

## FINAL CONCLUSION
We fixed three main problems:
Added missing testing tools
Made date handling safe
Added input validation