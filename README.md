# 15-Geolocation

A React application that retrieves the user's current GPS coordinates using the browser's Geolocation API and displays them with a direct link to OpenStreetMap. Built as a learning exercise to demonstrate how to extract component-level state logic into **reusable custom hooks** in React.

This project is part of the [Jonas Schmedtmann React course](https://www.udemy.com/course/the-ultimate-react-course/) and is intended for developers learning React patterns, specifically custom hooks and separation of concerns.

---

## Problem Statement

In many React applications, component logic grows bloated when state management, side effects, and API calls live directly inside the component. This project solves that problem by demonstrating how to:

- Extract geolocation logic into a dedicated custom hook (`useGeolocation`)
- Keep components lean, readable, and focused on rendering
- Promote reusability of non-UI logic across different components

---

## Features

- Fetches the user's real-time GPS coordinates (latitude and longitude) via the browser's Geolocation API
- Displays a clickable link to [OpenStreetMap](https://www.openstreetmap.org/) centered on the retrieved coordinates
- Tracks and displays the number of times the user has requested their position
- Handles loading and error states gracefully (unsupported browser, permission denied, etc.)
- Encapsulates all geolocation logic inside a reusable `useGeolocation` custom hook
- Follows the Single Responsibility Principle for component architecture

---

## Architecture / Key Concepts

The project follows a straightforward **custom hook extraction pattern**:

```
App (UI Component)
 ├── useGeolocation (Custom Hook)
 │    ├── isLoading   → loading state
 │    ├── position    → { lat, lng }
 │    ├── error       → error message
 │    └── getPosition → triggers Geolocation API
 └── countClicks (Local UI State)
```

**Key architectural decisions:**

- **Custom Hook (`useGeolocation`)** — Owns all geolocation-related state (`isLoading`, `position`, `error`) and exposes a `getPosition` function. Any component can consume this hook independently.
- **App Component** — Only manages UI-specific state (`countClicks`) and delegates geolocation concerns entirely to the hook.
- **Event Handler Convention** — The `handleClick` function follows the `handle` prefix naming convention for event handlers, composing the counter increment and the geolocation request.

---

## Tech Stack

| Category       | Technology                                                     |
| -------------- | -------------------------------------------------------------- |
| Language        | JavaScript (ES Modules)                                       |
| UI Library      | React 19                                                      |
| Build Tool      | Vite 7                                                        |
| Linting         | ESLint 9 with `eslint-plugin-react-hooks`, `eslint-plugin-react-refresh` |
| Styling         | Vanilla CSS                                                   |
| Platform        | Node.js                                                       |

---

## Requirements

- **Node.js** >= 18.x
- **npm** >= 9.x (or equivalent package manager)
- A modern browser with [Geolocation API support](https://caniuse.com/geolocation) (Chrome, Firefox, Safari, Edge)
- HTTPS or `localhost` (Geolocation API requires a secure context)

---

## Installation

### Prerequisites

Ensure Node.js and npm are installed on your system.

### Steps

```bash
# Clone the repository
git clone <repository-url>

# Navigate to the project directory
cd 15-geolocation

# Install dependencies
npm install
```

---

## Configuration

This project does not require environment variables or external configuration files. All settings are handled through `vite.config.js` with default values.

---

## Usage

### Start the development server

```bash
npm run dev
```

The app will be available at `http://localhost:5173` (default Vite port).

### Interact with the app

1. Click the **"Get my position"** button
2. Allow the browser's geolocation permission prompt
3. Your GPS coordinates will appear as a clickable link to OpenStreetMap
4. The click counter increments with each request

### Build for production

```bash
npm run build
```

### Preview the production build

```bash
npm run preview
```

---

## Project Structure

```text
15-geolocation/
├── docs/
│   └── LECTURE_STEPS.md          # Step-by-step lecture documentation
├── public/
│   └── vite.svg                  # Vite favicon
├── src/
│   ├── assets/
│   │   └── react.svg             # React logo asset
│   ├── Hooks/
│   │   └── useGeolocation.js     # Custom hook — Geolocation API logic
│   ├── App.jsx                   # Main application component
│   ├── index.css                 # Global styles
│   └── main.jsx                  # React entry point (StrictMode)
├── eslint.config.js              # ESLint configuration
├── index.html                    # HTML shell for Vite
├── package.json                  # Dependencies and scripts
├── README.md
└── vite.config.js                # Vite configuration
```

### Key directories

| Directory      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `src/Hooks/`   | Custom React hooks. Contains `useGeolocation` for browser Geolocation API integration. |
| `src/assets/`  | Static assets (SVG logos).                                   |
| `docs/`        | Lecture notes and step-by-step documentation for the challenge. |
| `public/`      | Public static files served directly by Vite.                 |

---

## Scripts / Commands

| Command           | Description                                      |
| ----------------- | ------------------------------------------------ |
| `npm run dev`     | Start the Vite development server with HMR       |
| `npm run build`   | Build the application for production              |
| `npm run preview` | Preview the production build locally              |
| `npm run lint`    | Run ESLint across the project                     |

---

## Testing & Quality

### Linting

The project uses **ESLint 9** with the following plugins:

- `eslint-plugin-react-hooks` — Enforces the Rules of Hooks
- `eslint-plugin-react-refresh` — Validates components for React Fast Refresh compatibility

Run the linter:

```bash
npm run lint
```

### Testing

No test framework is currently configured. See the [Roadmap](#roadmap--future-improvements) section for planned additions.

---

## Security

- **Geolocation permissions** — The browser prompts the user for permission before accessing location data. The app does not store or transmit coordinates to any external server; all data stays client-side.
- **Secure context** — The Geolocation API requires HTTPS or `localhost`. The Vite dev server satisfies this requirement during development.

---

## Contribution Guidelines

1. Fork the repository
2. Create a feature branch from `main`:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. Make your changes following the existing code style:
   - Use `const` arrow functions for handlers with the `handle` prefix
   - Use descriptive variable and function names
   - Include accessibility attributes where applicable
4. Run the linter before committing:
   ```bash
   npm run lint
   ```
5. Commit using clear, descriptive messages
6. Open a Pull Request with a description of the changes

---

## Roadmap / Future Improvements

- [ ] Add a default position parameter to `useGeolocation` so consumers can provide an initial `{ lat, lng }`
- [ ] Add TypeScript types for the hook return value (`UseGeolocationReturn`)
- [ ] Include additional Geolocation data: `accuracy`, `altitude`, and `timestamp`
- [ ] Improve error handling for permission-denied scenarios with user-friendly messages
- [ ] Add `aria-label` to the "Get my position" button for better accessibility
- [ ] Add unit tests with Vitest for the `useGeolocation` hook
- [ ] Add a visual map component (e.g., Leaflet) to display the position directly in the app

---

## License

This project is for educational purposes as part of the Jonas Schmedtmann React course. No explicit license is provided.

---

## Acknowledgments

- [Jonas Schmedtmann](https://www.udemy.com/user/jonasschmedtmann/) — Course author and original challenge design
- [OpenStreetMap](https://www.openstreetmap.org/) — Map service used for coordinate visualization
- [Vite](https://vite.dev/) — Build tool and development server
- [React](https://react.dev/) — UI library
