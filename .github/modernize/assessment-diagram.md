# eShopOnWeb Architecture Diagram

## Application Architecture

```mermaid
flowchart TD
    subgraph Clients["Client Layer"]
        Browser["Web Browser"]
        MobileClient["Mobile / External Client"]
    end

    subgraph Presentation["Presentation Layer (ASP.NET Core)"]
        Web["Web\n(ASP.NET Core MVC + Razor Pages)\nnet8.0"]
        BlazorAdmin["BlazorAdmin\n(Blazor WebAssembly)\nnet8.0"]
        BlazorShared["BlazorShared\n(Shared Models / DTOs)\nnet8.0"]
        PublicApi["PublicApi\n(ASP.NET Core Web API + Swagger)\nnet8.0"]
    end

    subgraph Business["Business Logic Layer"]
        AppCore["ApplicationCore\n(Domain Entities, Services,\nSpecifications, Interfaces)\nArdalis.Specification / MediatR"]
    end

    subgraph DataAccess["Data Access Layer"]
        Infra["Infrastructure\n(Entity Framework Core,\nASP.NET Core Identity,\nRepository Implementations)"]
    end

    subgraph DataStorage["Data Storage"]
        CatalogDB[("SQL Server\nCatalogDb")]
        IdentityDB[("SQL Server\nIdentityDb")]
    end

    subgraph ExternalServices["External Services"]
        AzureKeyVault["Azure Key Vault\n(Configuration Secrets)"]
        AzureIdentity["Azure Identity\n(Managed Identity / Auth)"]
    end

    Browser -->|"HTTP / HTTPS"| Web
    Browser -->|"HTTP / HTTPS (Blazor WASM)"| BlazorAdmin
    MobileClient -->|"REST / JWT"| PublicApi

    Web --> BlazorAdmin
    Web --> BlazorShared
    Web --> AppCore
    Web --> Infra

    BlazorAdmin --> BlazorShared
    BlazorAdmin --> AppCore

    PublicApi --> AppCore
    PublicApi --> Infra

    AppCore --> BlazorShared

    Infra --> AppCore
    Infra --> CatalogDB
    Infra --> IdentityDB

    Web -.->|"Azure SDK"| AzureKeyVault
    Web -.->|"Azure SDK"| AzureIdentity
```

## Technology Stack Summary

| Layer | Project | Framework / Libraries |
|---|---|---|
| Web UI | Web | ASP.NET Core 8 MVC + Razor Pages, Blazor WebAssembly Host, EF Core, Identity, MediatR, AutoMapper, Azure.Identity |
| Admin UI | BlazorAdmin | Blazor WebAssembly 8, Blazored.LocalStorage, FluentValidation |
| Shared Models | BlazorShared | .NET 8 class library |
| REST API | PublicApi | ASP.NET Core 8 Web API, Minimal API Endpoints, Swagger/OpenAPI, JWT Bearer |
| Domain | ApplicationCore | .NET 8, Ardalis.Specification, Ardalis.GuardClauses, Ardalis.Result |
| Data Access | Infrastructure | EF Core 8, SQL Server, ASP.NET Core Identity |
| Data Storage | — | Microsoft SQL Server (Catalog DB, Identity DB) |
| Cloud Integration | — | Azure Key Vault, Azure Identity (Managed Identity) |
