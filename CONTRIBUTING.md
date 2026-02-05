# Contributing to Habibit

Thank you for your interest in contributing to Habibit! This document provides guidelines and instructions for contributing to the project.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Coding Standards](#coding-standards)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Process](#pull-request-process)
- [Issue Reporting](#issue-reporting)

---

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment for all contributors, regardless of experience level, background, or identity.

### Expected Behavior

- Be respectful and considerate in all communications
- Provide constructive feedback
- Accept constructive criticism gracefully
- Focus on what is best for the project and community
- Show empathy towards other contributors

### Unacceptable Behavior

- Harassment, trolling, or discriminatory language
- Personal attacks or inflammatory comments
- Publishing others' private information without permission
- Other conduct that would be inappropriate in a professional setting

---

## How Can I Contribute?

### Reporting Bugs

Found a bug? Please help us fix it:

1. **Check existing issues** to avoid duplicates
2. **Create a new issue** with:
   - Clear, descriptive title
   - Steps to reproduce the bug
   - Expected vs. actual behavior
   - Platform/OS details (C++: compiler version, WordPress: PHP/MySQL versions)
   - Screenshots or error messages if applicable

**Bug Report Template:**
```markdown
**Platform**: [C++ / JavaScript / WordPress]
**OS**: [Windows 10 / macOS 12 / Ubuntu 22.04]
**Compiler/Browser/PHP Version**: [GCC 11.2 / Chrome 120 / PHP 8.0]

**Description**:
Brief description of the issue

**Steps to Reproduce**:
1. Step one
2. Step two
3. Step three

**Expected Behavior**:
What should happen

**Actual Behavior**:
What actually happens

**Error Messages/Screenshots**:
[Paste error messages or attach screenshots]
```

### Suggesting Enhancements

Have an idea for a new feature?

1. **Check existing feature requests** to see if it's already proposed
2. **Create an enhancement issue** explaining:
   - Problem the feature solves
   - Proposed solution or implementation approach
   - Alternative solutions considered
   - Which platforms it affects (C++ / JS / WordPress / All)

### Contributing Code

Areas where we especially welcome contributions:

**C++ Implementation:**
- [ ] Unit tests using Google Test
- [ ] Password hashing/encryption
- [ ] Input validation improvements
- [ ] File-based persistence (JSON/SQLite)
- [ ] Multi-language support

**JavaScript Implementation:**
- [ ] Modern UI framework integration (React, Vue)
- [ ] LocalStorage persistence
- [ ] Unit tests with Jest
- [ ] TypeScript migration

**WordPress Platform:**
- [ ] Docker Compose configuration
- [ ] Payment gateway integration (Stripe, PayPal)
- [ ] Custom rental duration selector
- [ ] Rating and review system
- [ ] Email notifications
- [ ] RESTful API endpoints

**Documentation:**
- [ ] Video tutorials
- [ ] API documentation
- [ ] Deployment guides
- [ ] Internationalization (i18n)

---

## Getting Started

### Prerequisites

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
```bash
git clone https://github.com/YOUR_USERNAME/Habibit.git
cd Habibit
```

3. **Add upstream remote**:
```bash
git remote add upstream https://github.com/jakujobi/Habibit.git
```

4. **Set up development environment** - See [DEVELOPMENT.md](./docs/DEVELOPMENT.md)

---

## Development Workflow

### 1. Create a Branch

Always create a new branch for your work:

```bash
# Update main branch
git checkout main
git pull upstream main

# Create feature branch
git checkout -b feature/your-feature-name

# Or for bug fixes
git checkout -b fix/bug-description
```

**Branch Naming Convention:**
- `feature/add-rating-system`
- `fix/cost-calculation-bug`
- `docs/update-readme`
- `refactor/improve-equipment-class`
- `test/add-borrower-tests`

### 2. Make Your Changes

**For C++ changes:**
```bash
cd "Cpp Impelemtation"

# Edit files
nano Habibit.cpp

# Compile and test
g++ -std=c++11 -Wall -Wextra -o habibit Habibit.cpp mainFile.cpp
./habibit

# Verify no warnings
g++ -std=c++11 -Wall -Wextra -Werror -o habibit Habibit.cpp mainFile.cpp
```

**For JavaScript changes:**
```bash
cd "Cpp Impelemtation/Javascript implementation"

# Edit Habibit.js
# Test in browser console
```

**For WordPress changes:**
```bash
cd habifarm/app/public/wp-content

# Make changes to theme/plugin
# Test in local WordPress installation
# Clear cache and test thoroughly
```

### 3. Test Your Changes

**Manual Testing Checklist:**
- [ ] Compile/run successfully on your platform
- [ ] Existing functionality still works
- [ ] New feature works as expected
- [ ] Edge cases handled properly
- [ ] No new compiler warnings
- [ ] Error messages are clear and helpful

**Automated Testing** (if applicable):
```bash
# C++ (when tests exist)
./run_tests

# JavaScript (when tests exist)
npm test

# WordPress (when tests exist)
phpunit
```

### 4. Commit Your Changes

See [Commit Guidelines](#commit-guidelines) below.

### 5. Push to Your Fork

```bash
git push origin feature/your-feature-name
```

### 6. Open a Pull Request

See [Pull Request Process](#pull-request-process) below.

---

## Coding Standards

### C++ Style Guide

**Code Formatting:**
- Indentation: 4 spaces (no tabs)
- Line length: 80-100 characters recommended
- Braces: K&R style (opening brace on same line for functions)

**Naming:**
- Classes: `PascalCase` (e.g., `Equipment`)
- Functions: `camelCase` (e.g., `calculateTotalCost`)
- Variables: `camelCase` (e.g., `costPerDay`)
- Constants: `UPPER_SNAKE_CASE` (e.g., `TAX_RATE`)

**Best Practices:**
- Use `const` for read-only parameters
- Prefer `std::string` over C-style strings
- Initialize variables in constructor initialization list
- Add comments for complex logic
- Use meaningful variable names

**Example:**
```cpp
class Equipment {
private:
    string name;
    double costPerDay;

public:
    Equipment(const string& n, double cost)
        : name(n), costPerDay(cost) {}
    
    // Get the daily rental cost
    double getCostPerDay() const {
        return costPerDay;
    }
};
```

### JavaScript Style Guide

**Code Formatting:**
- Indentation: 2 spaces
- Semicolons: Required
- Quotes: Single quotes for strings

**Naming:**
- Classes: `PascalCase`
- Functions/Variables: `camelCase`
- Constants: `UPPER_SNAKE_CASE`

**Best Practices:**
- Use `const` for immutable, `let` for mutable (avoid `var`)
- Use template literals for string concatenation
- Use arrow functions where appropriate
- Add JSDoc comments for functions

**Example:**
```javascript
/**
 * Represents rentable equipment
 */
class Equipment {
  constructor(name, costPerDay) {
    this.name = name;
    this.costPerDay = costPerDay;
  }
  
  /**
   * Get the daily rental cost
   * @returns {number} Cost per day
   */
  getCostPerDay() {
    return this.costPerDay;
  }
}
```

### WordPress/PHP Style Guide

Follow [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/)

**Key Points:**
- Indentation: Tabs (not spaces)
- Braces: Opening brace on same line
- Function names: `snake_case`
- Prefix custom functions: `habibit_`

**Example:**
```php
<?php
/**
 * Calculate total cost with tax
 *
 * @param float $subtotal Subtotal before tax
 * @return float Total with tax
 */
function habibit_calculate_total( $subtotal ) {
    $tax_rate = 0.025;
    $tax = $subtotal * $tax_rate;
    return $subtotal + $tax;
}
```

### Documentation Style

**Code Comments:**
- Explain WHY, not WHAT (code should be self-documenting)
- Use single-line comments (`//`) for brief notes
- Use multi-line comments (`/* */`) for function headers

**Markdown Documentation:**
- Use headers for structure
- Code blocks with language identifiers
- Tables for structured data
- Links to related documentation

---

## Commit Guidelines

### Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Type:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code formatting (no logic change)
- `refactor`: Code restructuring (no feature change)
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Scope** (optional):
- `cpp`: C++ implementation
- `js`: JavaScript implementation
- `wordpress`: WordPress platform
- `docs`: Documentation
- `build`: Build system

**Subject:**
- Imperative mood ("add" not "added")
- Lowercase (except proper nouns)
- No period at the end
- Max 50 characters

**Examples:**

```
feat(cpp): add rating system for equipment

Implement a 5-star rating system allowing borrowers to rate
equipment after rental completion. Ratings are stored in-memory
and displayed when viewing equipment details.

Closes #42
```

```
fix(js): correct tax calculation in checkout

The tax was being applied twice due to a logic error in the
calculateTotalCost() function. Now correctly applies 2.5% tax
once to the subtotal.

Fixes #38
```

```
docs: add WordPress setup instructions to README

Added step-by-step guide for setting up the WordPress platform
using Local by Flywheel and XAMPP.
```

### Atomic Commits

- One logical change per commit
- Commit should compile and pass tests
- Don't mix refactoring with feature additions

**Good:**
```
✓ feat(cpp): add Equipment constructor validation
✓ test(cpp): add tests for Equipment class
✓ docs: update DEVELOPMENT.md with testing guide
```

**Bad:**
```
✗ fix bugs and add features and update docs
```

---

## Pull Request Process

### Before Submitting

**Checklist:**
- [ ] Code follows style guidelines
- [ ] Code compiles without warnings
- [ ] Existing tests pass (if any)
- [ ] New tests added for new features
- [ ] Documentation updated if needed
- [ ] Commits follow commit guidelines
- [ ] Branch is up-to-date with upstream main

**Update Your Branch:**
```bash
git checkout main
git pull upstream main
git checkout feature/your-feature
git rebase main
# Resolve conflicts if any
git push --force-with-lease origin feature/your-feature
```

### Submitting the PR

1. **Navigate to your fork** on GitHub
2. **Click "Compare & pull request"**
3. **Fill out the PR template:**

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Refactoring
- [ ] Other (specify):

## Platform(s) Affected
- [ ] C++ Implementation
- [ ] JavaScript Implementation
- [ ] WordPress Platform
- [ ] Documentation

## Testing
Describe testing performed:
- Manual testing steps
- Test results
- Platforms tested (OS, compiler/browser versions)

## Screenshots
If applicable, add screenshots

## Related Issues
Closes #issue_number
Related to #issue_number

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No new warnings
```

4. **Submit the PR**

### PR Review Process

1. **Automated checks** will run (if configured)
2. **Maintainers will review** your code
3. **Address feedback** by pushing new commits
4. **Once approved**, maintainers will merge

**During Review:**
- Respond to comments promptly
- Be open to suggestions
- Make requested changes in new commits (don't force-push during review)
- Discussion is encouraged!

---

## Issue Reporting

### Security Vulnerabilities

⚠️ **Do NOT open public issues for security vulnerabilities**

See [SECURITY.md](./SECURITY.md) for reporting process.

### Bug Reports

Use the bug report template in the issue tracker.

### Feature Requests

Use the feature request template in the issue tracker.

### Questions

For questions about using Habibit:
- Check [README.md](./README.md)
- Check [DEVELOPMENT.md](./docs/DEVELOPMENT.md)
- Search existing issues
- Open a discussion (if available) or issue with `question` label

---

## Recognition

Contributors will be recognized in:
- Git commit history
- GitHub contributors page
- Project documentation (for significant contributions)

---

## License

By contributing to Habibit, you agree that your contributions will be licensed under the [GNU General Public License v3.0](./LICENSE).

---

## Questions?

- Open an issue with the `question` label
- Contact maintainers via GitHub

**Thank you for contributing to Habibit! 🎉**
