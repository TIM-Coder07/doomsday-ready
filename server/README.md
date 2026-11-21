# Doomsday Ready – Backend

The backend of Doomsday Ready is built with Node.js, Express, and TypeScript to provide REST APIs for managing MCU movies, generating prerequisite-based rewatch plans, and calculating cinema seat bundle combinations.

It handles business logic, request validation, database operations, and API responses. MongoDB is used to store movie and cinema data, while Mongoose provides schema modeling and database interaction.

The backend follows a modular architecture with separate responsibilities for routes, controllers, models, validators, services, and middleware to keep the code organized and maintainable.

## Tech Stack

| Part | Technology |
|---|---|
| Runtime | Node.js |
| Framework | Express 5 |
| Language | TypeScript |
| Database | MongoDB |
| ODM | Mongoose 9 |
| Validation | Zod |
| Testing | Vitest |

## Project Structure

The backend follows a modular and layered architecture. Each folder has a specific responsibility, separating API handling, business logic, database operations, and testing.

```text
server/
├── src/
│   ├── config/
│   │   └── db.ts                 # MongoDB connection
│   │
│   ├── models/
│   │   ├── Movie.ts              # Movie schema
│   │   └── Cinema.ts             # Cinema schema
│   │
│   ├── validators/
│   │   ├── movie.validator.ts    # Movie request validation
│   │   └── cinema.validator.ts   # Cinema request validation
│   │
│   ├── controllers/
│   │   ├── movie.controller.ts   # Movie request handlers
│   │   └── cinema.controller.ts  # Cinema request handlers
│   │
│   ├── routes/
│   │   ├── movie.routes.ts       # Movie API routes
│   │   └── cinema.routes.ts      # Cinema API routes
│   │
│   ├── services/
│   │   ├── cycleCheck.ts         # Circular dependency detection
│   │   ├── rewatchOrder.ts       # Rewatch ordering algorithm
│   │   └── bookingQuote.ts       # Minimum bundle calculation
│   │
│   ├── middlewares/
│   │   ├── notFound.ts           # 404 handler
│   │   └── errorHandler.ts       # Centralized error handler
│   │
│   ├── utils/
│   │   └── AppError.ts           # Custom application error
│   │
│   ├── scripts/
│   │   └── seed.ts               # Sample database seeding
│   │
│   ├── app.ts                    # Express app configuration
│   └── server.ts                 # Server entry point
│
├── tests/
│   ├── cycleCheck.test.ts
│   ├── rewatchOrder.test.ts
│   └── bookingQuote.test.ts
│
├── .env.example                  # Environment variables example
├── package.json
└── tsconfig.json
```

### Folder Responsibilities

- **config/** – Handles MongoDB database configuration and connection.
- **models/** – Defines Mongoose schemas for movies and cinemas.
- **validators/** – Validates incoming request bodies and query parameters using Zod.
- **controllers/** – Handles incoming HTTP requests, coordinates operations, and sends API responses.
- **routes/** – Defines Express API endpoints and connects them to their respective controllers.
- **services/** – Contains independent business logic and core algorithm implementations.
- **middlewares/** – Handles unknown routes and centralized application errors.
- **utils/** – Contains reusable utility classes such as the custom `AppError`.
- **scripts/** – Contains the database seeding script for loading sample movies and cinemas.
- **tests/** – Contains unit tests for the three core algorithm functions.
- **app.ts** – Configures Express middleware and registers API routes.
- **server.ts** – Establishes the database connection and starts the Express server.

## Features & Functionality

### 1. Movie Management API

- Create new movies with title, release year, runtime, and prerequisites.
- Retrieve the complete movie collection.
- Update movie information, including watched status and prerequisites.
- Validate incoming request data using Zod.
- Prevent duplicate movie titles, regardless of letter casing.
- Verify that prerequisite movie IDs exist before saving.
- Prevent circular dependencies when creating or updating movie relationships.

### 2. Rewatch Plan API

- Generate a prerequisite-based watch order for a selected target movie.
- Retrieve all required movies in a valid watching sequence.
- Ensure prerequisite movies appear before their dependent movies.
- Avoid duplicate movies when multiple dependency paths lead to the same movie.
- Calculate the total number of required movies and watched movies.
- Calculate the remaining runtime based on unwatched movies.
- Return an empty order when the target movie has no prerequisites.

### 3. Cinema Management API

- Create cinemas with unique names and available seat bundle sizes.
- Retrieve the list of available cinemas.
- Validate cinema information and bundle size constraints.
- Prevent duplicate cinema names.

### 4. Booking Calculation API

- Accept a cinema ID and requested group size.
- Calculate the minimum number of seat bundles required to accommodate the group exactly.
- Return the selected bundle combination and total bundle count.
- Handle cases where no exact combination is possible.
- Validate group size and reject invalid booking requests.

### 5. Error Handling & Validation

- Centralized error-handling middleware for consistent API responses.
- Zod-based request validation.
- Meaningful HTTP status codes and error messages.
- Handle invalid IDs, missing resources, duplicate records, and circular dependencies.
- Custom application errors with HTTP status codes and error codes.

## API Endpoints

All endpoints are available under `/api`.

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/health` | API health check |
| POST | `/movies` | Create a movie |
| GET | `/movies` | Retrieve all movies |
| PATCH | `/movies/:id` | Update a movie |
| GET | `/movies/:id/rewatch-order` | Generate rewatch order |
| POST | `/cinemas` | Create a cinema |
| GET | `/cinemas` | Retrieve all cinemas |
| GET | `/cinemas/:id/booking?groupSize=6` | Calculate booking quote |

## Implementation Approach

The backend follows a modular architecture with a clear separation of responsibilities.

- **Database Modeling:** Mongoose schemas define the Movie and Cinema data structures and their database constraints.
- **Request Validation:** Zod validates incoming request bodies and query parameters before processing.
- **Controller Layer:** Controllers handle HTTP requests, coordinate database operations and service functions, and return appropriate responses.
- **Routing:** Express routers organize API endpoints and connect them to their corresponding controllers.
- **Service Layer:** Core business logic is implemented independently of Express and database operations, making the algorithms easier to test and maintain.
- **Error Handling:** Centralized middleware handles application errors and returns consistent error responses.
- **Database Configuration:** A dedicated configuration module manages the MongoDB connection.

### Core Algorithm Implementation

The three main algorithm functions are implemented from scratch inside the service layer without using libraries that provide these solutions.

**1. Circular Dependency Detection (`cycleCheck.ts`)**

- Represents movie prerequisites as a directed graph.
- Uses iterative Depth-First Search (DFS) to determine whether adding a prerequisite would create a cycle.
- Uses a visited set to avoid exploring the same movie repeatedly.
- Prevents invalid circular dependencies before updating the database.

Time Complexity: O(V + E)

**2. Rewatch Ordering (`rewatchOrder.ts`)**

- Uses DFS to traverse the prerequisite graph starting from the target movie.
- Processes prerequisites before adding their dependent movie to the result.
- Follows post-order traversal to generate a valid topological ordering.
- Uses a visited set to prevent duplicate entries.
- Excludes the target movie from its own prerequisite list.

Time Complexity: O(V + E)

**3. Minimum Bundle Calculation (`bookingQuote.ts`)**

- Solves the minimum coin change problem using bottom-up Dynamic Programming.
- Calculates the minimum number of bundles required for every possible group size.
- Tracks the selected bundle size to reconstruct the final combination.
- Returns an impossible result when no exact combination exists.
- Avoids greedy selection, which does not always produce the optimal result.

Time Complexity: O(groupSize × number of bundle sizes)

## Testing

Vitest is used to test the three core algorithm functions independently.

A total of 25 unit tests cover:

- Circular dependency detection.
- Valid rewatch ordering and prerequisite relationships.
- Movies with no prerequisites.
- Minimum bundle combinations.
- Booking scenarios where an exact combination is impossible.
- Expected edge cases and algorithm behavior.

The tests verify the actual algorithmic results rather than relying only on successful API responses.

## AI Usage

I used Claude (Anthropic) as a coding assistant throughout backend development. It was used to:

- plan the backend folder structure and organize the implementation into separate layers
- assist with Mongoose models, Zod validators, controllers, routes, and middleware
- explore and implement the three core algorithms (`cycleCheck.ts`, `rewatchOrder.ts`, `bookingQuote.ts`)
- write unit tests for different algorithm scenarios and edge cases
- troubleshoot TypeScript, database, API, and testing issues
- review implementation approaches and prepare backend documentation

My own part:

- understanding the request-response lifecycle and how data flows through routes, controllers, services, and models
- understanding REST API design, HTTP methods, status codes, and request validation
- understanding graph representation, DFS, topological sorting, and dynamic programming behind the core algorithms
- reviewing the generated code and understanding the logic and time complexity of the algorithms
- running the backend locally, testing API endpoints, and checking responses against the expected results
- debugging implementation issues and verifying the behavior of the final API

I used AI to speed up development and explore implementation approaches, while focusing on understanding the underlying programming concepts, reviewing the generated code, and verifying the final implementation rather than blindly relying on AI-generated solutions.