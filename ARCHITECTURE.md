# SweatPro Architecture Documentation

## System Overview

SweatPro is a single-page React application that generates personalized workout routines. The application follows a component-based architecture with clear separation of concerns between UI components, business logic, and data.

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────┐
│                      App.jsx (Root)                      │
│  - Manages global state (workout, poison, muscles, goal) │
│  - Orchestrates workout generation                       │
└────────────┬────────────────────────────────────────────┘
             │
    ┌────────┴────────┬──────────────────┬────────────────┐
    │                 │                  │                │
┌───▼────┐    ┌──────▼──────┐    ┌─────▼──────┐   ┌────▼────┐
│  Hero  │    │  Generator  │    │  Workout   │   │ Utils   │
│        │    │             │    │            │   │         │
└────────┘    └──────┬──────┘    └─────┬──────┘   └────┬────┘
                     │                  │               │
              ┌──────┴──────┐    ┌──────▼──────┐  ┌────▼────┐
              │   Button    │    │ExerciseCard │  │function │
              └─────────────┘    └─────────────┘  │sweatpro │
                                                   └─────────┘
```

## Component Hierarchy

### App.jsx (Root Component)
**Responsibilities:**
- State management for workout configuration
- Workout generation orchestration
- Routing between sections via hash navigation

**State:**
```javascript
{
  workout: Array<Exercise> | null,  // Generated workout
  poison: string,                    // Training split type
  muscles: Array<string>,            // Selected muscle groups
  goal: string                       // Training goal
}
```

**Key Functions:**
- `updateWorkout()`: Validates inputs and generates workout using utility functions

### Hero.jsx
**Purpose:** Landing section with call-to-action

**Features:**
- Displays app branding
- Provides navigation to workout generator
- Sets the tone with motivational copy

### Generator.jsx
**Purpose:** Workout configuration interface

**Sub-components:**
- `Header`: Reusable section header with index, title, and description

**Features:**
- Three-step workout configuration process
- Modal for muscle group selection
- Dynamic UI based on selected training split
- Input validation (max 3 muscles for individual split)

**Key Functions:**
- `toggleModal()`: Controls muscle selection dropdown
- `updateMuscles(muscleGroup)`: Manages muscle group selection logic

### Workout.jsx
**Purpose:** Display generated workout

**Features:**
- Maps through workout array
- Renders ExerciseCard for each exercise
- Provides section wrapper with themed header

### ExerciseCard.jsx
**Purpose:** Individual exercise display component

**Expected Props:**
```javascript
{
  index: number,
  exercise: {
    name: string,
    type: string,
    muscles: Array<string>,
    description: string,
    reps: number,
    tempo: string,
    rest: number,
    unit: 'reps' | 'duration'
  }
}
```

### SectionWrapper.jsx
**Purpose:** Consistent layout wrapper for major sections

**Props:**
```javascript
{
  id: string,           // For hash navigation
  header: string,       // Section header text
  title: Array<string>, // Title parts for styling
  children: ReactNode   // Section content
}
```

### Button.jsx
**Purpose:** Reusable styled button component

**Props:**
```javascript
{
  text: string,
  func: Function
}
```

## Data Layer

### sweatpro.js
**Purpose:** Exercise database and training constants

**Exports:**

1. **TEMPOS**: Array of tempo patterns (e.g., '3 0 2')
   - Format: [eccentric, pause, concentric]

2. **SCHEMES**: Training goal configurations
   ```javascript
   {
     strength_power: {
       repRanges: [3, 8],
       ratio: [3, 2],      // compound:accessory
       rest: [120, 60]     // compound, accessory (seconds)
     },
     // ... other schemes
   }
   ```

3. **WORKOUTS**: Training split definitions
   ```javascript
   {
     individual: ['biceps', 'triceps', ...],
     bro_split: {
       push: ['triceps', 'chest', 'shoulders'],
       // ... other splits
     },
     // ... other workout types
   }
   ```

4. **EXERCISES**: Comprehensive exercise database
   - 100+ exercises with variations
   - Metadata: type, environment, level, equipment
   - Instructions and substitutes

### function.js
**Purpose:** Workout generation business logic

**Key Functions:**

1. **generateWorkout(args)**
   ```javascript
   Input: { muscles, poison, goal }
   Output: Array<Exercise>
   ```
   
   **Algorithm:**
   - Filters exercises by environment (excludes home-only exercises)
   - Determines muscle list based on training split
   - Applies goal-specific set scheme
   - Categorizes exercises into compound/accessory
   - Randomly selects exercises matching criteria
   - Assigns reps/duration, tempo, and rest periods
   - Prevents duplicate exercises

2. **shuffleArray(array)**
   - Fisher-Yates shuffle algorithm
   - Randomizes muscle group order

3. **exercisesFlattener(exercisesObj)**
   - Flattens exercise variants into individual entries
   - Creates unique keys for each variant
   - Maintains cross-references between variants

## Data Flow

### Workout Generation Flow

```
User Input (Generator)
    ↓
State Update (App)
    ↓
updateWorkout() triggered
    ↓
Validation Check
    ↓
generateWorkout() called
    ↓
┌─────────────────────────────────────┐
│ 1. Filter exercises by environment  │
│ 2. Determine target muscles         │
│ 3. Apply training scheme            │
│ 4. Categorize exercises             │
│ 5. Select random exercises          │
│ 6. Assign parameters                │
└─────────────────────────────────────┘
    ↓
Workout State Updated
    ↓
Workout Component Renders
    ↓
ExerciseCards Display
```

### State Management Pattern

SweatPro uses React's built-in state management with props drilling:

```
App (State Owner)
  ↓ (props)
Generator (State Consumer & Updater)
  ↓ (callbacks)
App (State Update)
  ↓ (props)
Workout (State Consumer)
```

## Styling Architecture

### Tailwind CSS Approach

- Utility-first CSS framework
- Responsive design with breakpoints (sm, md, lg)
- Custom color scheme (slate + blue accent)
- Gradient backgrounds
- Consistent spacing scale

### Theme Colors
- Background: `slate-800` to `slate-950` gradient
- Primary: `blue-400` / `blue-600`
- Text: `white` with `slate-400` accents

### Responsive Breakpoints
- `sm`: 640px
- `md`: 768px
- `lg`: 1024px

## Performance Considerations

### Optimization Strategies

1. **Component Memoization**: Potential for React.memo on static components
2. **Lazy Loading**: Could implement code splitting for exercise database
3. **State Optimization**: Minimal re-renders due to focused state updates
4. **Build Optimization**: Vite provides automatic code splitting and tree shaking

### Current Performance Characteristics

- **Initial Load**: Fast (small bundle size)
- **Workout Generation**: O(n) complexity, instant for typical use cases
- **Re-renders**: Minimal, only affected components update

## Security Considerations

1. **No Backend**: Client-side only, no API vulnerabilities
2. **No User Data**: No personal information collected or stored
3. **No Authentication**: Public access, no security concerns
4. **XSS Protection**: React's built-in XSS protection via JSX

## Scalability Considerations

### Current Limitations
- Exercise database loaded entirely in memory
- No pagination for large workout lists
- No workout history or persistence

### Future Scalability Options
1. Move exercise database to backend/API
2. Implement local storage for workout history
3. Add user accounts and cloud sync
4. Implement workout tracking and progress analytics

## Testing Strategy (Recommended)

### Unit Tests
- Workout generation algorithm
- Exercise filtering logic
- State update functions

### Integration Tests
- Component interaction flows
- Workout generation end-to-end
- Form validation

### E2E Tests
- Complete user journey
- Cross-browser compatibility
- Responsive design validation

## Deployment Architecture

### Build Process
```
Source Code (src/)
    ↓
Vite Build
    ↓
Optimized Bundle (dist/)
    ↓
Static File Hosting
```

### Recommended Hosting
- Vercel
- Netlify
- GitHub Pages
- AWS S3 + CloudFront

### Environment Variables
Currently none required. Future considerations:
- API endpoints
- Analytics keys
- Feature flags

## Future Architecture Enhancements

1. **State Management**: Consider Redux/Zustand for complex state
2. **Routing**: Implement React Router for multi-page experience
3. **Backend Integration**: Add API for workout persistence
4. **PWA Features**: Offline support, installability
5. **Animation Library**: Framer Motion for enhanced UX
6. **Testing Framework**: Jest + React Testing Library
7. **TypeScript**: Type safety for better DX
