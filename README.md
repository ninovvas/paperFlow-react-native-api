# PaperFlow React Native API

A lightweight REST API server for the **PaperFlow** React Native application. It provides authentication and data persistence for users, paper search filters, and favorited academic papers — powered by [`json-server`](https://github.com/typicode/json-server) and [`json-server-auth`](https://github.com/jeremyben/json-server-auth).

---

## Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Running the Server](#running-the-server)
- [API Endpoints](#api-endpoints)
  - [Authentication](#authentication)
  - [Filters](#filters)
  - [Favorites](#favorites)
- [Database Structure](#database-structure)
- [Environment Variables](#environment-variables)
- [Repository](#repository)

---

## Overview

PaperFlow is an academic paper discovery app. This backend server handles:

- **User registration and login** (JWT-based auth via `json-server-auth`)
- **Search filters** — saved queries with keywords, categories, and source configuration
- **Favorites** — bookmarked academic papers with metadata (title, authors, abstract, PDF URL)

---

## Tech Stack

| Tool | Version | Purpose |
|---|---|---|
| `json-server` | ^0.17.4 | REST API from a JSON file |
| `json-server-auth` | ^2.1.0 | JWT authentication middleware |
| Node.js | LTS | Runtime |

---

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (LTS recommended)
- npm

### Installation

```bash
git clone https://github.com/ninovvas/paperFlow-react-native-api.git
cd paperFlow-react-native-api
npm install
```

---

## Running the Server

### Development (port 3001)

```bash
npm run dev
```

The server will start at `http://localhost:3001`.

### Production

```bash
npm start
```

In production, the server reads the port from the `$PORT` environment variable and binds to `0.0.0.0`.

---

## API Endpoints

> Base URL: `http://localhost:3001` (development)

### Authentication

`json-server-auth` exposes the following auth routes:

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/register` | Register a new user |
| `POST` | `/login` | Log in and receive a JWT token |

#### Register

```http
POST /register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "yourpassword",
  "displayName": "Your Name"
}
```

#### Login

```http
POST /login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "yourpassword"
}
```

**Response:**
```json
{
  "accessToken": "<JWT_TOKEN>",
  "user": {
    "email": "user@example.com",
    "displayName": "Your Name",
    "id": 1
  }
}
```

Include the token in subsequent requests:

```http
Authorization: Bearer <JWT_TOKEN>
```

---

### Filters

Saved search filters for fetching academic papers.

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/filters` | Get all filters |
| `GET` | `/filters/:id` | Get a filter by ID |
| `POST` | `/filters` | Create a new filter |
| `PUT` | `/filters/:id` | Replace a filter |
| `PATCH` | `/filters/:id` | Update a filter partially |
| `DELETE` | `/filters/:id` | Delete a filter |

#### Filter Object

```json
{
  "id": 1,
  "name": "mobile robot Reinforcement Learning",
  "keywords": ["reinforcement learning", "mobile robots", "navigation with obstacles"],
  "categories": ["cs.RO"],
  "source": "arxiv",
  "isActive": true,
  "maxResults": 25,
  "dateFrom": null,
  "dateUntil": null,
  "userId": 1,
  "createdAt": "2026-02-28T20:59:55.355Z",
  "updatedAt": "2026-03-02T21:51:37.572Z"
}
```

---

### Favorites

Bookmarked academic papers saved by users.

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/favorites` | Get all favorites |
| `GET` | `/favorites/:id` | Get a favorite by ID |
| `POST` | `/favorites` | Save a paper to favorites |
| `DELETE` | `/favorites/:id` | Remove a paper from favorites |

#### Favorite Object

```json
{
  "id": 1,
  "paperId": "2602.23115v1",
  "source": "arxiv",
  "title": "FLIGHT: Fibonacci Lattice-based Inference for Geometric Heading in real-Time",
  "authors": ["David Dirnfeld", "Fabien Delattre", "Pedro Miraldo", "Erik Learned-Miller"],
  "abstract": "...",
  "categories": ["cs.CV", "cs.CG", "cs.RO"],
  "publishedDate": "2026-02-26T15:27:49Z",
  "pdfUrl": "https://arxiv.org/pdf/2602.23115v1",
  "userId": 1,
  "savedAt": "2026-02-28T20:42:11.000Z"
}
```

---

## Database Structure

The `db.json` file acts as the database. It contains three collections:

```json
{
  "users": [...],
  "filters": [...],
  "favorites": [...]
}
```

> `db.json` is mutated directly by the server. Back it up before deploying to production if persistence is important.

A demo user is seeded by default:

| Field | Value |
|---|---|
| Email | `demo@paperflow.com` |
| Password | `demo` (bcrypt hashed in db) |

---

## Environment Variables

| Variable | Description | Default |
|---|---|---|
| `PORT` | Port the server listens on (production) | — |

In development, the port is hardcoded to `3001` via the `dev` script.

---

## Repository

- **GitHub:** [https://github.com/ninovvas/paperFlow-react-native-api](https://github.com/ninovvas/paperFlow-react-native-api)
- **Issues:** [https://github.com/ninovvas/paperFlow-react-native-api/issues](https://github.com/ninovvas/paperFlow-react-native-api/issues)
