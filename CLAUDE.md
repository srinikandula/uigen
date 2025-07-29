# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

UIGen is an AI-powered React component generator that allows users to describe components in natural language and generates them with live preview. The application uses a virtual file system where generated components are stored in memory and can be previewed in real-time without writing files to disk.

## Development Commands

- `npm run setup` - Initial setup: installs dependencies, generates Prisma client, and runs database migrations
- `npm run dev` - Start development server with Turbopack (http://localhost:3000)
- `npm run dev:daemon` - Start development server in background with logs to logs.txt
- `npm run build` - Build for production
- `npm run lint` - Run ESLint
- `npm run test` - Run tests with Vitest
- `npm run db:reset` - Reset database and run migrations
- `git push` - Push committed changes to remote repository

## Architecture

### Core Components

**Virtual File System (`src/lib/file-system.ts`)**
- `VirtualFileSystem` class manages an in-memory file tree
- Files are stored as `FileNode` objects with type, name, path, content, and children
- Supports serialization/deserialization for persistence
- Files are never written to actual disk - everything exists in memory

**Context Providers (`src/lib/contexts/`)**
- `FileSystemProvider` - manages the virtual file system state and tool calls
- `ChatProvider` - handles AI chat interactions using Vercel AI SDK

**Main UI Structure (`src/app/main-content.tsx`)**
- Split-panel layout: Chat interface on left, Preview/Code view on right
- Preview tab shows live React component rendering
- Code tab shows file tree + Monaco code editor
- Resizable panels for flexible workspace

### Key Features

**AI Integration (`src/app/api/chat/route.ts`)**
- Uses Anthropic Claude via Vercel AI SDK
- Custom tools: `str_replace_editor` and `file_manager` for code manipulation
- Supports both authenticated users with projects and anonymous users
- Falls back to mock responses when `ANTHROPIC_API_KEY` is not set

**Authentication & Projects**
- Optional authentication system using bcrypt for passwords
- Prisma with SQLite for data persistence
- Anonymous users can use the app without saving projects
- Authenticated users get project persistence and history

**Component Preview (`src/components/preview/PreviewFrame.tsx`)**
- Renders generated React components in an isolated iframe
- Uses Babel standalone for client-side JSX compilation
- Hot reloading when files change in virtual file system

## Database Schema

- `User` model: id, email, password, timestamps
- `Project` model: id, name, userId (optional), messages (JSON), data (JSON), timestamps
- Prisma client generated to `src/generated/prisma/`

## Testing

- Vitest with jsdom environment
- React Testing Library for component tests
- Tests located in `__tests__/` directories alongside components
- Test files use `.test.tsx` or `.test.ts` extensions

## Key Patterns

- All components use TypeScript with strict typing
- Tailwind CSS v4 for styling with custom design system
- React Server Components for initial page loads
- Client components for interactive features
- Error boundaries and proper loading states throughout
- Consistent file naming: PascalCase for components, kebab-case for utilities