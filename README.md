# Cryptocurrency Tracker (Tugas Crypto)

A mobile-friendly and responsive cryptocurrency tracker application built with Ionic Vue, TypeScript, and Vite. Track real-time prices, 24-hour fluctuations, and market rankings of popular cryptocurrencies powered by the public CoinLore API.

## Features

- Real-time cryptocurrency data fetching from CoinLore API
- Display coin ranking, name, symbol, and current USD price
- 24-hour percentage change tracking with color-coded indicators (green for gains, red for losses)
- Coin logo / icon display with fallback handling for missing icons
- Manual data refresh button with instant UI updates
- Clean grid-based list layout optimized for mobile and desktop screens
- Cross-platform ready (Web, Android, iOS) via Capacitor
- Modern single-page routing powered by Vue Router and Ionic Vue

## Key Constraints

- Requires active internet connection to fetch real-time crypto prices from `https://api.coinlore.net`
- Data retrieval relies on rate limits and availability of the public CoinLore API
- Icon fallbacks default to Bitcoin logo if the coin specific logo fails to load
- Numbers and percentage changes are formatted based on raw API decimal returns

## Installation

### Prerequisites

- Node.js 18 or later
- npm or yarn

### Setup Steps

1. Navigate to the project directory:

```bash
cd crypto-list
```

2. Install dependencies:

```bash
npm install
```

## Running the Project

### Development Server

Start the local development server with hot module reloading:

```bash
npm run dev
```

The application will be available at `http://localhost:5173` (or the port indicated in your terminal).

### Production Build

Build and type-check the application for production:

```bash
npm run build
```

The optimized build output will be stored in the `dist/` directory.

### Preview Production Build

Preview the generated production build locally:

```bash
npm run preview
```

### Run Tests

Execute unit tests with Vitest:

```bash
npm run test:unit
```

Execute end-to-end tests with Cypress:

```bash
npm run test:e2e
```

### Code Quality

Run ESLint to check for code issues:

```bash
npm run lint
```

## Project Structure

```
crypto-list/
├── index.html                 # Main HTML entry point
├── package.json               # Project dependencies, scripts, and metadata
├── tsconfig.json              # TypeScript root configuration
├── tsconfig.node.json         # TypeScript configuration for Node/Vite tools
├── vite.config.ts             # Vite build configuration and plugins
├── capacitor.config.ts        # Capacitor configuration for mobile builds
├── ionic.config.json          # Ionic CLI project configuration
├── public/                    # Static assets
│   └── favicon.png            # App favicon
├── src/
│   ├── main.ts                # Application entry point - initializes Vue & Ionic
│   ├── App.vue                # Root Vue application component with IonApp
│   ├── vite-env.d.ts          # Vite client type definitions
│   ├── router/
│   │   └── index.ts           # Route definitions using Vue Router
│   ├── theme/
│   │   └── variables.css      # Ionic design tokens and theme variables
│   └── views/
│       └── HomePage.vue       # Crypto list view, API fetching, and UI rendering
└── tests/                     # Automated test suites
    ├── e2e/                   # Cypress end-to-end tests and fixtures
    └── unit/                  # Vitest unit test specifications
```

## Architecture

The application follows a reactive, single-direction data flow with Vue Composition API and Axios:

```
User Action (Refresh / Mount) → HomePage.vue → axios.get(CoinLore API)
                                      ↓
                               cryptoList.value
                                      ↓
                              Reactive DOM Render
```

### Core Layers

**main.ts** - Application bootstrapper
- Initializes the Vue 3 application instance
- Registers `@ionic/vue` and global CSS components
- Mounts the router to `#app`

**router/index.ts** - Navigation layer
- Configures routes using `createRouter` and `createWebHistory`
- Maps root path `/` to `HomePage.vue`

**views/HomePage.vue** - Primary UI & logic component
- Manages state for cryptocurrency tickers (`cryptoList`)
- Orchestrates asynchronous HTTP calls via `axios`
- Formats price, rank, and 24h percentage badges
- Handles image fallback gracefully on network or resource errors

**theme/variables.css** - Styling & theme configuration
- Defines CSS variables for Ionic colors (primary, secondary, dark, light)
- Applies custom styling for crypto card items and metrics

## Technologies Used

- **Language**: TypeScript (~5.9.0)
- **Framework**: Vue 3 (Composition API)
- **UI Toolkit**: Ionic Framework 8 (`@ionic/vue`, `ionicons`)
- **HTTP Client**: Axios
- **Mobile Runtime**: Capacitor 7 (`@capacitor/core`, `@capacitor/cli`)
- **Bundler / Dev Server**: Vite 5
- **Unit Testing**: Vitest & Vue Test Utils
- **E2E Testing**: Cypress
- **Code Linter**: ESLint with Vue & TypeScript plugins

## Development Notes

### Code Style

- Vue Single File Components (SFC) located in `src/views/`
- Component script using TypeScript (`<script lang="ts">`) and Composition API
- Ionic components prefixed with `ion-*` (`ion-page`, `ion-content`, `ion-grid`)
- Strict type definitions with interfaces (e.g., `Crypto` interface in `HomePage.vue`)

### Testing Strategy

- Unit testing with Vitest for components and helpers
- End-to-end testing with Cypress (`npm run test:e2e`)
- Type checking with `vue-tsc` during production builds

### Mobile Deployment (Capacitor)

To sync and run on mobile platforms (Android/iOS):

```bash
# Add platforms (if not already added)
npx cap add android
npx cap add ios

# Sync web assets to native project
npm run build
npx cap sync

# Open in Android Studio / Xcode
npx cap open android
npx cap open ios
```

## Troubleshooting

### API Request Failed / No Data Displayed

If the list does not load or you see an alert:
- Check your internet connection
- Verify that `https://api.coinlore.net/api/tickers/` is accessible in your browser
- Check browser developer tools (F12) Network/Console tab for any blocked CORS or network requests

### Port Already in Use

If port 5173 is already in use by another application:
- Vite will automatically attempt to bind to the next available port (e.g., 5174)
- Or manually specify a custom port: `npx vite --port 3000`

### Build / Type Errors

If you encounter TypeScript errors during `npm run build`:
- Ensure Node.js version is compatible (v18+)
- Re-run `npm install` to ensure matching package versions
- Run `npx vue-tsc --noEmit` to identify exact type discrepancies

## License

This project is provided for educational and development purposes.
