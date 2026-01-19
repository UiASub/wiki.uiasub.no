# Contributing

Thank you for your interest in contributing to the UiASub website! This guide will help you get started.

## How to Contribute

### Types of Contributions

We welcome:

- **Bug fixes**
- **New features**
- **Documentation improvements**
- **Design enhancements**
- **Accessibility improvements**
- **Translation updates**
- **Code refactoring**

## Getting Started

### 1. Fork the Repository

1. Visit [github.com/UiASub/uiasub.github.io](https://github.com/UiASub/uiasub.github.io)
2. Click the "Fork" button in the top-right corner
3. Clone your fork:

```bash
git clone https://github.com/YOUR-USERNAME/uiasub.github.io.git
cd uiasub.github.io
```

### 2. Set Up Development Environment

```bash
# Add upstream remote
git remote add upstream https://github.com/UiASub/uiasub.github.io.git

# Create a feature branch
git checkout -b feature/your-feature-name

# Start local server
python3 -m http.server
```

### 3. Make Your Changes

- Follow the [Code Standards](Code‐Standards)
- Test your changes thoroughly
- Ensure both Norwegian and English versions work
- Check responsive design

### 4. Commit Your Changes

```bash
git add .
git commit -m "Brief description of changes"
```

**Commit message guidelines**:

- Use present tense ("Add feature" not "Added feature")
- Keep first line under 50 characters
- Add detailed description after blank line if needed

### 5. Push and Create Pull Request

```bash
git push origin feature/your-feature-name
```

Then:

1. Go to your fork on GitHub
2. Click "New Pull Request"
3. Fill out the PR template
4. Submit for review

## Pull Request Guidelines

### PR Title Format

Use conventional commit format:

- `feat: Add new feature`
- `fix: Resolve bug in navigation`
- `docs: Update README`
- `style: Improve button styling`
- `refactor: Restructure member loading`
- `test: Add validation tests`
- `chore: Update dependencies`

### PR Description

Include:

- **What**: What changes were made
- **Why**: Why these changes were needed
- **How**: How the changes work
- **Testing**: How you tested the changes
- **Screenshots**: For visual changes

**Example**:

```markdown
## What
Added dark mode toggle to header

## Why
Users requested dark mode for better viewing at night

## How
- Created CSS variables for dark theme
- Added toggle button to header
- Persisted preference in localStorage

## Testing
- Tested on Chrome, Firefox, Safari
- Tested on mobile and desktop
- Verified persistence across page loads

## Screenshots
[Include before/after screenshots]
```

### PR Checklist

Before submitting:

- [ ] Code follows project conventions
- [ ] Changes work on both Norwegian and English versions
- [ ] Responsive design tested (mobile, tablet, desktop)
- [ ] No console errors
- [ ] Commit messages are clear
- [ ] PR description is complete

## Code Review Process

### What to Expect

1. **Initial Review**: Within 1-3 days
2. **Feedback**: Reviewers may request changes
3. **Iteration**: Make requested changes and push updates
4. **Approval**: Once approved, maintainers will merge
5. **Deployment**: Changes go live automatically

### Responding to Feedback

- Be open to suggestions
- Ask questions if feedback is unclear
- Make requested changes promptly
- Thank reviewers for their time

## Contribution Areas

### Good First Issues

Look for issues labeled `good-first-issue`:

- Documentation typos
- Simple bug fixes
- Adding translations
- Minor styling improvements

### Priority Areas

Current priorities:

1. **Accessibility**: WCAG 2.1 AA compliance
2. **Performance**: Image optimization, code splitting
3. **Internationalization**: Supporting more languages
4. **Testing**: Adding automated tests

## Style Guide

### HTML

```html
<!-- Good: Semantic, accessible -->
<section class="team-section" aria-label="Our team members">
  <h2>Our Team</h2>
  <img src="member.jpg" alt="Team member portrait">
</section>

<!-- Avoid: Non-semantic, no accessibility -->
<div class="team-section">
  <h3>Our Team</h3>
  <img src="member.jpg">
</div>
```

### CSS

```css
/* Good: Variables, clear names */
.member-card {
  padding: var(--spacing-lg);
  background: var(--color-surface);
  border-radius: var(--radius-lg);
}

/* Avoid: Magic numbers, unclear names */
.card {
  padding: 24px;
  background: #f8f9fa;
  border-radius: 16px;
}
```

### JavaScript

```javascript
// Good: Modern, error handling
const fetchData = async () => {
  try {
    const response = await fetch('/data/members.json')
    return await response.json()
  } catch (error) {
    console.error('Failed to fetch:', error)
    return []
  }
}

// Avoid: Callback hell, no error handling
function fetchData() {
  var xhr = new XMLHttpRequest()
  xhr.onload = function() {
    var data = JSON.parse(xhr.responseText)
    // ...
  }
  xhr.open('GET', '/data/members.json')
  xhr.send()
}
```

## Reporting Issues

### Bug Reports

Include:

- **Description**: What's wrong
- **Steps to reproduce**: How to trigger the bug
- **Expected behavior**: What should happen
- **Actual behavior**: What actually happens
- **Screenshots**: If visual
- **Environment**: Browser, OS, device

**Template**:

```markdown
**Bug Description**
Navigation menu doesn't close on mobile

**Steps to Reproduce**
1. Open site on mobile device
2. Tap hamburger menu
3. Tap a link
4. Menu stays open

**Expected**
Menu should close after link click

**Actual**
Menu remains open

**Environment**
- Browser: Safari on iOS 17
- Device: iPhone 14
```

### Feature Requests

Include:

- **Feature description**: What you'd like added
- **Use case**: Why it's needed
- **Alternatives**: Other solutions you've considered
- **Additional context**: Mockups, examples

## Community Guidelines

### Code of Conduct

- Be respectful and inclusive
- Welcome newcomers
- Provide constructive feedback
- Focus on the issue, not the person
- Follow Norwegian and English language guidelines

### Communication

- **GitHub Issues**: Bug reports, feature requests
- **Pull Requests**: Code contributions
- **Email**: <uia.submerged@gmail.com> for private matters

## Recognition

Contributors will be:

- Acknowledged in release notes
- Listed in GitHub contributors
- Thanked publicly on social media (with permission)

## License

By contributing, you agree that your contributions will be licensed under the same license as the project (typically MIT or similar).

## Questions?

- Check the [Wiki](Home) documentation
- Search existing GitHub issues
- Open a new issue with your question
- Email the team: <uia.submerged@gmail.com>

## Thank You

Every contribution, no matter how small, helps improve the UiASub website. We appreciate your time and effort!

---

[← Development Workflow](Development-Workflow) | [Back to Home](Home)
