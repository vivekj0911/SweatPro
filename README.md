# SweatPro

A dynamic React-based workout generator that creates personalized gym routines based on your training style, target muscles, and fitness goals.

## Overview

SweatPro is an intelligent fitness application that generates customized workout plans tailored to your preferences. Whether you're following a bro split, bodybuilder split, upper/lower routine, or targeting individual muscle groups, SweatPro creates scientifically-structured workouts with proper exercise selection, rep ranges, tempo, and rest periods.

## Features

- **Multiple Training Splits**: Choose from individual muscle targeting, bro split, bodybuilder split, or upper/lower routines
- **Goal-Oriented Programming**: Select from strength/power, hypertrophy/growth, or cardiovascular endurance goals
- **Smart Exercise Selection**: Automatically generates workouts with appropriate compound and accessory exercises
- **Comprehensive Exercise Database**: Includes detailed instructions, variations, and substitutes for each exercise
- **Responsive Design**: Fully responsive interface built with Tailwind CSS
- **Dynamic Workout Display**: View your generated workout with exercise cards showing sets, reps, tempo, and rest periods

## Tech Stack

- **Frontend Framework**: React 18.3.1
- **Build Tool**: Vite 5.4.1
- **Styling**: Tailwind CSS 3.4.10
- **Icons**: Font Awesome 6.6.0
- **Fonts**: Google Fonts (Poppins)
- **Linting**: ESLint 9.9.0

## Getting Started

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn

### Installation

1. Clone the repository:
```bash
git clone https://github.com/vivekj0911/SweatPro.git
cd sweatpro
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

4. Open your browser and navigate to the local development URL (typically `http://localhost:5173`)

### Build for Production

```bash
npm run build
```

### Preview Production Build

```bash
npm run preview
```

## Usage

1. **Pick Your Poison**: Select your preferred training split
   - Individual: Target specific muscle groups
   - Bro Split: Push/Pull/Legs routine
   - Bodybuilder Split: Chest/Back/Shoulders/Legs/Arms/Abs
   - Upper/Lower: Upper body and lower body split

2. **Lock on Targets**: Choose which muscle groups to train (up to 3 for individual split)

3. **Become Juggernaut**: Select your training goal
   - Strength/Power: 3-8 reps, longer rest periods
   - Growth/Hypertrophy: 8-15 reps, moderate rest
   - Cardiovascular/Endurance: 12-30 reps, shorter rest

4. **Formulate**: Generate your custom workout and scroll down to view the exercises

## Project Structure

```
sweatpro/
├── public/
│   └── treadmill.svg          # App icon
├── src/
│   ├── assets/                # Static assets
│   ├── components/
│   │   ├── Button.jsx         # Reusable button component
│   │   ├── ExerciseCard.jsx   # Individual exercise display
│   │   ├── Generator.jsx      # Workout configuration interface
│   │   ├── Hero.jsx           # Landing section
│   │   ├── SectionWrapper.jsx # Layout wrapper component
│   │   └── Workout.jsx        # Generated workout display
│   ├── utils/
│   │   ├── function.js        # Workout generation logic
│   │   └── sweatpro.js        # Exercise database and constants
│   ├── App.jsx                # Main application component
│   ├── main.jsx               # Application entry point
│   └── index.css              # Global styles
├── index.html                 # HTML template
├── package.json               # Project dependencies
├── vite.config.js             # Vite configuration
├── tailwind.config.js         # Tailwind CSS configuration
├── postcss.config.js          # PostCSS configuration
└── eslint.config.js           # ESLint configuration
```

## How It Works

### Workout Generation Algorithm

The workout generator uses a sophisticated algorithm that:

1. Filters exercises based on selected muscle groups and training environment
2. Categorizes exercises into compound and accessory movements
3. Applies goal-specific rep ranges, set ratios, and rest periods
4. Randomly selects exercises while avoiding duplicates
5. Assigns appropriate tempo and rep/duration targets
6. Ensures balanced muscle group coverage

### Exercise Database

The application includes an extensive exercise library with:
- Exercise type (compound/accessory)
- Target muscles
- Equipment requirements
- Multiple variations
- Detailed form instructions
- Exercise substitutes
- Tempo recommendations

### Training Schemes

Each goal has specific programming parameters:

- **Strength/Power**: 3-8 reps, 3:2 compound-to-accessory ratio, 120s/60s rest
- **Hypertrophy**: 8-15 reps, 2:3 compound-to-accessory ratio, 90s/60s rest
- **Endurance**: 12-30 reps, 2:4 compound-to-accessory ratio, 60s/45s rest

## Customization

### Adding New Exercises

Edit `src/utils/sweatpro.js` and add exercises following this structure:

```javascript
exercise_name: {
    type: 'compound' | 'accessory',
    meta: {
        environment: 'gym' | 'gymhome' | 'home',
        level: [0, 1, 2],
        equipment: ['barbell', 'dumbbells', etc.]
    },
    variants: {
        variant_name: 'Description of variant'
    },
    unit: 'reps' | 'duration',
    muscles: ['chest', 'back', etc.],
    description: 'Exercise instructions',
    substitutes: ['alternative_exercise_1', 'alternative_exercise_2']
}
```

### Modifying Training Schemes

Edit the `SCHEMES` object in `src/utils/sweatpro.js` to adjust rep ranges, ratios, and rest periods.

## Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run lint` - Run ESLint

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is private and not licensed for public use.

## Acknowledgments

- Exercise database compiled from various fitness resources
- UI design inspired by modern fitness applications
- Built with modern React best practices
