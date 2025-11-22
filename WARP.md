# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

Letters for Luna is a full-screen, mobile-focused Blazor WebAssembly app designed for toddlers learning letters. It's built with .NET 10 and uses the Web Speech API for text-to-speech functionality.

## Common Commands

### Development
```bash
# Run the app locally
dotnet run

# Build the project
dotnet build

# Clean build artifacts
dotnet clean

# Restore NuGet packages
dotnet restore
```

### Production
```bash
# Build for production deployment
dotnet publish -c Release

# Output location: bin/Release/net10.0/publish/wwwroot/
```

The published output can be deployed to any static web host.

### Accessing the App
- Local development: `https://localhost:5001` or `http://localhost:5000`
- The app is designed for mobile browsers with touch interaction

## Architecture

### Technology Stack
- **Framework**: .NET 10 Blazor WebAssembly
- **Target Framework**: net10.0
- **Package Dependencies**: 
  - Microsoft.AspNetCore.Components.WebAssembly 10.0.0
  - Microsoft.AspNetCore.Components.WebAssembly.DevServer 10.0.0
- **External Resources**: Andika font family from Google Fonts (child-friendly, readable)

### Application Structure

**Core Entry Point**: `Program.cs`
- Configures WebAssembly host with App component and HeadOutlet
- Registers HttpClient with base address

**Routing**: `App.razor`
- Uses Blazor Router with MainLayout as default layout
- Handles 404 with NotFound page

**Main Page**: `Pages/Home.razor` (route: `/`)
- Single interactive page displaying random letters
- On tap: speaks letter using JS interop → waits 1.5s → switches to new random letter
- Prevents same letter from repeating consecutively
- Calls `speakText` JS function via IJSRuntime

**Layout**: `Layout/MainLayout.razor`
- Minimal layout that just renders @Body
- No navigation, sidebars, or headers (intentionally simple for toddlers)

**JavaScript Integration**: `wwwroot/index.html`
- `window.speakText(text)` function uses Web Speech API (SpeechSynthesis)
- Configured with: rate 0.8 (slower), pitch 1.2 (child-friendly), volume 1.0
- Speaks lowercase to avoid "capital" being announced

**Styling**: `wwwroot/css/app.css`
- Full viewport letter display with gradient background
- Uppercase: 25vw font size, lowercase: 20vw (responsive on desktop)
- Prevents text selection and touch callouts for cleaner UX
- Uses touch-action: manipulation for better mobile interaction

### Key Design Patterns

**Interaction Flow**:
1. App initializes with random letter
2. User taps anywhere on screen → letter is spoken
3. 1.5 second delay for listening
4. New random letter appears (different from current)
5. Cycle repeats

**Mobile-First Design**:
- Full-screen viewport usage
- No scrolling or navigation elements
- Touch-optimized with `touch-action: manipulation`
- Meta tags for web app capabilities on iOS/Android

**State Management**:
- Component-local state in Home.razor (currentLetter)
- Random instance for letter selection
- No external state management needed (simple app)

## Development Notes

### Working with Blazor Components
- Razor files combine HTML markup with C# code in `@code` blocks
- Component lifecycle: OnInitialized() runs when component mounts
- Use `@inject` directive for dependency injection (e.g., IJSRuntime)

### JavaScript Interop
- Call JS from C#: `await JS.InvokeVoidAsync("functionName", args)`
- JS functions must be defined on `window` object
- Located in wwwroot/index.html for this project

### Adding New Pages
1. Create .razor file in Pages/ directory
2. Add `@page "/route"` directive at top
3. Import in _Imports.razor if needed
4. Router automatically discovers new routes

### Modifying Speech Behavior
- Speech settings in wwwroot/index.html `speakText` function
- Adjust rate, pitch, volume parameters
- SpeechSynthesis API: https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis

### CSS Changes
- App-wide styles: wwwroot/css/app.css
- Component-scoped CSS: Create .razor.css file alongside .razor file
- Scoped styles are automatically isolated to that component
