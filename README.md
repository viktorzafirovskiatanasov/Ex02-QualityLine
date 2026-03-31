<img width="747" height="522" alt="image" src="https://github.com/user-attachments/assets/fd289893-0b90-4050-a77e-d81602ae5b7a" /># Ex02 — Personal Details WinForms App

Overview
- Windows Forms application targeting .NET Framework 4.7.2 that captures and persists personal details to a database.
- Provides a simple UI for entering and viewing records and a small data-access layer for persistence.

Repository layout (important files)
- `Ex02.sln` — solution file
- `Ex02/` (project)
  - `Form1.cs` — main form logic (event handlers, validation, UI → DB flow)
  - `Form1.Designer.cs`, `Form1.resx` — generated UI layout and resources
  - `DbPersonalDetails.cs` — model class representing a personal-details record
  - `DbConnectionManager.cs` — database connection and data-access logic

What the application does (high level)
- The user enters personal details into the form on `Form1`.
- `Form1` creates a `DbPersonalDetails` instance populated from the form controls.
- `Form1` calls methods in `DbConnectionManager` to save or query records.
- `DbConnectionManager` opens a DB connection, executes SQL statements, and maps results to `DbPersonalDetails` objects.

Prerequisites
- Windows
- Visual Studio Community 2026 (or 2019/2022/2023) with the .NET Desktop Development workload
- .NET Framework 4.7.2
- A SQL database (SQL Server LocalDB, SQL Server, or SQLite). Update the connection string as described below.

Build and run
1. Open the solution in Visual Studio: `C:\Users\User\source\repos\Ex02\Src\Ex02.sln` (path may vary).
2. Restore NuGet packages if prompted.
3. Configure the database connection (see Database configuration).
4. Build the solution (Build → Build Solution).
5. Run the project (F5 or Debug → Start Debugging).

Database configuration
- Locate the DB connection logic in `DbConnectionManager.cs`. The project may currently use a hardcoded connection string or read from `App.config`.
- Recommended approach: add the connection string to `App.config` and read it using `ConfigurationManager.ConnectionStrings["DefaultConnection"].ConnectionString`.

Example SQL Server LocalDB connection string (for `App.config`):

```xml
<connectionStrings>
  <add name="DefaultConnection"
       connectionString="Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=Ex02Db;Integrated Security=True;"
       providerName="System.Data.SqlClient" />
</connectionStrings>
```

Example SQLite connection string:

```
Data Source=ex02.db;Version=3;
```

Database schema suggestion
- Table: `PersonalDetails`
  - `Id` INT IDENTITY PRIMARY KEY
  - `FirstName` NVARCHAR(100)
  - `LastName` NVARCHAR(100)
  - `Email` NVARCHAR(200)
  - `Phone` NVARCHAR(50)
  - `Address` NVARCHAR(500)
  - `DateOfBirth` DATE

Adjust the schema to match the properties defined in `DbPersonalDetails.cs`.

Where to make common changes
- UI layout and controls: `Form1.Designer.cs` (use the WinForms designer when possible).
- Form logic and event handlers: `Form1.cs`.
- Model fields and types: `DbPersonalDetails.cs`.
- Connection strings and SQL: `DbConnectionManager.cs` (or `App.config` if you externalize the string).

Recommendations and improvements
- Use parameterized queries or an ORM (Dapper, Entity Framework) to prevent SQL injection.
- Move the connection string to `App.config` and avoid hardcoding credentials.
- Make DB calls asynchronous (`async`/`await`) to keep the UI responsive.
- Introduce an interface (for example, `IPersonalDetailsRepository`) and dependency injection to make data access testable.
- Add input validation on `Form1` (required fields, email and phone format checks).
- Add unit tests for repository logic and any testable form logic.

Troubleshooting
- Connection errors: verify the connection string and that the database server is running.
- SQL exceptions: log or debug `DbConnectionManager` to inspect the SQL and parameters being executed.
- UI freezes during DB operations: move blocking work off the UI thread (use `Task.Run` or fully async methods).

Git / GitHub notes
- Add a Visual Studio `.gitignore` (exclude `bin/`, `obj/`, `.vs/`, `*.user`, and build artifacts).
- Use clear commit messages like "Add DbConnectionManager" or "Implement save functionality".
- Use `main` for stable code and `feature/<name>` branches for work in progress.

License
- Add a `LICENSE` file if required by your course or institution (MIT or Apache-2.0 are common choices).

If you want, I can:
- Update this README with exact property names and SQL examples after inspecting `DbPersonalDetails.cs` and `DbConnectionManager.cs`.
- Move a hardcoded connection string into `App.config` and update the code to read it.

