# Next.js Code Quality Setup Guide

> **Complete ESLint + Prettier + VS Code Configuration for Next.js 15+ Projects**
>
> This guide provides a comprehensive, production-ready code quality setup with Airbnb-style linting rules, automatic formatting, and VS Code integration.

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Step-by-Step Setup](#step-by-step-setup)
- [Configuration Files](#configuration-files)
- [VS Code Integration](#vs-code-integration)
- [Usage & Commands](#usage--commands)
- [Troubleshooting](#troubleshooting)
- [Customization Options](#customization-options)

---

## 🎯 Overview

This setup provides:
- **ESLint 9+** with flat config format and Airbnb-style rules
- **Prettier** with Tailwind CSS integration
- **VS Code** workspace optimization
- **Next.js 15+ & React 19** compatibility
- **Comprehensive ignore patterns** for build outputs, dependencies, and IDE files
- **Auto-fix capabilities** for common code issues

### Benefits:
- ✅ Zero configuration conflicts with modern Next.js
- ✅ Professional-grade code quality standards
- ✅ Automatic code formatting on save
- ✅ Consistent team development experience
- ✅ Production-ready linting rules

---

## 📦 Prerequisites

### Required Dependencies

```bash
# Core Next.js project
npm install next@latest react@latest react-dom@latest

# ESLint dependencies
npm install --save-dev eslint@^9.0.0
npm install --save-dev @eslint/js
npm install --save-dev eslint-config-next
npm install --save-dev eslint-plugin-react
npm install --save-dev eslint-plugin-react-hooks
npm install --save-dev eslint-plugin-jsx-a11y
npm install --save-dev eslint-plugin-import

# Prettier dependencies
npm install --save-dev prettier
npm install --save-dev prettier-plugin-tailwindcss

# Optional: Tailwind CSS (if using)
npm install --save-dev tailwindcss postcss autoprefixer
```

### System Requirements:
- Node.js 18+
- npm 9+ or yarn 3+
- VS Code (recommended)

---

## 📁 Project Structure

Your project should have this structure after setup:

```
your-nextjs-project/
├── .vscode/
│   ├── extensions.json     # Recommended VS Code extensions
│   └── settings.json       # Workspace settings
├── app/                    # Next.js 13+ app directory
├── components/            # shadcn components
├── lib/                   # Utility functions
├── public/               # Static assets
├── .prettierrc           # Prettier configuration
├── eslint.config.mjs     # ESLint configuration (flat config)
├── jsconfig.json         # JavaScript project configuration
├── next.config.mjs       # Next.js configuration
├── package.json          # Dependencies and scripts
├── postcss.config.mjs    # PostCSS configuration (if using Tailwind)
└── tailwind.config.js    # Tailwind configuration (if using)
```

---

## 🚀 Step-by-Step Setup

### Step 1: Initialize Next.js Project (if new project)

```bash
# Create new Next.js project
npx create-next-app@latest my-project --typescript --tailwind --eslint --app

# Navigate to project
cd my-project

# Or if existing project, ensure you have the base dependencies
npm install
```

### Step 2: Install Code Quality Dependencies

```bash
# Install all ESLint dependencies
npm install --save-dev eslint@^9.0.0 @eslint/js eslint-config-next eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-jsx-a11y eslint-plugin-import

# Install Prettier dependencies
npm install --save-dev prettier prettier-plugin-tailwindcss
```

### Step 3: Remove Old Configuration Files

```bash
# Remove deprecated .eslintignore if it exists
rm .eslintignore

# Remove old .eslintrc.json if it exists (ESLint 9+ uses flat config)
rm .eslintrc.json
```

### Step 4: Create Configuration Files

Create each configuration file as detailed in the [Configuration Files](#configuration-files) section below.

### Step 5: Update package.json Scripts

Add these scripts to your `package.json`:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check ."
  }
}
```

### Step 6: Setup VS Code Integration

Create the VS Code configuration files as shown in the [VS Code Integration](#vs-code-integration) section.

### Step 7: Initial Lint and Format

```bash
# Run initial linting (will show existing issues)
npm run lint

# Auto-fix what can be automatically fixed
npm run lint:fix

# Format all files
npm run format
```

---

## ⚙️ Configuration Files

### 1. ESLint Configuration (`eslint.config.mjs`)

```javascript
import js from '@eslint/js';
import nextConfig from 'eslint-config-next/core-web-vitals';

export default [
  // Base JavaScript recommended rules
  js.configs.recommended,

  // Next.js specific rules
  ...nextConfig,

  // Custom configuration
  {
    languageOptions: {
      ecmaVersion: 'latest',
      sourceType: 'module',
      globals: {
        // Node.js globals
        console: 'readonly',
        process: 'readonly',
        Buffer: 'readonly',
        __dirname: 'readonly',
        __filename: 'readonly',
        global: 'readonly',

        // Browser globals
        window: 'readonly',
        document: 'readonly',
        localStorage: 'readonly',
        sessionStorage: 'readonly',
        fetch: 'readonly',

        // Next.js specific
        JSX: 'readonly',
      },
    },

    rules: {
      // Airbnb-style rules adapted for Next.js

      // Code Style
      'indent': ['error', 2, {
        SwitchCase: 1,
        VariableDeclarator: 1,
        outerIIFEBody: 1,
        FunctionDeclaration: { parameters: 1, body: 1 },
        FunctionExpression: { parameters: 1, body: 1 },
      }],
      'quotes': ['error', 'single', { avoidEscape: true }],
      'semi': ['error', 'always'],
      'comma-dangle': ['error', 'always-multiline'],
      'max-len': ['warn', { code: 120, ignoreUrls: true }],

      // Variables
      'no-unused-vars': ['warn', {
        argsIgnorePattern: '^_',
        varsIgnorePattern: '^_',
      }],
      'no-var': 'error',
      'prefer-const': 'error',

      // Functions
      'arrow-spacing': 'error',
      'prefer-arrow-callback': 'error',
      'no-param-reassign': ['warn', { props: false }],

      // Objects and Arrays
      'object-shorthand': 'error',
      'prefer-destructuring': ['warn', {
        array: false,
        object: true,
      }],

      // Imports
      'import/order': ['warn', {
        groups: [
          'builtin',
          'external',
          'internal',
          'parent',
          'sibling',
          'index',
        ],
        'newlines-between': 'always',
        alphabetize: {
          order: 'asc',
          caseInsensitive: true,
        },
      }],
      'import/prefer-default-export': 'off',
      'import/no-unresolved': 'off', // Let Next.js handle resolution

      // React Rules
      'react/jsx-filename-extension': ['error', { extensions: ['.jsx', '.js'] }],
      'react/react-in-jsx-scope': 'off', // Not needed in Next.js 13+
      'react/prop-types': 'off', // Using TypeScript or not enforcing
      'react/jsx-props-no-spreading': 'warn',
      'react/function-component-definition': ['error', {
        namedComponents: 'arrow-function',
        unnamedComponents: 'arrow-function',
      }],
      'react/jsx-one-expression-per-line': 'off',
      'react/jsx-wrap-multilines': ['error', {
        declaration: 'parens-new-line',
        assignment: 'parens-new-line',
        return: 'parens-new-line',
        arrow: 'parens-new-line',
        condition: 'parens-new-line',
        logical: 'parens-new-line',
        prop: 'parens-new-line',
      }],

      // React Hooks
      'react-hooks/rules-of-hooks': 'error',
      'react-hooks/exhaustive-deps': 'warn',

      // Accessibility
      'jsx-a11y/anchor-is-valid': ['error', {
        components: ['Link'],
        specialLink: ['hrefLeft', 'hrefRight'],
        aspects: ['invalidHref', 'preferButton'],
      }],
      'jsx-a11y/click-events-have-key-events': 'warn',
      'jsx-a11y/no-static-element-interactions': 'warn',

      // Code Quality
      'no-console': 'warn',
      'no-debugger': 'error',
      'no-alert': 'warn',
      'eqeqeq': ['error', 'always'],
      'no-nested-ternary': 'warn',
      'prefer-template': 'error',
      'no-useless-concat': 'error',

      // Error Prevention
      'no-duplicate-imports': 'error',
      'no-unreachable': 'error',
      'no-unused-expressions': 'error',
      'no-useless-return': 'error',
      'default-case': 'warn',

      // Next.js Specific (handled by eslint-config-next)
      '@next/next/no-img-element': 'warn',
      '@next/next/no-page-custom-font': 'warn',

      // Promise handling
      'no-promise-executor-return': 'error',
      'no-await-in-loop': 'warn',
      'no-useless-catch': 'warn',
    },
  },

  // Comprehensive ignore patterns
  {
    ignores: [
      // Build outputs
      '.next/',
      'out/',
      'build/',
      'dist/',
      '.nuxt/',
      '.output/',

      // Dependencies
      'node_modules/',
      'bower_components/',
      'jspm_packages/',

      // Generated files
      '*.min.js',
      '*.min.css',
      '*.bundle.js',
      '*.chunk.js',
      '*.map',

      // Cache directories
      '.npm/',
      '.yarn/',
      '.pnp/',
      '.pnp.js',
      '.cache/',
      '.parcel-cache/',
      '.turbo/',

      // Environment files
      '.env',
      '.env.local',
      '.env.development.local',
      '.env.test.local',
      '.env.production.local',

      // IDE and OS files
      '.vscode/',
      '.idea/',
      '*.swp',
      '*.swo',
      '*~',
      '.DS_Store',
      '.DS_Store?',
      '._*',
      '.Spotlight-V100',
      '.Trashes',
      'ehthumbs.db',
      'Thumbs.db',

      // Logs
      'logs/',
      '*.log',
      'npm-debug.log*',
      'yarn-debug.log*',
      'yarn-error.log*',
      'lerna-debug.log*',

      // Coverage reports
      'coverage/',
      '.nyc_output/',
      '*.lcov',

      // Temporary files
      'tmp/',
      'temp/',
      '*.tmp',
      '*.temp',

      // Lock files
      'package-lock.json',
      'yarn.lock',
      'pnpm-lock.yaml',

      // Documentation builds
      '_book/',
      '.docusaurus/',

      // Storybook
      'storybook-static/',

      // Testing
      '.jest/',
      '__snapshots__/',

      // Misc
      '*.tsbuildinfo',
      '.eslintcache',
    ],
  },
];
```

### 2. Prettier Configuration (`.prettierrc`)

```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 120,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "always",
  "endOfLine": "lf",
  "singleAttributePerLine": true,
  "plugins": ["prettier-plugin-tailwindcss"],
  "overrides": [
    {
      "files": "*.json",
      "options": {
        "singleQuote": false,
        "printWidth": 80
      }
    },
    {
      "files": "*.md",
      "options": {
        "printWidth": 80,
        "proseWrap": "always"
      }
    },
    {
      "files": ["*.yml", "*.yaml"],
      "options": {
        "singleQuote": false,
        "tabWidth": 2
      }
    }
  ]
}
```

### 3. JavaScript Configuration (`jsconfig.json`)

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["dom", "dom.iterable", "ES6"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": false,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "baseUrl": ".",
    "paths": {
      "@/*": ["./*"],
      "@/components/*": ["./components/*"],
      "@/app/*": ["./app/*"],
      "@/lib/*": ["./lib/*"],
      "@/hooks/*": ["./hooks/*"],
      "@/store/*": ["./app/store/*"],
      "@/services/*": ["./app/services/*"]
    }
  },
  "include": [
    "next-env.d.ts",
    "**/*.js",
    "**/*.jsx",
    ".next/types/**/*.ts",
    "**/*.mjs"
  ],
  "exclude": [
    "node_modules",
    ".next",
    "out",
    "build",
    "dist"
  ]
}
```

---

## 💻 VS Code Integration

### 1. Workspace Settings (`.vscode/settings.json`)

```json
{
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "editor.formatOnType": false,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit",
    "source.organizeImports": "explicit"
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "eslint.format.enable": true,
  "eslint.lintTask.enable": true,
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[markdown]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.wordWrap": "on"
  },
  "files.associations": {
    "*.css": "tailwindcss"
  },
  "emmet.includeLanguages": {
    "javascript": "javascriptreact"
  },
  "tailwindCSS.includeLanguages": {
    "javascript": "javascript",
    "html": "HTML"
  },
  "tailwindCSS.experimental.classRegex": [
    "tw`([^`]*)",
    "tw=\"([^\"]*)",
    "tw={\"([^\"}]*)",
    "tw\\.\\w+`([^`]*)",
    "tw\\(.*?\\)`([^`]*)"
  ],
  "editor.quickSuggestions": {
    "strings": "on"
  },
  "css.validate": false,
  "less.validate": false,
  "scss.validate": false,
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "files.eol": "\n",
  "files.insertFinalNewline": true,
  "files.trimTrailingWhitespace": true,
  "editor.rulers": [120],
  "workbench.editor.highlightModifiedTabs": true,
  "problems.showCurrentInStatus": true
}
```

### 2. Recommended Extensions (`.vscode/extensions.json`)

```json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "bradlc.vscode-tailwindcss",
    "ms-vscode.vscode-json",
    "christian-kohler.path-intellisense",
    "formulahendry.auto-rename-tag",
    "aaron-bond.better-comments",
    "usernamehw.errorlens",
    "streetsidesoftware.code-spell-checker",
    "ms-vscode.vscode-typescript-next",
    "gruntfuggly.todo-tree",
    "oderwat.indent-rainbow",
    "pkief.material-icon-theme",
    "github.copilot"
  ],
  "unwantedRecommendations": [
    "ms-vscode.vscode-typescript",
    "hookyqr.beautify"
  ]
}
```

---

## 🎯 Usage & Commands

### Daily Development Commands

```bash
# Development server
npm run dev

# Code quality checks
npm run lint                    # Check for linting issues
npm run lint:fix               # Auto-fix linting issues
npm run format                 # Format all files with Prettier
npm run format:check          # Check formatting without fixing

# Build commands
npm run build                  # Production build
npm start                     # Start production server
```

### Git Hooks Setup (Optional)

Add pre-commit hooks to ensure code quality:

```bash
# Install husky for git hooks
npm install --save-dev husky lint-staged

# Add to package.json
```

```json
{
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{json,css,scss,md}": [
      "prettier --write"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  }
}
```

### IDE Shortcuts

- **Format Document**: `Ctrl+Shift+I` (Windows) / `Cmd+Shift+I` (Mac)
- **Fix ESLint Issues**: `Ctrl+Shift+P` → "ESLint: Fix all auto-fixable Problems"
- **Organize Imports**: `Ctrl+Shift+O` (Windows) / `Cmd+Shift+O` (Mac)

---

## 🛠️ Troubleshooting

### Common Issues and Solutions

#### 1. ESLint Configuration Not Found
```bash
# Error: ESLint configuration not found
# Solution: Ensure eslint.config.mjs exists and is properly formatted
npm run lint -- --debug
```

#### 2. Prettier and ESLint Conflicts
```bash
# If you see conflicting rules between Prettier and ESLint
# Solution: The configuration is designed to avoid conflicts, but if issues arise:
npm install --save-dev eslint-config-prettier
```

Then add to eslint.config.mjs:
```javascript
import prettier from 'eslint-config-prettier';

export default [
  // ... other config
  prettier, // Add this last to override conflicting rules
];
```

#### 3. Import Resolution Issues
```bash
# Error: Module not found or import/no-unresolved
# Solution: Update jsconfig.json paths or disable the rule for specific cases
```

#### 4. VS Code Not Auto-formatting
1. Check that Prettier extension is installed and enabled
2. Verify `.vscode/settings.json` is properly configured
3. Restart VS Code
4. Check Output panel for Prettier/ESLint errors

#### 5. Performance Issues with Large Projects
```javascript
// In eslint.config.mjs, add more specific ignores
{
  ignores: [
    // Add specific large directories
    'public/assets/**',
    'docs/**',
    '**/*.min.*'
  ]
}
```

### Debugging Commands

```bash
# Debug ESLint configuration
npx eslint --print-config app/page.js

# Check which files ESLint will process
npx eslint --debug .

# Verify Prettier configuration
npx prettier --check . --debug-check

# Check for configuration conflicts
npm ls eslint prettier
```

---

## 🎨 Customization Options

### Adjusting Rules Severity

In `eslint.config.mjs`, you can modify rule severity:

```javascript
rules: {
  // Change from error to warning
  'no-console': 'warn',          // Instead of 'error'

  // Disable rules entirely
  'react/jsx-props-no-spreading': 'off',

  // Make warnings into errors
  'no-unused-vars': 'error',     // Instead of 'warn'
}
```

### Project-Specific Ignores

Add project-specific files to ignore:

```javascript
{
  ignores: [
    // Add your specific files/directories
    'legacy/**',
    'vendor/**',
    'generated/**',
    '**/old-code/**',
  ]
}
```

### Team-Specific Rules

For different team preferences:

```javascript
// Stricter settings for senior teams
rules: {
  'max-len': ['error', { code: 100 }],  // Shorter line length
  'complexity': ['error', 10],          // Lower complexity limit
  'no-console': 'error',                // No console statements
}

// More lenient for junior teams
rules: {
  'max-len': ['warn', { code: 140 }],   // Longer line length
  'no-console': 'warn',                 // Allow console for debugging
  'no-unused-vars': 'warn',            // Warnings instead of errors
}
```

### Framework-Specific Additions

For specific frameworks or libraries:

```bash
# React Native
npm install --save-dev eslint-plugin-react-native

# Redux
npm install --save-dev eslint-plugin-redux-saga

# Testing libraries
npm install --save-dev eslint-plugin-jest eslint-plugin-testing-library
```

---

## 📚 Additional Resources

### Official Documentation
- [ESLint Configuration](https://eslint.org/docs/user-guide/configuring/)
- [Prettier Configuration](https://prettier.io/docs/en/configuration.html)
- [Next.js ESLint](https://nextjs.org/docs/basic-features/eslint)

### Recommended Reading
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [React Best Practices](https://reactjs.org/docs/thinking-in-react.html)
- [ESLint Flat Config Migration](https://eslint.org/docs/user-guide/migrating-to-9.0.0)

### Community Resources
- [Awesome ESLint](https://github.com/dustinspecker/awesome-eslint)
- [Prettier Community Plugins](https://prettier.io/docs/en/plugins.html)

---

## 📄 License and Credits

This configuration guide is based on industry best practices and combines:
- Airbnb JavaScript Style Guide principles
- Next.js official recommendations
- React community standards
- Modern ESLint and Prettier configurations

Feel free to adapt and modify this setup for your specific project needs!

---

## 🤝 Contributing to This Guide

If you find improvements or have suggestions:

1. Test the configuration thoroughly
2. Document any changes or additions
3. Include reasoning for modifications
4. Update version compatibility information
5. Add troubleshooting steps for new issues

---

**Last Updated**: September 2025
**Compatible With**: Next.js 15+, React 19+, ESLint 9+, Prettier 3+
