# Todo List App

A personal todo list application built with React. The project is intended to let users create, view, edit, complete, and remove tasks.

> **Current status:** This repository is still in the initial development stage. The todo components are placeholders and have not yet been connected to the main application, so the browser currently displays an empty app page.

## Technology used

- React 18
- Create React App / React Scripts
- Font Awesome React icons
- UUID for generating unique task identifiers

## What to install on your local machine

Before cloning the project, install:

1. **Git** - required to clone the repository.
2. **Node.js 18 or newer** - includes npm, which installs dependencies and runs the app. An active Node.js LTS release is recommended.
3. **A modern web browser** - Chrome, Firefox, Edge, or Safari.

Optional but recommended:

- A code editor such as Visual Studio Code.

Confirm that Git, Node.js, and npm are available:

```bash
git --version
node --version
npm --version
```

## Run the app after cloning

1. Clone the repository:

   ```bash
   git clone <repository-url>
   ```

   Replace `<repository-url>` with the HTTPS or SSH URL for this repository.

2. Enter the project directory:

   ```bash
   cd todo-list-app
   ```

3. Install the exact dependency versions recorded in `package-lock.json`:

   ```bash
   npm ci
   ```

   If `npm ci` cannot be used because the lockfile has been changed or removed, run `npm install` instead.

4. Start the development server:

   ```bash
   npm start
   ```

5. Open [http://localhost:3000](http://localhost:3000) in your browser. Create React App usually opens this address automatically.

To stop the development server, press `Ctrl+C` in the terminal.

## Available commands

Run these commands from the project directory:

| Command | Purpose |
| --- | --- |
| `npm start` | Starts the local development server with automatic reloads. |
| `npm test` | Runs the test suite in interactive watch mode. |
| `npm run build` | Creates an optimized production build in the `build` directory. |
| `npm run eject` | Exposes Create React App configuration. This is irreversible and is normally unnecessary. |

## Production build

Create a deployable build with:

```bash
npm run build
```

The generated static files will be placed in the `build` directory. They can be served by a static hosting provider or web server.

## Project structure

```text
todo-list-app/
|-- public/                  Static public assets and HTML template
|-- src/
|   |-- components/         Todo UI component placeholders
|   |-- App.js              Root React component
|   |-- App.css             Application styles
|   `-- index.js            Application entry point
|-- package.json            Dependencies and npm scripts
`-- package-lock.json       Locked dependency versions
```

No database, backend server, environment variables, or external services are currently required to run the project locally.
