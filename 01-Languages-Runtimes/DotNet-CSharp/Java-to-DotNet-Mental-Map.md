---
title: "Java to .NET Mental Map"
aliases: [Java vs C#, Spring vs ASP.NET Core, Bảng ánh xạ Java .NET]
type: comparison
status: draft
tags: [dotnet, csharp, java, spring, comparison, moc]
topics: ["Mental Model Mapping"]
java_equivalent: "(note tổng hợp — chính nó là bảng ánh xạ)"
related: ["[[Clean-Architecture-in-DotNet]]", "[[CQRS-and-MediatR-Pattern]]", "[[Roadmap-DotNet]]"]
created: 2026-11-13
updated: 2026-11-13
sources:
  - https://learn.microsoft.com/dotnet/csharp/tour-of-csharp/tips-for-java-developers
---

# Java → .NET Mental Map

> [!summary] TL;DR
> - Note "bản đồ tổng" — mỗi dòng là một **cầu nối** từ thứ bạn đã biết (Java/Spring) sang thứ đang học (C#/.NET).
> - Cột **"Note chi tiết"** là wikilink: khi cần học sâu dòng nào thì tạo note đó (Just-In-Time).
> - Phần **⚠️ Bẫy** ở cuối quan trọng hơn cả bảng: đó là chỗ "giống mà không giống".

---

## 1. Runtime & Tooling

| ☕ Java | 🟣 .NET | Ghi chú | Note chi tiết |
| --- | --- | --- | --- |
| JVM | CLR (Common Language Runtime) | Cả hai: JIT, GC, managed memory | [[CLR-vs-JVM]] |
| Bytecode (`.class`) | IL — Intermediate Language | | |
| JAR | Assembly (`.dll`) | 1 project = 1 assembly | |
| Maven / Gradle | `dotnet` CLI + MSBuild | | |
| `pom.xml` / `build.gradle` | `.csproj` | Ngắn gọn hơn nhiều (SDK-style) | |
| Parent POM | `Directory.Build.props` | Tự import cho mọi project con | |
| `<dependencyManagement>` | `Directory.Packages.props` (Central Package Management) | | |
| Maven Central | nuget.org | | |
| Multi-module project | Solution (`.sln` / `.slnx`) + nhiều `.csproj` | | [[Clean-Architecture-in-DotNet]] |
| GraalVM native image | Native AOT (`<PublishAot>true</PublishAot>`) | | |
| SDKMAN / nhiều JDK | `global.json` ghim SDK version | | |
| JUnit 5 | xUnit / NUnit / MSTest | xUnit phổ biến nhất | [[Testing-in-DotNet]] |
| Mockito | NSubstitute / Moq | | |
| AssertJ | Shouldly / FluentAssertions | FluentAssertions 8+ là thương mại | |
| Testcontainers | Testcontainers for .NET | API gần như giống hệt | |
| ArchUnit | NetArchTest / ArchUnitNET | | |

## 2. Ngôn ngữ (Language)

| ☕ Java | 🟣 C# | Ghi chú | Note chi tiết |
| --- | --- | --- | --- |
| `package com.x.y;` | `namespace X.Y;` (file-scoped) | Namespace **không** bắt buộc khớp thư mục (nhưng nên) | |
| `import` | `using` (+ `global using`, ImplicitUsings) | | |
| getter/setter, Lombok `@Data` | **Property**: `public string Name { get; set; }` | Built-in, không cần Lombok | [[CSharp-Properties-and-Records]] |
| `record` (Java 16+) | `record` / `record struct` | C# record có `with` expression: `order with { Status = ... }` | [[CSharp-Properties-and-Records]] |
| `final` field | `readonly` field / `init` accessor | | |
| `final` class | `sealed` class | Nên `sealed` mặc định (hiệu năng + ý định rõ) | |
| `Optional<T>` | **Nullable reference types** (`string?`) + `?.` `??` | Kiểm tra lúc **compile**, không phải wrapper | [[Nullable-Reference-Types]] |
| Primitive + wrapper (`int`/`Integer`) | Value type (`int` = `System.Int32`) — không có boxing wrapper riêng | `int?` = `Nullable<int>` | [[Value-Types-vs-Reference-Types]] |
| (không có) | `struct` — value type do người dùng định nghĩa | Java Valhalla mới đang tới | [[Value-Types-vs-Reference-Types]] |
| Generics — **type erasure** | Generics — **reified** (giữ kiểu lúc runtime) | `typeof(T)`, `new T()`, `List<int>` không boxing | [[Generics-Reified-vs-Erasure]] |
| Stream API | **LINQ** | `filter→Where`, `map→Select`, `flatMap→SelectMany`, `collect(toList())→ToList()`, `reduce→Aggregate` | [[LINQ-vs-Stream-API]] |
| `CompletableFuture` / Virtual Threads | `Task` / `async`-`await` | async/await "lan" suốt call stack | [[Async-Await-Deep-Dive]] |
| `Function<T,R>`, `Supplier<T>`, `Consumer<T>` | `Func<T,TResult>`, `Func<T>`, `Action<T>` | + `delegate`, `event` | [[Delegates-Events-Lambdas]] |
| Annotation `@Something` | Attribute `[Something]` | | |
| try-with-resources | `using var x = ...;` / `await using` | `IDisposable` / `IAsyncDisposable` | |
| Checked exceptions | **Không có** checked exception | Không cần `throws` | |
| Static utility class | **Extension methods** | `"abc".IsPalindrome()` | |
| `switch` pattern matching (21) | `switch` expression, pattern matching (`is`, property patterns) | C# có từ lâu & mạnh hơn | |
| `String.format` / text blocks | `$"Hello {name}"`, raw string `"""..."""` | | |
| `enum` có field/method | `enum` chỉ là số nguyên | Muốn giàu hành vi → class/record (smart enum) | |
| `Object.equals/hashCode` | `Equals/GetHashCode`, `IEquatable<T>`; `record` tự sinh | | |
| `var` (10+) | `var` | | |
| Package-private | `internal` (theo **assembly**) | `InternalsVisibleTo` cho test | |

## 3. Spring Boot ↔ ASP.NET Core

| ☕ Spring Boot | 🟣 ASP.NET Core | Ghi chú | Note chi tiết |
| --- | --- | --- | --- |
| `@SpringBootApplication` + `main` | `Program.cs` + `WebApplication.CreateBuilder()` | Top-level statements | [[ASPNETCore-Hosting-and-Program]] |
| `@Component` / `@Service` + scan | `builder.Services.AddScoped<I, T>()` | **Đăng ký tường minh** | [[Dependency-Injection-DotNet-vs-Spring]] |
| Bean scope: singleton / prototype / request | `Singleton` / `Transient` / `Scoped` | Scoped = 1 instance / HTTP request | [[Dependency-Injection-DotNet-vs-Spring]] |
| `@Autowired` / constructor injection | Constructor injection (primary constructor) | Không có field injection | |
| `@Qualifier` | Keyed services: `[FromKeyedServices("x")]` (.NET 8+) | | |
| `@RestController` | `[ApiController]` + `ControllerBase` | Hoặc **Minimal API** `app.MapGet(...)` | [[Controllers-vs-Minimal-APIs]] |
| `@GetMapping("/{id}")` | `[HttpGet("{id}")]` | | |
| `@PathVariable` / `@RequestParam` / `@RequestBody` | `[FromRoute]` / `[FromQuery]` / `[FromBody]` | `[ApiController]` tự suy luận | |
| `ResponseEntity<T>` | `ActionResult<T>` / `IResult` (`Results.Ok()`) | | |
| `application.yml` + profiles | `appsettings.json` + `appsettings.{Environment}.json` | `ASPNETCORE_ENVIRONMENT` | [[Configuration-and-Options-Pattern]] |
| `@ConfigurationProperties` | `IOptions<T>` / `IOptionsSnapshot<T>` / `IOptionsMonitor<T>` | | [[Configuration-and-Options-Pattern]] |
| `@Value("${x}")` | `IConfiguration["x"]` | Nên dùng Options | |
| Servlet `Filter` / `HandlerInterceptor` | **Middleware** / MVC Filters | Middleware = pipeline toàn app | [[Middleware-Pipeline]] |
| `@RestControllerAdvice` | `IExceptionHandler` + `ProblemDetails` | | [[CQRS-and-MediatR-Pattern]] |
| Bean Validation `@Valid` | DataAnnotations / **FluentValidation** | | [[CQRS-and-MediatR-Pattern]] |
| Spring AOP | MediatR Pipeline Behavior / Decorator (Scrutor) / Filters | Không có AOP proxy mặc định | [[CQRS-and-MediatR-Pattern]] |
| Spring Security | Authentication + Authorization middleware, Policies, ASP.NET Core Identity | | [[AuthN-AuthZ-in-ASPNETCore]] |
| Actuator | Health Checks (`MapHealthChecks`) + OpenTelemetry | | |
| `@Scheduled` | `BackgroundService` / Hangfire / Quartz.NET | | [[Background-Jobs-in-DotNet]] |
| `RestTemplate` / `WebClient` | `HttpClient` qua `IHttpClientFactory` | **Không** `new HttpClient()` mỗi request | |
| OpenFeign | Refit | | |
| Resilience4j | Polly / `Microsoft.Extensions.Http.Resilience` | | |
| Spring Cache `@Cacheable` | `IMemoryCache` / `IDistributedCache` / `HybridCache` (.NET 9+) | | |
| springdoc-openapi | `Microsoft.AspNetCore.OpenApi` + Scalar/Swagger UI | | |
| SLF4J + Logback | `ILogger<T>` + Serilog | Structured logging `{Placeholder}` | |
| Spring Cloud | .NET Aspire (local orchestration), YARP (gateway) | | |

## 4. Data Access

| ☕ JPA / Hibernate | 🟣 EF Core | Ghi chú | Note chi tiết |
| --- | --- | --- | --- |
| `EntityManager` | `DbContext` | DbContext = Unit of Work + Repository | [[EF-Core-vs-JPA-Hibernate]] |
| Persistence Context | `ChangeTracker` | | |
| `@Entity`, `@Table`, `@Column` | Fluent API `IEntityTypeConfiguration<T>` (hoặc attribute) | Fluent API giữ Domain sạch | [[Clean-Architecture-in-DotNet]] |
| `@Embeddable` | Complex Type (`ComplexProperty`) / Owned Type | | |
| `JpaRepository<T,ID>` | `DbSet<T>` | | |
| JPQL / Criteria API | **LINQ** (type-safe, compile-time) | | |
| `FetchType.LAZY/EAGER`, `JOIN FETCH` | `Include()` / `ThenInclude()`; lazy loading **tắt mặc định** | | [[EF-Core-Loading-Related-Data]] |
| Dirty checking + auto flush khi commit | Change tracking + **phải gọi `SaveChangesAsync()`** | ⚠️ khác biệt lớn | |
| `@Transactional` | `SaveChanges` (tx ngầm) / `BeginTransactionAsync` / `TransactionScope` | | [[EF-Core-Transactions-and-Concurrency]] |
| `@Version` (optimistic lock) | `[Timestamp]` / `IsRowVersion()` / concurrency token | | [[EF-Core-Transactions-and-Concurrency]] |
| Flyway / Liquibase | EF Core Migrations (`dotnet ef migrations add`) | Có thể export SQL script: `dotnet ef migrations script` | |
| Spring Data Projections | `.Select(x => new Dto(...))` | | |
| `JdbcTemplate` | **Dapper** | | [[Dapper-for-Read-Side]] |
| `@Query(nativeQuery = true)` | `FromSql($"...")` / `SqlQuery<T>()` | Interpolated string an toàn SQL injection | |
| Read-only transaction | `AsNoTracking()` | | |

---

## 5. ⚠️ Bẫy "giống mà không giống"

> [!warning] 1. async/await phải "async all the way"
> Không bao giờ `.Result` hoặc `.Wait()` trên `Task` trong code ASP.NET Core → nguy cơ thread-pool starvation / deadlock (ở môi trường có SynchronizationContext). Không có khái niệm "block cho nhanh" như `future.get()`.

> [!warning] 2. Captive dependency (DI lifetime)
> Inject service **Scoped** (vd `DbContext`) vào **Singleton** → DbContext sống mãi, dùng chung giữa các request → lỗi thread-safety. Spring cũng có vấn đề tương tự (request-scoped bean trong singleton cần proxy), nhưng .NET **không tự tạo proxy** — chế độ Development sẽ ném lỗi `Cannot consume scoped service from singleton` để cảnh báo.

> [!warning] 3. EF Core không tự flush
> Quên `SaveChangesAsync()` = không có gì được ghi xuống DB, và không có lỗi nào báo bạn.

> [!warning] 4. Không có component scan
> Viết class xong phải đăng ký vào `IServiceCollection`. Lỗi `Unable to resolve service for type ...` lúc runtime là dấu hiệu quên đăng ký.

> [!warning] 5. Nullable reference types chỉ là cảnh báo compile-time
> `string?` không phải `Optional<String>`. Runtime vẫn có thể null (dữ liệu từ JSON, DB). Bật `TreatWarningsAsErrors` để nghiêm túc hơn.

> [!warning] 6. `DateTime` vs `DateTimeOffset`
> `DateTime` có `Kind` (Utc/Local/Unspecified) rất dễ sai. Ưu tiên `DateTimeOffset` (≈ `OffsetDateTime`) và `TimeProvider` (≈ `java.time.Clock`). Chỉ ngày: `DateOnly` (≈ `LocalDate`).

> [!warning] 7. So sánh chuỗi phụ thuộc culture
> `ToUpper()`, `StartsWith(string)`, `Compare` mặc định dùng culture hiện tại. Dùng `StringComparison.Ordinal` / `OrdinalIgnoreCase`, `ToUpperInvariant()` cho so sánh kỹ thuật.

> [!warning] 8. `==` trên string
> Trong C#, `==` so sánh **giá trị** string (không phải reference như Java). Thói quen `.equals()` không cần thiết.

> [!warning] 9. Enum chỉ là số
> Gán `(OrderStatus)999` vẫn compile & chạy. Luôn validate (`Enum.IsDefined`) với input từ ngoài.
