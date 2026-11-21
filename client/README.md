# Doomsday Ready – Frontend

The frontend of Doomsday Ready is built with Next.js and React to provide an interactive and user-friendly experience for MCU fans. It allows users to manage movies, generate rewatch plans, track their watching progress, and calculate cinema seat bookings through a clean and responsive interface.

The frontend communicates with the Express REST API using Axios and handles user interactions, form submissions, data fetching, and displaying API responses.

## Tech Stack

| Part | Technology |
|---|---|
| Framework | Next.js 16 (App Router) |
| UI Library | React 19 |
| Language | TypeScript |
| Styling | Tailwind CSS 4 |
| API Integration | Axios |
| Form Handling | React Hook Form |
| Validation | Zod |

## Project Structure

The frontend follows a feature-oriented component structure using Next.js App Router. Pages, reusable components, API utilities, and TypeScript types are organized separately to keep the codebase maintainable.

```text
client/
├── public/                     # Static assets
│
├── src/
│   ├── app/                    # Application pages and routing
│   │   ├── movies/             # Movie management page
│   │   ├── rewatch-plan/       # Rewatch planning page
│   │   ├── booking/            # Cinema booking page
│   │   ├── layout.tsx          # Root layout
│   │   ├── page.tsx            # Home page
│   │   └── globals.css         # Global styles
│   │
│   ├── components/             # Reusable UI components
│   │   ├── Navbar.tsx          # Navigation component
│   │   ├── forms/              # Form components
│   │   ├── movie-list/         # Movie list components
│   │   ├── progress-bar/       # Watching progress component
│   │   └── booking-result/     # Booking result component
│   │
│   ├── lib/
│   │   └── api/                # Axios client and API functions
│   │
│   └── types/                  # Shared TypeScript types
│
├── .env.example                # Environment variables example
├── package.json
├── tsconfig.json
└── next.config.ts
```

### Folder Responsibilities

- **app/** – Contains application pages and handles navigation using Next.js App Router.
- **components/** – Stores reusable UI components, forms, navigation, and feature-specific components.
- **lib/api/** – Contains Axios configuration and API functions for communicating with the Express backend.
- **types/** – Contains TypeScript interfaces and type definitions used throughout the frontend.
- **public/** – Stores static assets used by the application.

## Features & Functionality

### 1. Movie Management

- Display the available movies with their details, including title, release year, runtime, and watched status.
- Add new movies through a form.
- Update movie information and watching status.
- Select prerequisite movies when creating or updating a movie.
- Display validation errors for invalid form inputs.
- Show appropriate feedback for successful and failed operations.

### 2. Rewatch Plan

- Select a target movie to generate its prerequisite watchlist.
- Display movies in the correct watching order.
- Show watched and unwatched movie status.
- Allow users to mark movies as watched.
- Display overall watching progress.
- Show the remaining watch time based on unwatched movies.
- Handle movies with no prerequisites with an appropriate empty state.

### 3. Cinema Booking

- Display available cinemas and their seat bundle options.
- Add a new cinema through a form.
- Select a cinema and enter the required group size.
- Display the calculated booking result.
- Show the minimum number of bundles required for an exact group size.
- Handle unavailable booking combinations with clear feedback.

### 4. User Interface & Experience

- Responsive layouts for different screen sizes.
- Reusable React components to maintain consistency.
- Form validation and meaningful error messages.
- Loading, empty, and error states for a better user experience.
- Clear navigation between Movies, Rewatch Plan, and Booking pages.

## Implementation Approach

The frontend follows a component-based architecture using Next.js App Router and React.

- **Page Structure:** Each main feature has its own page, keeping movie management, rewatch planning, and cinema booking separate.
- **Reusable Components:** Common UI elements are organized into reusable components to reduce duplication and maintain consistency.
- **API Integration:** Axios is used to communicate with the Express backend through centralized API functions.
- **Form Handling:** React Hook Form manages form state and submissions, while Zod handles input validation.
- **Type Safety:** TypeScript interfaces and types help maintain consistent data structures between API responses and frontend components.
- **User Feedback:** Loading indicators, empty states, validation messages, and error handling provide feedback during user interactions.

## AI Usage

I used Claude (Anthropic) as a coding assistant during frontend development. It was used to:

- plan the frontend structure and organize pages and reusable components
- assist with building the movie management, rewatch plan, and cinema booking interfaces
- implement form handling and validation using React Hook Form and Zod
- integrate frontend pages with the Express REST API using Axios
- improve component structure, TypeScript types, and responsive layouts
- troubleshoot frontend errors and prepare the README documentation

My own part:

- understanding the component-based architecture and how data flows between pages, components, and API functions
- understanding React state, props, event handling, and form submission workflows
- reviewing the generated components and understanding their responsibilities
- testing frontend interactions and verifying that API responses are displayed correctly
- debugging UI issues, API integration problems, and form validation errors
- checking the application across different screen sizes and verifying the overall user experience

I used AI to speed up development and explore implementation approaches, while focusing on understanding the React concepts, reviewing the generated code, and verifying the final functionality rather than blindly relying on AI-generated solutions.