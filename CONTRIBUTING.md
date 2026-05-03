# Contributing to Xync Website

Thank you for your interest in contributing! This document provides guidelines for contributing to the Xync website project.

## Getting Started

1. **Fork the repository**
2. **Clone your fork:**
   ```bash
   git clone https://github.com/yourusername/xync-website.git
   cd xync-website
   ```
3. **Create a branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```

## Development Setup

Since this is a static HTML website, development is simple:

```bash
# Option 1: Python
python3 -m http.server 8000

# Option 2: Node.js
npx http-server

# Option 3: PHP
php -S localhost:8000
```

Visit `http://localhost:8000` to see your changes.

## Code Style Guidelines

### HTML
- Use semantic HTML5 elements
- Maintain consistent indentation (2 spaces)
- Keep inline comments descriptive
- Use meaningful IDs and classes

### CSS
- Follow the existing CSS variable naming convention
- Use kebab-case for class names
- Group related styles together
- Comment major sections
- Mobile-first approach (base styles, then `@media` queries)

### JavaScript
- Use ES6+ syntax
- Keep functions small and focused
- Use meaningful variable names
- Add comments for complex logic
- Follow existing naming conventions (camelCase for variables/functions)

### Example:
```javascript
// Good
function calculateScore(answers) {
  const totalScore = answers.reduce((sum, answer) => sum + answer.value, 0);
  return totalScore;
}

// Avoid
function calc(a) {
  let t = 0;
  for(let i = 0; i < a.length; i++) t += a[i].v;
  return t;
}
```

## Making Changes

### Small Changes
- Fix typos
- Update copy/content
- Small CSS adjustments
- Bug fixes

**Process:**
1. Make your changes
2. Test in multiple browsers (Chrome, Firefox, Safari)
3. Test on mobile
4. Commit with a clear message:
   ```bash
   git commit -m "Fix: Correct typo in About page headline"
   ```

### Major Changes
- New features
- Significant redesigns
- New pages
- Assessment question changes

**Process:**
1. Open an issue first to discuss the change
2. Wait for approval before implementing
3. Make changes in a feature branch
4. Test thoroughly
5. Submit PR with detailed description

## Testing Checklist

Before submitting a PR, ensure:

- [ ] All pages load correctly
- [ ] Navigation works on all pages
- [ ] Mobile menu functions properly
- [ ] Ops Clarity Index assessment works end-to-end
- [ ] Forms don't break
- [ ] No console errors
- [ ] Responsive on mobile (375px width minimum)
- [ ] Responsive on tablet (768px)
- [ ] Responsive on desktop (1200px+)
- [ ] Tested in Chrome
- [ ] Tested in Firefox
- [ ] Tested in Safari (if possible)
- [ ] No broken links

## Browser Support

Test your changes in:
- **Chrome** (latest)
- **Firefox** (latest)
- **Safari** (latest, if on Mac)
- **Mobile Safari** (iOS)
- **Chrome Mobile** (Android)

## Commit Message Format

Use clear, descriptive commit messages:

```
Type: Brief description

Longer description if needed (optional)

Examples:
- Fix: Correct calculation in assessment scoring
- Feature: Add new service tier to Services page
- Style: Update color scheme for better contrast
- Docs: Update README with deployment instructions
- Refactor: Simplify navigation JavaScript
```

**Types:**
- `Fix:` - Bug fixes
- `Feature:` - New features
- `Style:` - Visual/CSS changes
- `Docs:` - Documentation updates
- `Refactor:` - Code restructuring without functionality changes
- `Test:` - Adding or updating tests
- `Chore:` - Maintenance tasks

## Pull Request Process

1. **Update your branch:**
   ```bash
   git fetch origin
   git rebase origin/main
   ```

2. **Push your changes:**
   ```bash
   git push origin feature/your-feature-name
   ```

3. **Create Pull Request:**
   - Go to GitHub
   - Click "New Pull Request"
   - Select your branch
   - Fill out the template:

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Tested in Chrome
- [ ] Tested in Firefox
- [ ] Tested on mobile
- [ ] Assessment still works
- [ ] No console errors

## Screenshots (if applicable)
Add before/after screenshots for visual changes
```

4. **Wait for Review:**
   - Address any feedback
   - Make requested changes
   - Push updates to the same branch

5. **Merge:**
   - Once approved, maintainers will merge
   - Delete your feature branch after merge

## Reporting Bugs

Use GitHub Issues with this template:

```markdown
## Bug Description
Clear description of the bug

## Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. Scroll down to '...'
4. See error

## Expected Behavior
What should happen

## Actual Behavior
What actually happens

## Screenshots
If applicable

## Environment
- Browser: Chrome 120
- OS: macOS 14.1
- Device: Desktop
```

## Feature Requests

Use GitHub Issues with this template:

```markdown
## Feature Description
Clear description of the proposed feature

## Problem It Solves
What problem does this address?

## Proposed Solution
How should it work?

## Alternatives Considered
What other approaches did you consider?

## Additional Context
Any other relevant information
```

## Code of Conduct

### Our Standards
- Be respectful and inclusive
- Welcome newcomers
- Accept constructive criticism
- Focus on what's best for the project

### Unacceptable Behavior
- Harassment or discrimination
- Trolling or insulting comments
- Personal or political attacks
- Publishing others' private information

## Questions?

- Open a GitHub Discussion
- Comment on related issues
- Contact the maintainers

## Recognition

Contributors will be recognized in:
- GitHub contributors list
- Release notes (for significant contributions)

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to Xync! 🚀
