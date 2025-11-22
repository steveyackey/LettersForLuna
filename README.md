# Letters for Luna 🔤

A full-screen, mobile-focused Blazor WebAssembly app for toddlers learning letters.

## Features

- **Full-screen letter display** - Shows both uppercase and lowercase letters in large, easy-to-read text
- **Interactive learning** - Tap anywhere on the screen to hear the letter pronounced
- **Random letter switching** - After speaking, automatically switches to a new random letter
- **Mobile-optimized** - Designed for touch interaction with no navigation or distractions
- **Text-to-speech** - Uses the Web Speech API for natural letter pronunciation

## Running the App

### Development
```bash
dotnet run
```

Then open your browser to the URL shown (typically `https://localhost:5001` or `http://localhost:5000`)

### Build for Production
```bash
dotnet publish -c Release
```

The output will be in `bin/Release/net10.0/publish/wwwroot/` and can be deployed to any static web host.

## How It Works

1. The app displays a random letter in both uppercase and lowercase
2. When the user taps anywhere on the screen:
   - The letter is spoken using text-to-speech
   - After a brief delay, a new random letter is displayed
3. The cycle continues for endless learning fun!

## Technical Details

- Built with .NET 10 Blazor WebAssembly
- Uses Web Speech API (SpeechSynthesis) for audio
- Fully responsive with mobile-first design
- No external dependencies beyond .NET framework
