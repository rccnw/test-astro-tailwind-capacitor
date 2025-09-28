# test-astro-tailwind

A project combining Astro (static site generator), Tailwind CSS (utility-first CSS framework), and Capacitor (cross-platform native runtime) to create a web application that can be deployed both as a website and as a mobile app.

## ⚠️ CRITICAL REQUIREMENT
**If any terminal command produces an error, STOP immediately and troubleshoot before proceeding.** 
Continuing with errors will compound problems and make debugging much harder. Each step must complete successfully before moving to the next step.

## Project Setup Steps

### Phase 1: Core Astro Setup

#### 1. Initialize Package.json
```bash
# Initialize npm project in root directory
npm init -y
# Purpose: Creates package.json for dependency management and scripts
```

#### 2. Install Core Astro Dependencies
```bash
# Install minimal Astro dependencies
npm install astro typescript

# Ensure optional binaries (like Rollup on Windows) are installed
npm install --include=optional
# Purpose: Installs the Astro framework with TypeScript support and required optional dependencies
```

#### 3. Create Basic Project Structure
```bash
# Create standard Astro folder structure
mkdir src
mkdir src/pages
mkdir src/components
mkdir src/layouts
mkdir public
# Purpose: Establishes the standard Astro project organization
```

#### 4. Configure Package.json Scripts
```bash
# Manually edit package.json to add these scripts to the "scripts" section:
{
  "scripts": {
    "dev": "astro dev",
    "start": "astro dev", 
    "build": "astro build",
    "preview": "astro preview"
  }
}
# Purpose: Enables npm run commands for development workflow
```

#### 5. Create Astro Configuration
```bash
# Create astro.config.mjs file with this exact content:
import { defineConfig } from 'astro/config';

export default defineConfig({
  output: 'static'
});
# Purpose: Configures Astro for static site generation (required for Capacitor)
```

#### 6. Create TypeScript Configuration
```bash
# Create tsconfig.json file with this exact content:
{
  "extends": "astro/tsconfigs/strict",
  "compilerOptions": {
    "jsx": "preserve"
  }
}
# Purpose: Enables TypeScript compilation and IDE support for Astro
```

#### 7. Create Basic Index Page
```bash
# Create src/pages/index.astro file with this exact content:
---
// Component script (runs on server)
---

<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="generator" content={Astro.generator} />
    <title>Astro + Tailwind + Capacitor</title>
  </head>
  <body>
    <main>
      <h1>Welcome to Astro!</h1>
      <p>This is a basic test page to verify Astro is working.</p>
    </main>
  </body>
</html>
# Purpose: Provides a test page to verify Astro is working
```

#### 8. Test Basic Astro Setup
```bash
# Start development server
npm run dev
# Expected result: 
# - Server starts on http://localhost:4321
# - Browser shows "Welcome to Astro!" heading
# - No console errors in terminal
# - Page loads without JavaScript errors
# Purpose: Validate that Astro is properly configured before proceeding
```

### Phase 2: Add Tailwind CSS

#### 9. Install Tailwind Dependencies
```bash
# Install Tailwind CSS and all required packages
npm install @astrojs/tailwind tailwindcss autoprefixer
# Purpose: Adds Tailwind CSS framework and its dependencies for styling
```

#### 10. Configure Tailwind Integration
```bash
# Update astro.config.mjs to this exact content:
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  output: 'static',
  integrations: [tailwind()]
});

# Create tailwind.config.mjs with this exact content:
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{astro,html,js,jsx,md,mdx,svelte,ts,tsx,vue}'],
  theme: {
    extend: {},
  },
  plugins: [],
}
# Purpose: Integrates Tailwind CSS with Astro build process
```

#### 11. Test Tailwind Integration
```bash
# Update src/pages/index.astro to use Tailwind classes:
<body class="bg-blue-50 p-8">
  <main class="max-w-md mx-auto bg-white rounded-xl shadow-lg p-6">
    <h1 class="text-2xl font-bold text-gray-900 mb-4">Welcome to Astro!</h1>
    <p class="text-gray-600">This is a test page with Tailwind CSS styling.</p>
  </main>
</body>

# Start dev server and verify Tailwind styles work
npm run dev
# Expected result:
# - Page has blue background and centered white card
# - Text styling matches Tailwind classes
# - No CSS-related errors in browser console
# Purpose: Validate Tailwind CSS is working before adding Capacitor
```

### Phase 3: Add Capacitor for Mobile

#### 12. Install Capacitor Dependencies
```bash
# Install Capacitor core and CLI
npm install @capacitor/core @capacitor/cli
# Purpose: Adds Capacitor framework for mobile app capabilities
```

#### 13. Initialize Capacitor
```bash
# Initialize Capacitor with specific configuration
npx cap init "Astro Tailwind App" "com.example.astrotailwind" --web-dir dist
# Purpose: Creates capacitor.config.ts and sets up native app configuration without interactive prompts
```

#### 14. Configure Build Output
```bash
# Verify astro.config.mjs has static output (should already be set from step 10):
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  output: 'static',  // This line is critical for Capacitor
  integrations: [tailwind()]
});
# Purpose: Ensures Astro builds static files that Capacitor can use (no SSR)
```

#### 15. Add Mobile Platforms
```bash
# Install platform-specific packages (install both even if you only target one)
npm install @capacitor/ios @capacitor/android

# Add iOS platform (macOS required)
npx cap add ios

# Add Android platform
npx cap add android
# Purpose: Installs Capacitor platform packages and creates native project folders
```

#### 16. Build and Sync
```bash
# Build web assets and sync to mobile platforms
npm run build
# Expected result: dist/ folder created with static files

npx cap sync
# Expected result: 
# - iOS and Android folders updated with latest web assets
# - No sync errors in terminal
# - Native dependencies updated
# Purpose: Copies built web assets to native platform projects
```

#### 17. Test Mobile Platforms
```bash
# Open iOS project (macOS only)
npx cap open ios
# Expected result: Xcode opens with the project loaded

# Open Android project  
npx cap open android
# Expected result: Android Studio opens with the project loaded
# 
# In the simulators/emulators you should see:
# - Your Astro app loads in a mobile webview
# - Tailwind styles render correctly
# - App behaves like a native mobile app
# Purpose: Launch native IDEs for mobile testing and deployment
```

## Project Structure (Final)
```
├── src/                    # Astro source files
│   ├── components/         # Reusable components
│   ├── layouts/           # Page layouts
│   └── pages/             # Routes (index.astro, etc.)
├── public/                # Static assets
├── dist/                  # Built files (generated)
├── ios/                   # iOS native project (generated)
├── android/               # Android native project (generated)
├── astro.config.mjs       # Astro configuration
├── tailwind.config.mjs    # Tailwind CSS configuration
├── capacitor.config.ts    # Capacitor configuration
├── tsconfig.json          # TypeScript configuration
├── package.json           # Dependencies and scripts
└── README.md              # This setup guide
```

## Development Workflow
1. **Web Development**: `npm run dev` for live development
2. **Build for Production**: `npm run build` 
3. **Mobile Sync**: `npx cap sync` after building
4. **Mobile Testing**: Use `npx cap open ios/android` to test on devices

## Validation Checkpoints
- ✅ After Step 8: Astro dev server starts and shows basic page
- ✅ After Step 11: Tailwind styles appear on the page  
- ✅ After Step 16: Mobile platforms build successfully
- ✅ Final: App runs in mobile simulators/devices

## Notes
- Each phase should be tested before proceeding to the next
- Keep the project structure simple and standard
- Avoid complex workspace configurations for this use case