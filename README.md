# PROG6212-Raceday-assignment-part2
## Project structure
```
RaceDay.Api/     Controllers, Models, DTOs, Data (DbContext), Services (JWT + password hashing), Program.cs
RaceDay.Tests/   xUnit tests (controller unit tests, role-attribute tests, full HTTP integration tests)
.github/workflows/ci.yml   Builds and runs all tests on every push
```

## Differences from the Part 1 design (update your ERD/SQL to match)
Part 1 only had IDs and names, so the schema had to be extended to support login and the Part 2 requirements:
* `Participants` + `Organizers` are merged into one **Users** table with a `Role` column, `Email` and `PasswordHash`.
* **Events** now has Name, Description, Date, Location, DistanceKm, Type (Run/Walk/Cycle) and an OrganiserId (it no longer holds the participant).
* **Categories** belong to an event.
* **Enrolments** (new) link User + Event + Category and hold the BibNumber.
* **Results** (new, replaces Event_results) store FinishTime and Position per enrolment.
* `Route` and `Performance_History` from Part 1 are not used by the Part 2 endpoints. Performance history can be read from Results.

## Testing
The suite covers registration/login, password hashing, token contents, profile, events, categories, enrolments,
results, and role rules (success and failure cases). `ApiIntegrationTests` starts the real API so 401 (no token)
and 403 (wrong role) are checked through the real authentication pipeline.
