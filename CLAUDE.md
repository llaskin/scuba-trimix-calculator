# CLAUDE.md

This file provides guidance to Claude Code when working with this project.

## Overview

This is a single-page web application for calculating gas fill volumes for technical diving trimix blends. It helps divers calculate how much of each gas (helium, oxygen, and top-off gas) they need to achieve a target breathing mix.

## Project Structure

```
scuba/
├── index.html      # Single-page application (HTML + CSS + JS)
└── CLAUDE.md       # This file
```

## Technical Details

### Gas Calculation Logic

The calculator uses partial pressure blending to determine fill volumes:

1. **Helium** is added first (pure gas)
2. **Oxygen** is added second (pure gas)
3. **Top-off gas** (Air or Nitrox 32) is added last

Key formula for top-off gas volume:
```
topOffVolume = targetNitrogen / topOffN2Fraction
```

Where nitrogen only comes from the top-off gas (air = 79% N₂, Nitrox 32 = 68% N₂).

Pure oxygen needed:
```
pureO2 = targetO2 - (topOffVolume × topOffO2Fraction)
```

### Unit Conversions

The app supports both Imperial and Metric units:

| Imperial | Metric | Conversion Factor |
|----------|--------|-------------------|
| PSI | bar | × 0.0689476 |
| CF (cubic feet) | Liters | × 28.3168 |
| feet (MOD) | meters | × 0.3048 |

### Key Features

- **Tank configuration**: Single or double tanks
- **Custom tank volume**: User-specified (rated volume at service pressure)
- **Custom end pressure**: User-specified final fill pressure
- **Target mix**: Helium %, Oxygen % (Nitrogen auto-calculated)
- **Top-off gas options**: Air (21/79) or Nitrox 32 (32/68)
- **MOD warning**: Displays Maximum Operating Depth for mixes >21% O₂

### Validation

The calculator validates:
- Gas percentages sum to 100%
- No negative percentages
- Mix is achievable with selected top-off gas (catches impossible mixes where top-off provides more O₂ than target)

## Development

### Running Locally

Simply open `index.html` in a web browser:

```bash
open index.html
# or
python3 -m http.server 8000  # then visit localhost:8000
```

### Modifying the Calculator

All code is in `index.html`:
- **CSS**: In `<style>` tag in `<head>`
- **HTML**: In `<body>` tag
- **JavaScript**: In `<script>` tag at end of `<body>`

Key JavaScript functions:
- `calculate()` - Main calculation logic
- `displayResults()` - Renders results to DOM
- `toggleUnits()` - Switches Imperial/Metric
- `toggleDouble()` - Switches single/double tanks
- `updateNitrogen()` - Auto-calculates N₂ when He/O₂ change

### Adding New Top-Off Gas Options

To add a new top-off gas (e.g., Nitrox 36):

1. Add option to the select element:
```html
<option value="36">Nitrox 36 (36% O₂ / 64% N₂)</option>
```

2. Update display logic in `displayResults()` to handle the new option name.

## Safety Notice

This calculator is for planning purposes only. Always:
- Verify calculations independently
- Analyze your mix with a calibrated analyzer before diving
- Understand the risks of breathing custom gas mixes
- Get proper training in technical diving gas blending
