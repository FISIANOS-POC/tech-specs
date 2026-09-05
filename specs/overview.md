# FISIANOS POC — Project Overview

## Description

**FISIANOS** is a proof-of-concept (POC) full-stack web application designed to demonstrate a modern, reactive architecture connecting a **React** frontend with a **Spring Boot WebFlux** backend.

The project is structured as a multi-repository monorepo consisting of three independent codebases:

| Repository        | Purpose                                                        |
| ----------------- | -------------------------------------------------------------- |
| `tech-specs`      | Specifications, architecture diagrams, and AI agent guidelines |
| `tech-frontend`   | React + TypeScript + Vite client application                   |
| `tech-backend`    | Spring Boot WebFlux (Java) reactive REST API                   |

## High-Level Architecture

```
┌──────────────────────────┐         ┌──────────────────────────────┐
│      tech-frontend       │  HTTP   │        tech-backend          │
│  React + TypeScript      │◄───────►│  Spring Boot + WebFlux       │
│  Vite dev server / build │  REST   │  Reactive REST API (Netty)   │
│  Port: 5173              │         │  Port: 8080                  │
└──────────────────────────┘         └──────────────────────────────┘
```

## Goals

1. **Decoupled Architecture**: Frontend and backend are completely independent, communicating exclusively via RESTful HTTP endpoints.
2. **Reactive Backend**: Leverage Spring WebFlux and Project Reactor to handle requests in a non-blocking, event-driven manner.
3. **Modern Frontend**: Utilize React 18+ with TypeScript for type safety, Vite for fast HMR, and Yarn as the package manager.
4. **Developer Experience**: Provide clear specifications, architecture documentation, and AI-assisted development guidelines to accelerate onboarding.

## Tech Stack

### Frontend (`tech-frontend`)
- **Framework**: React 18+
- **Language**: TypeScript
- **Build Tool**: Vite
- **Package Manager**: Yarn
- **Styling**: CSS Modules / Vanilla CSS

### Backend (`tech-backend`)
- **Framework**: Spring Boot 3.x
- **Reactive Stack**: Spring WebFlux (Reactor Netty)
- **Language**: Java 17+
- **Build Tool**: Maven
- **API Style**: RESTful JSON

## Communication Flow

1. The React frontend sends HTTP requests (GET, POST, PUT, DELETE) to the backend API.
2. The Spring WebFlux backend processes requests reactively using `Mono` and `Flux` publishers.
3. Responses are serialized as JSON and returned to the frontend.
4. CORS is configured on the backend to allow requests from the frontend's development origin (`http://localhost:5173`).

## Getting Started

Refer to each repository's own README for setup instructions:

- **Frontend**: `tech-frontend/README.md`
- **Backend**: `tech-backend/README.md`
