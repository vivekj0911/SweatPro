# Contributing to SweatPro

Thank you for your interest in contributing to SweatPro! This document provides guidelines and instructions for contributing to the project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Coding Standards](#coding-standards)
- [Adding Exercises](#adding-exercises)
- [Submitting Changes](#submitting-changes)
- [Reporting Issues](#reporting-issues)

## Code of Conduct

- Be respectful and inclusive
- Provide constructive feedback
- Focus on what is best for the community
- Show empathy towards other contributors

## Getting Started

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn
- Git
- Code editor (VS Code recommended)

### Setup Development Environment

1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/vivekj0911/sweatpro.git
   cd sweatpro
   ```

3. Install dependencies:
   ```bash
   npm install
   ```

4. Create a new branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```

5. Start the development server:
   ```bash
   npm run dev
   ```

## Development Workflow

### Branch Naming Convention

- `feature/` - New features (e.g., `feature/add-exercise-timer`)
- `fix/` - Bug fixes (e.g., `fix/workout-generation-bug`)
- `docs/` - Documentation updates (e.g., `docs/update-readme`)
- `refactor/` - Code refactoring (e.g., `refactor/component-structure`)
- `style/` - UI/styling changes (e.g., `style/improve-mobile-layout`)

### Commit Message Format

Follow the conventional commits specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, missing semicolons, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples:**
```
feat(exercises): add yoga exercises to database

fix(generator): prevent duplicate exercises in workout

docs(readme): update installation instructions

style(workout): improve mobile responsiveness
```

## Coding Standards

### JavaScript/React Guidelines

1. **Use Functional Components**
   ```javascript
   // Good
   function MyComponent() {
     return <div>Hello</div>
   }

   // Avoid
   class MyComponent extends React.Component {
     render() {
       return <div>Hello</div>
     }
   }
   ```

2. **Use Hooks for State Management**
   ```javascript
   const [state, setState] = useState(initialValue)
   ```

3. **Destructure Props**
   ```javascript
   // Good
   function Component({ title, description }) {
     return <div>{title}</div>
   }

   // Avoid
   function Component(props) {
     return <div>{props.title}</div>
   }
   ```

4. **Use Arrow Functions for Callbacks**
   ```javascript
   <button onClick={() => handleClick()}>Click</button>
   ```

5. **Keep Components Small and Focused**
   - Single responsibility principle
   - Extract reusable logic into custom hooks
   - Break down large components

### Styling Guidelines

1. **Use Tailwind Utility Classes**
   ```javascript
   <div className="flex items-center justify-center gap-4">
   ```

2. **Follow Responsive Design Pattern**
   ```javascript
   <div className="text-sm sm:text-base md:text-lg">
   ```

3. **Maintain Consistent Spacing**
   - Use Tailwind's spacing scale (gap-4, p-4, m-4, etc.)

4. **Color Scheme**
   - Background: `slate-800`, `slate-950`
   - Primary: `blue-400`, `blue-600`
   - Text: `white`, `slate-400`

### File Organization

```
src/
├── components/       # React components
│   ├── Button.jsx
│   └── ...
├── utils/           # Utility functions and data
│   ├── function.js
│   └── sweatpro.js
├── assets/          # Static assets
├── App.jsx          # Root component
├── main.jsx         # Entry point
└── index.css        # Global styles
```

### Code Quality

1. **Run Linter Before Committing**
   ```bash
   npm run lint
   ```

2. **Fix Linting Errors**
   ```bash
   npm run lint -- --fix
   ```

3. **Test Your Changes**
   - Test on multiple screen sizes
   - Test all workout generation scenarios
   - Verify no console errors

## Adding Exercises

### Exercise Structure

When adding new exercises to `src/utils/sweatpro.js`, follow this structure:

```javascript
exercise_name: {
    type: 'compound' | 'accessory',
    meta: {
        environment: 'gym' | 'gymhome' | 'home',
        level: [0, 1, 2],  // 0=beginner, 1=intermediate, 2=advanced
        equipment: ['barbell', 'dumbbells', 'bands', etc.]
    },
    variants: {
        variant_name: 'Detailed description of this variant'
    },
    unit: 'reps' | 'duration',
    muscles: ['primary_muscle', 'secondary_muscle'],
    description: 'Clear, concise form instructions',
    substitutes: ['alternative_exercise_1', 'alternative_exercise_2']
}
```

### Exercise Naming Convention

- Use lowercase with underscores: `barbell_bench_press`
- Be specific: `incline_dumbbell_press` not just `press`
- Include equipment: `dumbbell_curl` not just `curl`

### Exercise Guidelines

1. **Type Classification**
   - `compound`: Multi-joint movements (bench press, squat, deadlift)
   - `accessory`: Isolation movements (bicep curl, leg extension)

2. **Environment**
   - `gym`: Requires gym equipment
   - `gymhome`: Can be done at gym or home with equipment
   - `home`: Bodyweight or minimal equipment

3. **Muscle Groups**
   Available options: `chest`, `back`, `shoulders`, `biceps`, `triceps`, `quads`, `hamstrings`, `glutes`, `calves`, `abs`

4. **Description Quality**
   - Clear form cues
   - Safety considerations
   - Range of motion details
   - Common mistakes to avoid

5. **Variants**
   - Different angles (incline, flat, decline)
   - Different grips (overhand, underhand, neutral)
   - Different stances (wide, narrow, standard)

### Example Exercise Addition

```javascript
dumbbell_shoulder_press: {
    type: 'compound',
    meta: {
        environment: 'gymhome',
        level: [0, 1, 2],
        equipment: ['dumbbells']
    },
    variants: {
        seated: 'Perform this exercise seated on a bench with back support.',
        standing: 'Perform this exercise standing with feet shoulder-width apart.',
        arnold: 'Start with palms facing you, rotate to palms forward as you press up.'
    },
    unit: 'reps',
    muscles: ['shoulders', 'triceps'],
    description: 'Hold dumbbells at shoulder height, press overhead until arms are fully extended, then lower back to starting position. Keep core engaged and avoid arching your back.',
    substitutes: ['barbell_overhead_press', 'machine_shoulder_press']
}
```

### Pull Request Process

1. **Update Your Branch**
   ```bash
   git checkout main
   git pull origin main
   git checkout your-branch
   git rebase main
   ```

2. **Push Your Changes**
   ```bash
   git push origin your-branch
   ```

3. **Create Pull Request**
   - Go to GitHub repository
   - Click "New Pull Request"
   - Select your branch
   - Fill out the PR template

### Pull Request Template

```markdown

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Code refactoring
- [ ] Style/UI improvement


## Testing
- [ ] Tested on desktop
- [ ] Tested on mobile
- [ ] Tested all workout generation scenarios
- [ ] No console errors

### Review Process

1. Maintainer will review your PR
2. Address any requested changes
3. Once approved, PR will be merged
4. Your contribution will be credited


### Bug Reports

When reporting bugs, include:

1. **Description**: Clear description of the bug
2. **Steps to Reproduce**:
   - Step 1
   - Step 2
   - Step 3
3. **Expected Behavior**: What should happen
4. **Actual Behavior**: What actually happens
5. **Screenshots**: If applicable
6. **Environment**:
   - Browser and version
   - Device type
   - Screen size

### Feature Requests

When requesting features, include:

1. **Problem Statement**: What problem does this solve?
2. **Proposed Solution**: How should it work?
3. **Alternatives Considered**: Other approaches you've thought about
4. **Additional Context**: Any other relevant information

### Issue Labels

- `bug`: Something isn't working
- `enhancement`: New feature or request
- `documentation`: Documentation improvements
- `good first issue`: Good for newcomers
- `help wanted`: Extra attention needed

## Development Tips

### Debugging

1. **React DevTools**: Install browser extension for component inspection
2. **Console Logging**: Use `console.log()` for debugging
3. **Breakpoints**: Use browser debugger for step-through debugging

### Testing Workout Generation

Test these scenarios:
- Individual split with 1, 2, and 3 muscles
- Each training split type
- Each goal type
- Edge cases (no muscles selected)

### Performance Testing

- Check bundle size: `npm run build`
- Test on slow connections
- Test on mobile devices
- Monitor console for warnings

## Questions?

If you have questions:
- Check existing documentation
- Search existing issues
- Create a new issue with the `question` label

## Recognition

Contributors will be recognized in:
- README.md contributors section
- Release notes
- Project documentation

Thank you for contributing to SweatPro! 💪
