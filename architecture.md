# Architecture — FISIANOS POC

## System Architecture Diagram

```mermaid
graph TB
    subgraph Client["🖥️ Frontend — tech-frontend"]
        UI["React Components<br/>(TypeScript + JSX)"]
        State["State Management<br/>(React Hooks)"]
        API_Client["API Service Layer<br/>(fetch / HTTP client)"]
        Vite["Vite Dev Server<br/>:5173"]
    end

    subgraph Server["⚙️ Backend — tech-backend"]
        Router["Router Functions<br/>(Spring WebFlux)"]
        Handler["Handler / Controller<br/>(@RestController)"]
        Service["Service Layer<br/>(Business Logic)"]
        Reactor["Project Reactor<br/>(Mono / Flux)"]
        Netty["Reactor Netty<br/>:8080"]
    end

    subgraph Data["🗄️ Data Layer"]
        DB["Database<br/>(R2DBC Reactive Driver)"]
    end

    UI --> State
    State --> API_Client
    API_Client -->|"HTTP REST (JSON)"| Netty
    Netty --> Router
    Router --> Handler
    Handler --> Service
    Service --> Reactor
    Reactor --> DB

    style Client fill:#1a1a2e,stroke:#e94560,stroke-width:2px,color:#eee
    style Server fill:#16213e,stroke:#0f3460,stroke-width:2px,color:#eee
    style Data fill:#0f3460,stroke:#533483,stroke-width:2px,color:#eee
```

## Data Flow — Request/Response Cycle

```mermaid
sequenceDiagram
    participant User as 👤 User
    participant React as React App<br/>(Vite :5173)
    participant API as API Service
    participant WebFlux as Spring WebFlux<br/>(Netty :8080)
    participant Service as Service Layer
    participant DB as Database

    User->>React: Interacts with UI
    React->>API: Calls API service function
    API->>WebFlux: HTTP Request (GET/POST/PUT/DELETE)
    WebFlux->>Service: Routes to handler method
    Service->>DB: Reactive query (Mono/Flux)
    DB-->>Service: Reactive result stream
    Service-->>WebFlux: Returns Mono/Flux response
    WebFlux-->>API: HTTP Response (JSON)
    API-->>React: Updates state with data
    React-->>User: Re-renders UI
```

## Deployment Topology

```mermaid
graph LR
    subgraph Development["🛠️ Development Environment"]
        FE_DEV["Vite Dev Server<br/>localhost:5173"]
        BE_DEV["Spring Boot<br/>localhost:8080"]
    end

    subgraph Production["🚀 Production"]
        CDN["Static Hosting / CDN<br/>(Vite build output)"]
        BE_PROD["Spring Boot JAR<br/>(Containerized)"]
    end

    FE_DEV -->|"Proxy / CORS"| BE_DEV
    CDN -->|"HTTPS REST"| BE_PROD

    style Development fill:#1b1b2f,stroke:#e94560,stroke-width:2px,color:#eee
    style Production fill:#162447,stroke:#1f4068,stroke-width:2px,color:#eee
```

## Key Architectural Decisions

| Decision                     | Choice                        | Rationale                                                    |
| ---------------------------- | ----------------------------- | ------------------------------------------------------------ |
| Frontend Framework           | React + TypeScript            | Industry standard, strong ecosystem, type safety             |
| Build Tool                   | Vite                          | Instant HMR, fast builds, native ES modules                  |
| Backend Framework            | Spring Boot + WebFlux         | Non-blocking I/O, high throughput, backpressure support      |
| API Protocol                 | REST over HTTP/JSON           | Simplicity, broad tooling support, easy debugging            |
| Reactive Database Access     | R2DBC                         | Non-blocking database driver compatible with WebFlux         |
| Package Manager (Frontend)   | Yarn                          | Deterministic installs, workspaces support, speed            |
| Build Tool (Backend)         | Maven                         | Mature ecosystem, Spring Boot starter support                |
