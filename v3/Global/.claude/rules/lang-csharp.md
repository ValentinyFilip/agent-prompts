---
paths:
  - "**/*.cs"
  - "**/*.csproj"
  - "**/*.razor"
  - "**/Directory.Build.props"
---

# C# and .NET Coding Rules

These rules apply when I read or change C# and .NET files. Claude Code loads this file
only for the paths in the frontmatter. Do not import this file from `CLAUDE.md`.

The conventions come from the .NET projects in `C:/data/projects`, reviewed in
October 2026.

## 1. Precedence

1. If a `.steering/` tech file of the target sub-project gives a different rule, obey
   the steering file.
2. If `.editorconfig` or an analyzer rule gives a different rule, obey that rule.
3. In an existing file that uses a different pattern, match that file. Do not
   reformat code that you do not change.
4. In a new file or a new project, use the conventions in this file.

## 2. Platform and Project

1. Target `net10.0`. Set `Nullable` and `ImplicitUsings` to `enable`.
2. Do not add a `GlobalUsings.cs` file.
3. Do not add `!` to hide a nullable warning.
4. Do not add a `#pragma warning disable` without a comment that gives the reason.
5. Do not add a NuGet package without written permission of the user.
6. If the repository uses `Directory.Packages.props`, put each package version there.

## 3. Structure

1. Use vertical slices: `Features/<Area>/<UseCase>/`.
2. Use FastEndpoints for HTTP endpoints. Do not add MVC controllers.
3. Do not add MediatR, AutoMapper, or Mapster. Map objects by hand.
4. Register services in an `AddXxx(this IServiceCollection services)` extension method.
5. Use constructor injection. Do not use a service locator.
6. Read settings with `IOptions<T>`.
7. Do not add an interface that has only one implementation, unless a test needs it.
8. Put one type in each file. Small related records can share one file.
9. Give the file the same name as its main type.

## 4. Naming

1. Start each interface name with `I`.
2. Name private fields `_camelCase`.
3. Name constants in `PascalCase`. Do not use `UPPER_SNAKE`.
4. Give each async method the `Async` suffix. A framework override keeps the
   framework name.
5. Name each custom exception `<Name>Exception`. Put it in `Domain/Exceptions/` or
   `Common/Exceptions/`.
6. Name each test project `<Project>.Tests`.

## 5. Language Features

1. Use file-scoped namespaces.
2. Use `var` for local variables.
3. Use primary constructors for classes that get injected services.
4. Make each class `sealed`, unless another class inherits from it.
5. Use `record` for DTOs, requests, responses, commands, and messages.
6. Use `required` and `init` for properties that are set one time.
7. Use `is null` and `is not null`. Do not use `== null` or `!= null`.
8. Use collection expressions, for example `[]`. Use target-typed `new()` when the
   type is clear.
9. In a public signature, use `IReadOnlyList<T>` or `IReadOnlyCollection<T>`. Do not
   expose `List<T>`.
10. Use LINQ method syntax. Do not use LINQ query syntax.
11. Use `$""` string interpolation. Use `string.Format` only for a resource format
    string.
12. Use `ArgumentNullException.ThrowIfNull` and
    `ArgumentException.ThrowIfNullOrWhiteSpace` for guard clauses. Do not add a guard
    library.
13. Do not use `#region`.
14. Write `/// <summary>` comments only on the public API of a library or an SDK.

## 6. Async Code

1. Pass the `CancellationToken` through every async call that accepts it.
2. Do not use `.Result`, `.Wait()`, or `async void`. Event handlers are the exception.
3. Do not add `ConfigureAwait(false)` in ASP.NET Core application code.

## 7. Errors

1. Use one global handler: an `IExceptionHandler` with `AddProblemDetails()`. Return
   `ProblemDetails` to the client.
2. Do not catch an exception and ignore it. Do not write an empty `catch` block.
3. Catch the most specific exception type that you can handle.
4. If you catch `Exception`, log it, then rethrow it or return a failure result.

## 8. Monitoring: Logs, Metrics, and Traces

### 8.1 Setup

1. Configure monitoring one time at startup, in one extension method on
   `IHostApplicationBuilder`. A second call duplicates the telemetry.
2. Bind the settings from the `Monitoring` configuration section: `ServiceName`,
   `OtlpEndpoint`, and `OtlpToken`. If `ServiceName` is empty, use the application
   name.
3. For logs, use Serilog with `AddSerilog`. Read the configuration, call
   `Enrich.FromLogContext()`, and write to the console.
4. Set the minimum log level to `Debug`. Override `System` and `Microsoft` to
   `Warning`.
5. For traces and metrics, call `AddOpenTelemetry()` and set the service name as the
   resource.
6. Add the ASP.NET Core and `HttpClient` instrumentation to traces and to metrics.
7. Add other instrumentation, for example the SQL client instrumentation, only in the
   service that needs it.
8. Export logs, traces, and metrics through OTLP over HTTP/protobuf, with a bearer
   token. Send logs with `Serilog.Sinks.OpenTelemetry`.
9. If `OtlpEndpoint` or `OtlpToken` is empty, do not register the OTLP exporters.
10. Keep `OtlpToken` in configuration or a secret store. Do not write it in code.
11. Do not add Application Insights, Prometheus, Jaeger, or Zipkin.
12. In an existing service with a different setup, keep that setup. Do not migrate it
    without a request of the user.

### 8.2 Logs

1. Use `ILogger<T>` through constructor injection.
2. Use message templates with `PascalCase` placeholders, for example
   `logger.LogInformation("Order {OrderId} paid", orderId)`. Do not use string
   interpolation in a log call.
3. In a hot path, use the `[LoggerMessage]` source generator.
4. Use `logger.BeginScope` to add shared context, for example an entity ID, to a group
   of log entries.
5. Use `Information` for business events, `Warning` for problems that the code
   recovers from, and `Error` for failures.
6. Log an exception one time, at the layer that handles it.
7. Do not log secrets, tokens, passwords, or personal data.

### 8.3 Metrics

1. Put the `Meter` and the instruments of a feature area in one `<Area>Telemetry`
   class, for example `SyncTelemetry`.
2. Name each meter `<ApplicationName>.<Area>`. The setup registers `<ApplicationName>`
   and `<ApplicationName>.*`. Register a meter with a different name with `AddMeter`.
3. Use `Counter<T>` for counts. Use `Histogram<T>` for durations and sizes.
4. Name instruments in lowercase dot notation, per the OpenTelemetry conventions, for
   example `sync.items.processed`.
5. Do not use a tag with many unique values, for example a user ID or a request ID.

### 8.4 Traces

1. Put the `ActivitySource` of a feature area in the same `<Area>Telemetry` class.
2. Name each source `<ApplicationName>.<Area>`. The setup registers `<ApplicationName>`
   and `<ApplicationName>.*`. Register a source with a different name with `AddSource`.
3. Start an activity for a business operation, a background job, or an outbound call
   that the automatic instrumentation does not cover.
4. Write `using var activity = <Area>Telemetry.Source.StartActivity("<Name>")`.
5. `StartActivity` can return `null`. Use `activity?.SetTag(...)`.
6. If the operation fails, call `activity?.SetStatus(ActivityStatusCode.Error)`.
7. Do not add custom correlation ID middleware. The W3C trace context of
   OpenTelemetry gives the correlation.

### 8.5 Health Checks

1. Map health checks to `/health` with `MapHealthChecks`.
2. Add a check for each database and for each required external dependency.

## 9. Data Access

1. Inject the `DbContext` directly into the endpoint or the handler. Use a repository
   only for a complex aggregate.
2. For a read-only query, use `AsNoTracking()`.
3. Filter in the query. Do not load a full table into memory to filter it.
4. For a bulk change, use `ExecuteUpdateAsync` or `ExecuteDeleteAsync`, unless the
   tests use the InMemory provider.
5. Configure each entity in an `IEntityTypeConfiguration<T>` class named
   `<Entity>Configuration`. Call `ApplyConfigurationsFromAssembly` in `OnModelCreating`.
6. Name each context `<Name>DbContext`.
7. Keep migrations in `Infrastructure/Persistence/Migrations/`.
8. For a change to a migration or to SQL, also obey `lang-sql.md`.

## 10. Tests

1. Use xUnit v3 in a new test project. In an existing project, keep its xUnit version.
2. Use the built-in `Assert` class. Do not add an assertion library.
3. Use NSubstitute in a new test project. In an existing project, keep its mocking
   library.
4. Name test methods `Method_Condition_Expected`.

## 11. Verification

1. After a change, run `dotnet build` for the changed project.
2. If tests exist for the changed code, run `dotnet test` for that test project.
