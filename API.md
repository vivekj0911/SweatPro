# SweatPro API Documentation

## Overview

SweatPro is currently a client-side only application with no backend API. This document describes the internal JavaScript API and data structures used within the application.

## Core Functions

### generateWorkout()

Generates a personalized workout based on user preferences.

**Location:** `src/utils/function.js`

**Signature:**
```javascript
generateWorkout(args: Object): Array<Exercise>
```

**Parameters:**
```javascript
{
  muscles: Array<string>,    // Selected muscle groups
  poison: string,            // Training split type
  goal: string              // Training goal
}
```

**Returns:**
```javascript
Array<{
  name: string,              // Exercise name
  type: string,              // 'compound' or 'accessory'
  tempo: string,             // e.g., '3 0 2'
  rest: number,              // Rest period in seconds
  reps: number,              // Reps or duration
  unit: string,              // 'reps' or 'duration'
  muscles: Array<string>,    // Target muscles
  description: string,       // Exercise instructions
  substitutes: Array<string>, // Alternative exercises
  meta: Object              // Exercise metadata
}>
```

**Example Usage:**
```javascript
const workout = generateWorkout({
  muscles: ['chest', 'triceps'],
  poison: 'individual',
  goal: 'strength_power'
});
```

**Algorithm Steps:**
1. Filter exercises by environment (excludes home-only)
2. Determine muscle list based on training split
3. Shuffle muscle groups for variety
4. Apply goal-specific set scheme
5. Categorize exercises into compound/accessory
6. Randomly select exercises matching criteria
7. Assign reps/duration based on goal and exercise type
8. Assign tempo from predefined list
9. Calculate rest periods based on exercise type
10. Return filtered workout array

**Edge Cases:**
- Returns empty array if no muscles selected
- Adjusts rep count if tempo × reps > 85 seconds
- Falls back to opposite exercise type if no matches found
- Prevents duplicate exercises in same workout

---

### shuffleArray()

Randomizes array order using Fisher-Yates algorithm.

**Location:** `src/utils/function.js`

**Signature:**
```javascript
shuffleArray(array: Array<any>): Array<any>
```

**Parameters:**
- `array`: Array to shuffle (mutates original)

**Returns:**
- Shuffled array (same reference)

**Example:**
```javascript
const muscles = ['chest', 'back', 'shoulders'];
shuffleArray(muscles);
// muscles is now randomized
```

---

### exercisesFlattener()

Flattens exercise variants into individual exercise entries.

**Location:** `src/utils/function.js`

**Signature:**
```javascript
exercisesFlattener(exercisesObj: Object): Object
```

**Parameters:**
- `exercisesObj`: Nested exercise object with variants

**Returns:**
- Flattened object with variant exercises as separate entries

**Example:**
```javascript
// Input
{
  bench_press: {
    variants: {
      incline: 'Description',
      flat: 'Description'
    }
  }
}

// Output
{
  incline_bench_press: { /* exercise data */ },
  flat_bench_press: { /* exercise data */ }
}
```

---

## Data Structures

### EXERCISES

Complete exercise database with metadata and instructions.

**Location:** `src/utils/sweatpro.js`

**Structure:**
```javascript
{
  exercise_name: {
    type: 'compound' | 'accessory',
    meta: {
      environment: 'gym' | 'gymhome' | 'home',
      level: Array<number>,        // [0, 1, 2] = beginner to advanced
      equipment: Array<string>     // Required equipment
    },
    variants?: {
      variant_name: string         // Variant description
    },
    unit: 'reps' | 'duration',
    muscles: Array<string>,        // Primary muscles worked
    description: string,           // Exercise instructions
    substitutes: Array<string>     // Alternative exercises
  }
}
```

**Available Muscle Groups:**
- `chest`
- `back`
- `shoulders`
- `biceps`
- `triceps`
- `quads`
- `hamstrings`
- `glutes`
- `calves`
- `abs`

**Exercise Types:**
- `compound`: Multi-joint movements (e.g., bench press, squat)
- `accessory`: Isolation movements (e.g., bicep curl, leg extension)

**Environment Types:**
- `gym`: Requires gym equipment
- `gymhome`: Can be done at gym or home with equipment
- `home`: Bodyweight or minimal equipment

---

### SCHEMES

Training goal configurations with rep ranges and rest periods.

**Location:** `src/utils/sweatpro.js`

**Structure:**
```javascript
{
  scheme_name: {
    repRanges: [min, max],         // Rep range for goal
    ratio: [compound, accessory],  // Exercise ratio
    rest: [compound, accessory]    // Rest in seconds
  }
}
```

**Available Schemes:**

1. **strength_power**
   ```javascript
   {
     repRanges: [3, 8],
     ratio: [3, 2],      // 3 compound : 2 accessory
     rest: [120, 60]     // 2min compound, 1min accessory
   }
   ```

2. **growth_hypertrophy**
   ```javascript
   {
     repRanges: [8, 15],
     ratio: [2, 3],      // 2 compound : 3 accessory
     rest: [90, 60]      // 1.5min compound, 1min accessory
   }
   ```

3. **cardiovascular_endurance**
   ```javascript
   {
     repRanges: [12, 30],
     ratio: [2, 4],      // 2 compound : 4 accessory
     rest: [60, 45]      // 1min compound, 45s accessory
   }
   ```

---

### WORKOUTS

Training split definitions and muscle group mappings.

**Location:** `src/utils/sweatpro.js`

**Structure:**
```javascript
{
  split_name: Array<string> | Object
}
```

**Available Splits:**

1. **individual**
   ```javascript
   ['biceps', 'triceps', 'chest', 'back', 'shoulders', 
    'quads', 'hamstrings', 'glutes', 'calves', 'abs']
   ```

2. **bro_split**
   ```javascript
   {
     push: ['triceps', 'chest', 'shoulders'],
     pull: ['back', 'shoulders', 'biceps'],
     legs: ['glutes', 'calves', 'hamstrings', 'quads']
   }
   ```

3. **bodybuilder_split**
   ```javascript
   {
     chest: ['chest'],
     back: ['back'],
     shoulders: ['shoulders'],
     legs: ['glutes', 'quads', 'hamstrings', 'calves'],
     arms: ['biceps', 'triceps'],
     abs: ['abs']
   }
   ```

4. **upper_lower**
   ```javascript
   {
     upper: ['triceps', 'biceps', 'shoulders', 'chest', 'back'],
     lower: ['quads', 'calves', 'hamstrings', 'glutes']
   }
   ```

---

### TEMPOS

Exercise tempo patterns for time under tension.

**Location:** `src/utils/sweatpro.js`

**Format:** `'eccentric pause concentric'`

**Available Tempos:**
```javascript
[
  '3 0 2',  // 3s down, 0s pause, 2s up
  '2 2 2',  // 2s down, 2s pause, 2s up
  '4 1 1',  // 4s down, 1s pause, 1s up
  '5 3 1',  // 5s down, 3s pause, 1s up (very slow)
  '1 0 1',  // 1s down, 0s pause, 1s up (explosive)
  '3 2 1',  // 3s down, 2s pause, 1s up
  '2 1 1'   // 2s down, 1s pause, 1s up
]
```

**Tempo Phases:**
1. **Eccentric**: Lowering/lengthening phase
2. **Pause**: Hold at bottom position
3. **Concentric**: Lifting/shortening phase

---

## Component Props API

### App Component

**State:**
```javascript
{
  workout: Array<Exercise> | null,
  poison: string,
  muscles: Array<string>,
  goal: string
}
```

**Methods:**
```javascript
updateWorkout(): void
```

---

### Generator Component

**Props:**
```javascript
{
  poison: string,
  setPoison: (poison: string) => void,
  muscles: Array<string>,
  setMuscles: (muscles: Array<string>) => void,
  goal: string,
  setGoal: (goal: string) => void,
  updateWorkout: () => void
}
```

**Internal State:**
```javascript
{
  showModal: boolean
}
```

**Methods:**
```javascript
toggleModal(): void
updateMuscles(muscleGroup: string): void
```

---

### Workout Component

**Props:**
```javascript
{
  workout: Array<Exercise>
}
```

---

### ExerciseCard Component

**Props:**
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
    unit: 'reps' | 'duration',
    substitutes: Array<string>
  }
}
```

---

### SectionWrapper Component

**Props:**
```javascript
{
  id: string,
  header: string,
  title: Array<string>,
  children: ReactNode
}
```

---

### Button Component

**Props:**
```javascript
{
  text: string,
  func: () => void
}
```

---

## Constants

### Maximum Selections
```javascript
MAX_INDIVIDUAL_MUSCLES = 3
```

### Time Under Tension Limit
```javascript
MAX_TUT_SECONDS = 85  // Maximum time under tension per set
```

### Navigation Anchors
```javascript
SECTIONS = {
  HERO: '#',
  GENERATOR: '#generate',
  WORKOUT: '#workout'
}
```

---

## Error Handling

### Validation Rules

1. **Muscle Selection**
   - Must select at least 1 muscle group
   - Maximum 3 muscles for individual split
   - Exactly 1 selection for other splits

2. **Workout Generation**
   - Returns early if no muscles selected
   - Filters out empty exercise objects
   - Falls back to opposite exercise type if needed

3. **Rep Calculation**
   - Adjusts reps if tempo × reps > 85 seconds
   - Rounds duration to nearest 5 seconds
   - Adds 4 reps to accessory exercises

---

## Usage Examples

### Generate Strength Workout
```javascript
const workout = generateWorkout({
  muscles: ['chest', 'triceps'],
  poison: 'individual',
  goal: 'strength_power'
});
```

### Generate Hypertrophy Push Day
```javascript
const workout = generateWorkout({
  muscles: ['push'],
  poison: 'bro_split',
  goal: 'growth_hypertrophy'
});
```

### Generate Endurance Upper Body
```javascript
const workout = generateWorkout({
  muscles: ['upper'],
  poison: 'upper_lower',
  goal: 'cardiovascular_endurance'
});
```

---

## Future API Considerations

If a backend is implemented, consider these endpoints:

### Potential REST API

```
POST   /api/workouts/generate
GET    /api/workouts/:id
POST   /api/workouts/:id/complete
GET    /api/exercises
GET    /api/exercises/:name
POST   /api/users/register
POST   /api/users/login
GET    /api/users/profile
GET    /api/users/history
```

### Potential GraphQL Schema

```graphql
type Query {
  exercises(filter: ExerciseFilter): [Exercise]
  workout(id: ID!): Workout
  userWorkouts: [Workout]
}

type Mutation {
  generateWorkout(input: WorkoutInput!): Workout
  completeWorkout(id: ID!): Workout
  saveWorkout(input: WorkoutInput!): Workout
}
```
