# Blazor in Azure Static Web Apps: Capabilities and Limitations

This document outlines what you can and cannot do with Blazor WebAssembly applications hosted on Azure Static Web Apps, focusing on the static nature constraints and available workarounds.

## What You CAN Do with Blazor in Azure Static Web Apps

### 🟢 Client-Side Capabilities

- **Rich Interactive UI**: Full SPA experience with component-based architecture
- **Client-side routing**: Navigate between pages without server round-trips
- **State management**: Manage application state entirely on the client
- **Local storage**: Use browser's localStorage and sessionStorage
- **API consumption**: Call external REST APIs, Azure Functions, or third-party services
- **Real-time features**: SignalR connections to external hubs
- **Authentication**: Azure AD B2C, GitHub, Twitter, Google, Facebook integration
- **Progressive Web App (PWA)**: Offline capabilities, push notifications, app-like experience
- **Browser APIs**: Access camera, geolocation, notifications, etc.
- **Client-side validation**: Form validation without server round-trips
- **Responsive design**: Bootstrap, CSS frameworks, custom responsive layouts

### 🟢 Development & Deployment Benefits

- **Global CDN**: Fast content delivery worldwide
- **Auto-scaling**: Handles traffic spikes automatically
- **Cost-effective**: Pay only for what you use
- **Easy deployment**: Direct GitHub/Azure DevOps integration
- **Custom domains**: Use your own domain with SSL
- **Staging environments**: Pull request previews

## What You CANNOT Do (Static Limitations)

### 🔴 Server-Side Processing

- **No server-side code execution**: No Blazor Server components
- **No database connections**: Cannot directly connect to SQL Server, CosmosDB, etc.
- **No file system access**: Cannot read/write files on server
- **No server-side sessions**: No HttpContext, session state, or server memory
- **No server-side authentication logic**: Must rely on external identity providers

### 🔴 Backend Operations

- **No background services**: No hosted services, timers, or background tasks
- **No server-side data processing**: Heavy computations must be client-side or external
- **No email sending**: Cannot send emails directly (need external service)
- **No scheduled tasks**: No cron jobs or scheduled operations

### 🔴 Security Limitations

- **Client-side secrets**: All code is visible to users
- **No server-side authorization**: Authorization logic runs on client (can be bypassed)
- **API keys exposure**: Cannot hide sensitive keys (must use proxy APIs)

## Workarounds for Static Limitations

### ✅ Azure Functions Integration

```csharp
// Call Azure Functions for server-side logic
var response = await Http.GetFromJsonAsync<WeatherData[]>("/api/weather");
```

### ✅ External APIs

```csharp
// Use external services for backend functionality
builder.Services.AddScoped(sp => new HttpClient { 
    BaseAddress = new Uri("https://api.external-service.com/") 
});
```

### ✅ Database Access via APIs

- Use Azure Functions or Azure API Management as proxy
- Call REST APIs that handle database operations
- Use Azure Static Web Apps database connections (preview feature)

## Best Use Cases for Static Blazor Apps

1. **Portfolio websites** with interactive demos
2. **Documentation sites** with search and filtering
3. **Client tools** and calculators
4. **Dashboards** consuming external APIs
5. **E-commerce frontends** (with external payment processing)
6. **Content management** for static content
7. **Educational apps** and tutorials

## When to Choose Blazor Server Instead

- Need real-time server processing
- Handle sensitive business logic
- Require direct database access
- Need server-side authentication/authorization
- Process large amounts of server-side data

## .NET 9 Blazor Improvements

For future .NET 9 Blazor apps, you'll have access to:

- Improved AOT compilation
- Better performance optimizations
- Enhanced component libraries
- New authentication improvements
- Better integration with .NET 9 ecosystem

## Architecture Considerations

### Recommended Static Web App Architecture

```
Frontend (Blazor WASM)
    ↓ HTTP calls
Azure Functions API
    ↓ Database calls
Azure SQL / CosmosDB
```

### Authentication Flow

```
User → Azure AD B2C → Static Web App → Protected API endpoints
```

This architecture ensures proper separation of concerns while maintaining the benefits of static hosting.
