# Architecture Diagram

This diagram illustrates the high-level architecture of the eShopOnWeb application, a reference ASP.NET Core e-commerce solution with a layered structure.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Browser\nHTML / CSS / JavaScript"]
    end

    subgraph Presentation["Presentation Layer"]
        Web["Web\nASP.NET Core MVC\nRazor Pages"]
        BlazorAdmin["BlazorAdmin\nBlazor WebAssembly\nAdmin UI"]
        PublicApi["PublicApi\nASP.NET Core Web API\nSwagger / OpenAPI"]
    end

    subgraph Shared["Shared"]
        BlazorShared["BlazorShared\nShared Models\nFluentValidation"]
    end

    subgraph Business["Business Logic Layer"]
        AppCore["ApplicationCore\nDomain Entities\nUse Cases / Services\nArdalis Specification\nMediatR / CQRS"]
    end

    subgraph DataAccess["Data Access Layer"]
        Infra["Infrastructure\nEntity Framework Core\nRepositories\nIdentity Services\nJWT Token Provider"]
    end

    subgraph Storage["Data Storage"]
        CatalogDb[("SQL Server\nCatalogDb")]
        IdentityDb[("SQL Server\nIdentityDb")]
        InMemory[("In-Memory DB\nTesting / Dev")]
    end

    subgraph External["External Services"]
        AzureKV["Azure Key Vault\nSecrets Management"]
        AzureIdentity["Azure Identity\nManaged Identity"]
    end

    Browser -->|"HTTP / HTTPS"| Web
    Browser -->|"HTTP / HTTPS"| PublicApi
    Web -->|"hosts"| BlazorAdmin
    BlazorAdmin -->|"HTTP JSON"| PublicApi
    Web --> AppCore
    Web --> Infra
    PublicApi --> AppCore
    PublicApi --> Infra
    BlazorAdmin --> BlazorShared
    AppCore --> BlazorShared
    Infra --> AppCore
    Infra -->|"EF Core"| CatalogDb
    Infra -->|"EF Core Identity"| IdentityDb
    Infra -->|"EF Core InMemory"| InMemory
    Web -->|"Azure SDK"| AzureKV
    AzureKV --> AzureIdentity
```
