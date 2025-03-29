# Gen AI Analytics Dashboard

## Overview

The Gen AI Analytics Dashboard is a React-based single-page application (SPA) that allows users to input natural language queries (e.g., "Show sales by region") and visualize the results through interactive charts. The app simulates AI-powered query processing, stores query history in `localStorage`, and supports light/dark theme toggling. It’s built with React, Redux, Chart.js, and Tailwind CSS, and is deployed on GitHub Pages.

### Live Demo
- **Deployment Link**: [https://ksaimounika.github.io/gen-ai-analytics-dashboard/]

### GitHub Repository
- **Repository Link**: [https://github.com/KSAIMOUNIKA/gen-ai-analytics-dashboard]

## Features
- **Natural Language Query Input**: Users can input queries with AI-powered suggestions (e.g., "Show sales by region", "Revenue this year").
- **Query History**: Past queries are stored in `localStorage` and can be re-run with a single click.
- **Interactive Charts**: Results are displayed using Chart.js with options to switch between line, bar, and area charts.
- **Theme Toggle**: Supports light and dark modes with smooth transitions.
- **Loading/Error States**: Handles different states (idle, loading, success, error) with appropriate UI feedback.

## Setup Instructions

### Prerequisites
- Node.js (v14 or higher) and npm.
- A modern web browser (e.g., Chrome, Firefox).
- Git installed for version control.

### Running Locally
1. Clone the repository:
   ```
   git clone https://github.com/KSAIMOUNIKA/gen-ai-analytics-dashboard.git
   ```
2. Navigate to the project directory:
   ```
   cd gen-ai-analytics-dashboard
   ```
3. Install dependencies:
   ```
   npm install
   ```
4. Start the development server:
   ```
   npm start
   ```
   The app will open at `http://localhost:3000`.

### Deployment
The app is deployed on GitHub Pages with the following steps:
1. **Ensure the Project is Built**:
   - Run the build command to generate the production-ready static files:
     ```
     npm run build
     ```
   - This creates a `build/` folder containing the bundled static files (HTML, CSS, JS).

2. **Install the `gh-pages` Package**:
   - Add the `gh-pages` package as a dev dependency to help with deployment:
     ```
     npm install --save-dev gh-pages
     ```
   - Update the `package.json` file to include deployment scripts and a `homepage` field:
     ```json
     {
       "homepage": "https://KSAIMOUNIKA.github.io/gen-ai-analytics-dashboard/",
       "scripts": {
         "predeploy": "npm run build",
         "deploy": "gh-pages -d build",
         "start": "react-scripts start",
         "build": "react-scripts build",
         "test": "react-scripts test",
         "eject": "react-scripts eject"
       }
     }
     ```
     
3. **Set Up SPA Routing**:
   - GitHub Pages does not natively support SPA routing, so we need to handle client-side routing by ensuring all routes redirect to `index.html`.
   - Create a `404.html` file in the `build/` folder after running `npm run build`:
     - Copy `build/index.html` to `build/404.html`:
       ```
       copy build\index.html build\404.html
       ```
     - This ensures that GitHub Pages serves `index.html` for all routes, allowing React Router (or manual routing) to handle navigation.

4. **Push the Project to GitHub**:
   - Ensure your project is a Git repository and pushed to GitHub:
     ```
     git add .
     git commit -m "Prepare project for GitHub Pages deployment"
     git push origin main
     ```

5. **Deploy to GitHub Pages**:
   - Run the deploy command:
     ```
     npm run deploy
     ```
   - This uses the `gh-pages` package to push the contents of the `build/` folder to a `gh-pages` branch in your repository.

6. **Configure GitHub Pages**:
   - Go to your repository on GitHub: `https://github.com/KSAIMOUNIKA/gen-ai-analytics-dashboard`.
   - Navigate to **Settings** > **Pages**.
   - Under **Source**, select the `gh-pages` branch and set the folder to `/ (root)`.
   - Save the settings. GitHub Pages will provide a URL (e.g., `https://KSAIMOUNIKA.github.io/gen-ai-analytics-dashboard/`).

7. **Test the Deployment**:
   - Visit the GitHub Pages URL (e.g., `https://KSAIMOUNIKA.github.io/gen-ai-analytics-dashboard/`) to confirm the app is live and working.

## Dependency Management
- Initially faced dependency issues with `react-scripts@3.0.1`, which introduced numerous vulnerabilities.
- Upgraded to `react-scripts@5.0.1` to resolve most vulnerabilities and ensure compatibility with React 18.
- Explicitly set `postcss` to `^8.4.47` to address a known vulnerability.
- **Note on Remaining Vulnerabilities**: After upgrading dependencies, a few vulnerabilities remain in transitive dependencies (e.g., `webpack-dev-server`, `terser`). These primarily affect development tools and the build process, not the production runtime of the app. Given the 48-hour hackathon timeline, I prioritized functionality, UI/UX, and code quality over fully resolving these issues, as they do not impact the deployed app on GitHub Pages, which is a client-side SPA with no sensitive data.

## Evaluation Focus

### React Component Structure
- The app is structured into modular components:
  - `App`: Orchestrates the layout and theme toggling.
  - `QueryInput`: Handles user input, suggestions, and query submission.
  - `QueryHistory`: Displays past queries in a collapsible accordion.
  - `ResultDisplay`: Renders charts and manages loading/error states.
- Components are organized in a dedicated directory, following a single-responsibility principle. PropTypes are used for type-checking props.

### State Management Efficiency
- **Redux**: Used for global state management, with actions, a reducer, and a store organized in a dedicated directory.
  - Actions: `SUBMIT_QUERY`, `SET_RESULT`, `SET_ERROR`, `ADD_TO_HISTORY`, `SET_CHART_TYPE`, `TOGGLE_THEME`.
  - Reducer: Manages state transitions, with `localStorage` integration for query history.
- Local state (via `useState`) is used minimally in components, ensuring global state is centralized in Redux.

### UI/UX Design
- **Inspiration**: The design is inspired by modern analytics dashboards on Behance/Dribbble, focusing on clean layouts, gradient backgrounds, and intuitive interactions.
- **Implementation**:
  - **Layout**: A responsive grid layout (`grid-cols-1 md:grid-cols-2`) ensures the query input and results are side-by-side on larger screens.
  - **Styling**: Tailwind CSS is used for utility-first styling, with gradient backgrounds and smooth transitions.
  - **Interactivity**: Features like hover effects, animations, and a collapsible query history enhance user engagement.
  - **Accessibility**: ARIA labels and keyboard navigation improve accessibility.
- **Theme Support**: Light and dark modes are implemented with dynamic color schemes, ensuring readability.

### Code Quality
- **Readability**: Code is organized into separate files with clear naming conventions.
- **Maintainability**: Modular components and Redux logic make the codebase easy to extend.
- **Error Handling**: Robust error handling in `mockAIProcess` and UI feedback for error states.
- **Performance**: Chart.js instances are destroyed on component unmount to prevent memory leaks.
- **Best Practices**: PropTypes ensure type safety, and the app avoids unnecessary re-renders.

### Creativity in Simulating AI Query Interaction
- **Mock AI Processing**: The `mockAIProcess` function simulates AI query processing with a 1-second delay.
- **Dynamic Suggestions**: The `QueryInput` component filters suggestions based on user input.
- **Query Parsing**: The app parses queries to return relevant data (e.g., "sales" returns a bar chart).
- **Result Visualization**: Charts include dynamic tooltips showing percentage changes.
- **History Persistence**: Storing and re-running queries from `localStorage` mimics a real analytics tool.

## Approach Explanation
- **Traditional Structure**: Refactored the original single-file `index.html` into a traditional React project using `create-react-app`, with separate files for components, Redux logic, and utilities.
- **State Management**: Used Redux for centralized state management, with local state for component-specific needs.
- **UI/UX Focus**: Prioritized a modern design with Tailwind CSS, incorporating gradients, animations, and theme support.
- **AI Simulation**: Implemented a creative `mockAIProcess` function to simulate AI query processing, with dynamic data generation for "revenue" queries.
- **Dependency Management**: Upgraded to `react-scripts@5.0.1` to resolve most vulnerabilities and ensure compatibility with React 18. Noted remaining vulnerabilities in transitive dependencies but prioritized functionality due to the 48-hour hackathon timeline.
- **Deployment**: Hosted on GitHub and deployed on GitHub Pages using the `gh-pages` package, with SPA routing handled via a `404.html` file.

## Future Improvements
- Integrate a real AI API for query processing.
- Enhance the suggestion system with advanced AI-driven recommendations.
- Add more chart customization options (e.g., date range filters, data export).
- Migrate to a more modern build tool like Vite for faster development and fewer dependency issues.
- Fully resolve remaining vulnerabilities by overriding transitive dependencies or updating to a newer version of `react-scripts` (if available).