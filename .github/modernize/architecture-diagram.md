# Architecture Diagram

This diagram represents the high-level architecture of the eShopOnWeb application, a .NET-based e-commerce reference application using ASP.NET Core, Blazor WebAssembly, and Entity Framework Core.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser / Client"]

    subgraph Presentation["Presentation Layer"]
        Web["Web\nASP.NET Core MVC + Razor Pages\nAutoMapper · MediatR · JWT Bearer"]
        BlazorAdmin["BlazorAdmin\nBlazor WebAssembly\nBlazoredLocalStorage · WASM Auth"]
        PublicApi["PublicApi\nASP.NET Core Web API\nArdalis.ApiEndpoints · Swagger · JWT Bearer"]
    end

    subgraph Shared["Shared"]
        BlazorShared["BlazorShared\nShared Models and Components\nFluentValidation"]
    end

    subgraph Business["Business Logic Layer"]
        AppCore["ApplicationCore\nDomain Entities · Services · Specifications\nArdalis.GuardClauses · Ardalis.Result · Ardalis.Specification"]
    end

    subgraph DataAccess["Data Access Layer"]
        Infra["Infrastructure\nEF Core Repositories · Identity\nSqlServer · Ardalis.Specification.EFCore"]
    end

    subgraph Storage["Data Storage"]
        CatalogDb[("SQL Server\nCatalogDb")]
        IdentityDb[("SQL Server\nIdentityDb")]
    end

    subgraph External["External Services"]
        AzureKeyVault["Azure Key Vault\nSecrets Management\nAzure.Identity"]
    end

    Browser -->|"HTTP requests"| Web
    Browser -->|"HTTP requests"| PublicApi
    Web -->|"hosts"| BlazorAdmin
    BlazorAdmin -->|"REST calls"| PublicApi
    Web --> BlazorShared
    BlazorAdmin --> BlazorShared
    Web --> AppCore
    Web --> Infra
    PublicApi --> AppCore
    PublicApi --> Infra
    AppCore --> BlazorShared
    Infra --> AppCore
    Infra -->|"EF Core queries"| CatalogDb
    Infra -->|"EF Core Identity"| IdentityDb
    Web -->|"reads secrets"| AzureKeyVault
```
