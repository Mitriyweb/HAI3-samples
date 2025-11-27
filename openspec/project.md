# Project Context

## Purpose
HAI3 Samples is a demonstration application for the HAI3 UI framework. It showcases:
- Screenset-based architecture with vertical slices
- Theme system with multiple built-in themes (default, light, dark, dracula)
- UI component library integration
- Internationalization (i18n) support for 36 languages

## Tech Stack
- **TypeScript** 5.4 - Strict mode enabled
- **React** 18.3 - UI framework
- **Vite** 6.4 - Build tool and dev server
- **Redux Toolkit** 2.2 - State management (Flux architecture)
- **Tailwind CSS** 3.4 - Utility-first styling
- **PostCSS** + **Autoprefixer** - CSS processing
- **ESLint** 9 - Linting with custom local plugins
- **Lucide React** - Icon library

### HAI3 Framework Packages
- `@hai3/uicore` - Core framework (routing, state, registries)
- `@hai3/uikit` - UI component library
- `@hai3/uikit-contracts` - Type definitions and contracts
- `@hai3/studio` - Development tools

## Project Conventions

### Code Style
- **No `any` types** - ESLint enforces `@typescript-eslint/no-explicit-any`
- **Unused variables** - Must be prefixed with `_` (e.g., `_unused`)
- **Lodash for string operations** - Use lodash instead of native methods (`trim`, `toUpperCase`, etc.)
- **Path aliases** - Use `@/` for src imports (e.g., `@/screensets/*`, `@/themes/*`)
- **File naming**:
  - Screensets: `*Screenset.tsx` (e.g., `demoScreenset.tsx`)
  - Screens: `*Screen.tsx` (e.g., `HelloWorldScreen.tsx`)
  - IDs: Defined in `ids.ts` files per screenset/domain

### Architecture Patterns

#### Screenset Architecture (Vertical Slices)
- Each screenset is self-contained with its own screens, icons, translations, and API mocks
- Screensets auto-discovered via Vite glob pattern: `./*/*[Ss]creenset.tsx`
- Self-registration pattern: screensets call `screensetRegistry.register()` on import

#### Flux Architecture (Strictly Enforced by ESLint)
- **Actions** → Emit events via `eventBus.emit()`, must be pure functions (no async, no state access)
- **Effects** → Listen to events, update Redux slices (cannot emit events or import actions)
- **Components** → Use `useSelector` for state, dispatch actions only (no direct slice dispatch)
- **No circular flows** - Unidirectional data flow is enforced at lint time

#### Theme System
- Themes registered via `themeRegistry.register()`
- Applied via `applyTheme()` from uikit
- Available themes: default, light, dark, dracula, dracula-large

### Testing Strategy
- **Type checking**: `npm run type-check` (tsc --noEmit)
- **Linting**: `npm run lint` (zero warnings policy)
- **Architecture checks**: `npm run arch:check` and `npm run arch:deps` (dependency-cruiser)

### Git Workflow
- **Main branch**: `main`
- **Feature branches**: `feature/<name>` (e.g., `feature/repo_init`)
- **Commit style**: Conventional commits (`feat:`, `fix:`, `chore:`, etc.)

## Domain Context

### Key Concepts
- **Screenset**: A collection of related screens with shared context (like a mini-app)
- **Screen**: An individual view/page within a screenset
- **UIKit**: Reusable UI component library (buttons, forms, modals, etc.)
- **Theme**: Visual styling configuration (colors, typography, spacing)
- **i18n**: Translation system supporting 36 languages with lazy-loaded JSON files

### Registries
- `screensetRegistry` - Manages screenset discovery and registration
- `themeRegistry` - Manages theme registration and application
- `uikitRegistry` - Manages custom icon registration
- `apiRegistry` - Manages API mocks and service registration
- `I18nRegistry` - Manages translation loaders

## Important Constraints
- **No barrel exports** for events/effects (enforced by `local/no-barrel-exports-events-effects`)
- **No coordinator effects** (enforced by `local/no-coordinator-effects`)
- **Domain IDs required** (enforced by `local/no-missing-domain-id`)
- **Domain event format** (enforced by `local/domain-event-format`)
- **Strict TypeScript** - All strict options enabled
- **No inline ESLint config** - `noInlineConfig: true`

## External Dependencies
- **HAI3 Framework** (`@hai3/*` packages) - Core framework providing:
  - Redux store setup and provider
  - AppRouter with dynamic routing
  - URL ↔ Redux sync
  - Menu and footer components
  - API registry for mock/real service switching
