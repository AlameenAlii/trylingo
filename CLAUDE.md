# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Duolingo clone built with Next.js 14, featuring a language learning platform with lessons, progress tracking, and gamification elements.

## Tech Stack

- **Framework**: Next.js 14 with App Router and Server Actions
- **Database**: PostgreSQL (NeonDB) with DrizzleORM
- **Authentication**: Clerk
- **UI Components**: Shadcn UI with Radix UI primitives
- **Styling**: Tailwind CSS with custom animations
- **State Management**: Server-side with database queries
- **Package Manager**: pnpm

## Development Commands

```bash
# Development
pnpm dev                 # Start Next.js development server
pnpm build              # Build production bundle
pnpm lint               # Run ESLint

# Database
pnpm db:gen             # Generate Drizzle migrations
pnpm db:migrate         # Apply migrations to database
pnpm db:push            # Push schema changes directly (dev only)
pnpm db:studio          # Open Drizzle Studio GUI
pnpm db:seed            # Seed database with initial data

# Environment
pnpm env:gen            # Generate .env.example from .env.local
```

## Architecture

### Routing Structure

The app uses Next.js 14 App Router with parallel routes:

- `/(landing)` - Public landing page
- `/(user)` - Authenticated user area with:
  - `/learn` - Main learning interface with units and lessons
  - `/courses` - Course selection
  - `/shop` - Hearts exchange system
  - `/leaderboard` - User rankings
  - `/quests` - Achievement system
- `/lesson` - Individual lesson interface

### Database Schema

Located in `/db/schema/`:
- `courses` - Language courses
- `units` - Course units containing lessons
- `lessons` - Individual lessons within units
- `challenges` - Questions/exercises within lessons
- `challengeProgress` - User's progress on challenges
- `userProgress` - Overall user progress tracking

Database queries are organized in `/db/queries/` with dedicated files for each entity.

### Component Organization

- `/components/landing/` - Landing page components
- `/components/user/` - Authenticated user components
- `/components/ui/` - Reusable UI primitives (shadcn/ui)
- `/components/motion/` - Animation components using Framer Motion
- `/components/theme/` - Theme provider and toggle

### Key Patterns

1. **Server Components by Default**: Most components are Server Components with data fetching at the component level
2. **Parallel Routes**: Dashboard uses parallel routes for `@main` and `@userProgress` slots
3. **Database Access**: Direct database queries in Server Components using DrizzleORM
4. **Environment Variables**: Managed with dotenvx, loaded via `pnpm env:load` prefix
5. **SVG Handling**: Custom webpack config to import SVGs as React components

## Code Style

- **Prettier**: Single quotes, no semicolons, 100-character line width
- **TypeScript**: Strict mode enabled
- **Imports**: Use `@/` alias for root-relative imports
- **Components**: Functional components with TypeScript