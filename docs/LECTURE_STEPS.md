
# 👨🏾‍💻 Project: 15-geolocation

## 📋 Project Overview

### What This Project Does
- Provides a simple React app that fetches the user's current GPS coordinates using the browser's Geolocation API.
- Displays latitude and longitude with a clickable link to OpenStreetMap.
- Tracks how many times the user has requested their position.
- Demonstrates how to extract component logic into a **reusable custom hook** (`useGeolocation`).

### Technology Stack
- **React 19** (with JSX)
- **Vite 7** (build tool and dev server)
- **JavaScript (ES Modules)**
- **CSS** (vanilla)
- **ESLint 9** (linting)

### Key Components
- **`App.jsx`** — Main application component that consumes the `useGeolocation` custom hook, manages click count state, and renders the UI.
- **`Hooks/useGeolocation.js`** — Custom hook encapsulating the Geolocation API logic (`isLoading`, `position`, `error`, `getPosition`).
- **`main.jsx`** — Entry point that renders the `<App />` component inside `<StrictMode>`.

---

## 📑 Table of Contents

- [👨🏾‍💻 Project: 15-geolocation](#-project-15-geolocation)
  - [📋 Project Overview](#-project-overview)
  - [📑 Table of Contents](#-table-of-contents)
  - [📁 Visual Project Tree](#-visual-project-tree)
  - [🧳 Section 01: *Custom Hooks — Challenges*](#-section-01-custom-hooks--challenges)
    <details>
    <summary>Section 01 - Lessons</summary>

      * [📚 Lesson 173: CHALLENGE: 01# useGeolocate](#-173-lesson-173--challenge-01-usegeolocate)
    </details>

---

## 📁 Visual Project Tree

```
📁 15-geolocation/
├── 📁 docs/
│   └── 📄 LECTURE_STEPS.md          # Step-by-step lecture documentation
├── 📁 public/
│   └── 📄 vite.svg                  # Vite favicon
├── 📁 src/
│   ├── 📁 assets/
│   │   └── 📄 react.svg             # React logo asset
│   ├── 📁 Hooks/
│   │   └── 📄 useGeolocation.js     # Custom hook — Geolocation API logic
│   ├── 📄 App.jsx                   # Main application component
│   ├── 📄 index.css                 # Global styles
│   └── 📄 main.jsx                  # React entry point
├── 📄 .gitignore
├── 📄 eslint.config.js
├── 📄 index.html                    # HTML shell for Vite
├── 📄 package.json
├── 📄 package-lock.json
├── 📄 README.md
└── 📄 vite.config.js                # Vite configuration
```

---

## 🧳 Section 13: *Custom Hooks — Challenges*

### 📑 Table of Contents

- [📚 Lesson 173: CHALLENGE: 01# useGeolocate](#-173-lesson-173--challenge-01-usegeolocate)

---

<br>

## 🔧 173. Lesson 173 — *CHALLENGE: 01# useGeolocate*

- [173. Lesson 173 — CHALLENGE: 01# useGeolocate](#-173-lesson-173--challenge-01-usegeolocate)
  - [173.1 Context](#-1731-context)
  - [173.2 Updating code/theory according the context](#️-1732-updating-codetheory-according-the-context)
    - [173.2.1 Initial code from repo](#17321-initial-code-from-repo)
    - [173.2.2 Create `src/Hooks/useGeolocation.js` custom hook](#17322-create-srchooksusegeolocationjs-custom-hook)
    - [173.2.3 Import `useGeolocation` custom hook inside `App` component](#17323-import-usegeolocation-custom-hook-inside-app-component)
  - [173.3 Issues](#-1733-issues)
  - [173.4 Pending Fixes (TODO)](#-1734-pending-fixes-todo)

### 🧠 173.1 Context:

This lesson is a **challenge** that focuses on extracting component-level state logic into a **custom React hook**.

**Problem:** The starter code has **all** the geolocation logic (loading state, position state, error state, and the `getPosition` function) living directly inside the `App` component. This makes the component bloated and the geolocation logic impossible to reuse elsewhere.

**Goal:** Create a custom hook called `useGeolocation` that:
1. Encapsulates the three pieces of state: `isLoading`, `position`, and `error`.
2. Exposes a `getPosition` function that triggers the browser's `navigator.geolocation.getCurrentPosition` API.
3. Returns an object `{ isLoading, position, error, getPosition }` so any component can consume it.

**Why custom hooks matter:**
- They follow the **Single Responsibility Principle** — each hook owns one concern.
- They promote **reusability** — the same geolocation logic can be used in any component.
- They keep components **lean and readable** — `App` only manages UI and its own local state (`countClicks`).

**Solution approach:**
1. Start from the monolithic `App.jsx` that contains everything.
2. Create `src/Hooks/useGeolocation.js` and move the three `useState` calls and the `getPosition` function into it.
3. In `App.jsx`, call `useGeolocation()` and destructure the returned values.
4. Keep `countClicks` in `App` because it is UI-specific, not geolocation-specific.
5. Create a `handleClick` handler that increments the counter **and** calls `getPosition`.

[sandbox repo](https://codesandbox.io/p/sandbox/react-challenge-usegeolocation-starter-lufjm4?file=%2Fsrc%2FApp.js)

### ⚙️ 173.2 Updating code/theory according the context:

#### 173.2.1 Initial code from repo

This is the **starting point** provided by the challenge. Everything lives inside `App.jsx` — notice the empty `useGeolocation` function that we need to implement:

```jsx
/* src/App.jsx */
import { useState } from "react";

function useGeolocation() {}

export default function App() {
  const [isLoading, setIsLoading] = useState(false);
  const [countClicks, setCountClicks] = useState(0);
  const [position, setPosition] = useState({});
  const [error, setError] = useState(null);

  const { lat, lng } = position;

  function getPosition() {
    setCountClicks((count) => count + 1);

    if (!navigator.geolocation)
      return setError("Your browser does not support geolocation");

    setIsLoading(true);
    navigator.geolocation.getCurrentPosition(
      (pos) => {
        setPosition({
          lat: pos.coords.latitude,
          lng: pos.coords.longitude
        });
        setIsLoading(false);
      },
      (error) => {
        setError(error.message);
        setIsLoading(false);
      }
    );
  }

  return (
    <div>
      <button onClick={getPosition} disabled={isLoading}>
        Get my position
      </button>

      {isLoading && <p>Loading position...</p>}
      {error && <p>{error}</p>}
      {!isLoading && !error && lat && lng && (
        <p>
          Your GPS position:{" "}
          <a
            target="_blank"
            rel="noreferrer"
            href={`https://www.openstreetmap.org/#map=16/${lat}/${lng}`}
          >
            {lat}, {lng}
          </a>
        </p>
      )}

      <p>You requested position {countClicks} times</p>
    </div>
  );
}
```

#### 173.2.2 Create `src/Hooks/useGeolocation.js` custom hook:

We extract `isLoading`, `position`, and `error` state — along with the `getPosition` function — into a dedicated custom hook. The hook returns an object so consumers can destructure exactly what they need:

```jsx
/* src/Hooks/useGeolocation.js */
import { useState } from "react";                                 // 👈🏽 ✅

export default function useGeolocation() {                        // 👈🏽 ✅
  const [isLoading, setIsLoading] = useState(false);              // 👈🏽 ✅
  const [position, setPosition] = useState({});                   // 👈🏽 ✅
  const [error, setError] = useState(null);                       // 👈🏽 ✅


  function getPosition() {                                        // 👈🏽 ✅
    if (!navigator.geolocation) return setError("Your browser does not support geolocation");

    setIsLoading(true);
    navigator.geolocation.getCurrentPosition(
      (pos) => {
        setPosition({
          lat: pos.coords.latitude,
          lng: pos.coords.longitude,
        });
        setIsLoading(false);
      },
      (error) => {
        setError(error.message);
        setIsLoading(false);
      },
    );
  }

  return { isLoading, position, error, getPosition };             // 👈🏽 ✅
}
```

#### 173.2.3 Import `useGeolocation` custom hook inside `App` component:

Now `App` is **clean and focused** — it only manages its own `countClicks` state and delegates all geolocation concerns to the hook. Key changes:

1. **`handleClick`** — Renamed from `getPosition` to follow the `handle` prefix convention for event handlers. It increments the counter **and** calls `getPosition` from the hook.
2. **`onClick={handleClick}`** — The button now calls the local handler instead of the raw geolocation function.
3. **`useGeolocation()` destructuring** — We destructure `position` with an alias `{ lat, lng }` inline, which eliminates the separate `const { lat, lng } = position` line.
4. **Removed all geolocation state** — `isLoading`, `position`, `error`, and `getPosition` are now provided by the hook.

```jsx
/* src/App.jsx */
import { useState } from "react";
import useGeolocation from "./hooks/useGeolocation";              // 👈🏽 ✅ (3)

export default function App() {
  const {
    isLoading,
    position: { lat, lng },                                       // 👈🏽 ✅ (3)
    error,
    getPosition,
  } = useGeolocation();                                           // 👈🏽 ✅ (3)

  const [countClicks, setCountClicks] = useState(0);

  const handleClick = () => {                                     // 👈🏽 ✅ (1)
    setCountClicks((count) => count + 1);                         // 👈🏽 ✅ (1)
    getPosition();                                                // 👈🏽 ✅ (4)
  };

  return (
    <div>
      <button onClick={handleClick} disabled={isLoading}>         {/* 👈🏽 ✅ (2) */}
        Get my position
      </button>

      {isLoading && <p>Loading position...</p>}
      {error && <p>{error}</p>}
      {!isLoading && !error && lat && lng && (
        <p>
          Your GPS position:{" "}
          <a target="_blank" rel="noreferrer" href={`https://www.openstreetmap.org/#map=16/${lat}/${lng}`}>
            {lat}, {lng}
          </a>
        </p>
      )}

      <p>You requested position {countClicks} times</p>
    </div>
  );
}
```

### 🐞 173.3 Issues:
- No issues encountered during this lesson.

| Issue | Status | Log/Error |
|---|---|---|
| N/A | — | — |

### 🧱 173.4 Pending Fixes (TODO)

- [ ] Add default position parameter to `useGeolocation` so consumers can provide an initial `{ lat, lng }`.
- [ ] Add TypeScript types for the hook return value (`UseGeolocationReturn`).
- [ ] Consider adding `accuracy`, `altitude`, and `timestamp` from the Geolocation API.
- [ ] Add error handling for permission denied scenarios with a user-friendly message.
- [ ] Add `aria-label` to the "Get my position" button for better accessibility.

[↑ top - [Lesson 173 — CHALLENGE: 01# useGeolocate]](#-173-lesson-173--challenge-01-usegeolocate)

--- 

<br>
<br>
<br>

🔥 🔥 🔥 

<br>

## 🔧 XXX. Lesson XXX — *{{TITLE_NAME}}*

### 🧠 XXX.1 Context:

### ⚙️ XXX.2 Updating code/theory according the context:

#### XXX.2.1
```jsx
/*  */

```

#### XXX.2.2
```jsx
/*  */

```

#### XXX.2.3
```jsx
/*  */

```

#### XXX.2.4
```jsx
/*  */

```

### 🐞 XXX.3 Issues:
- **first issue**: something..

| Issue | Status | Log/Error |
|---|---|---|

### 🧱 XXX.4 Pending Fixes (TODO)

- [ ]
