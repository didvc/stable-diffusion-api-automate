# Contributing to Stable Diffusion API Automate

Thank you for your interest in contributing to this project! We welcome contributions from the community and appreciate your effort to help improve the tool.

## Table of Contents

- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [How to Contribute](#how-to-contribute)
- [Code Style and Standards](#code-style-and-standards)
- [Testing](#testing)
- [Submitting Changes](#submitting-changes)
- [Contributor License Agreement (CLA)](#contributor-license-agreement-cla)
- [Code of Conduct](#code-of-conduct)
- [Getting Help](#getting-help)

## Getting Started

Before contributing, please:

1. Read through the [README.md](README.md) to understand the project
2. Check existing [issues](https://github.com/didvc/stable-diffusion-api-automate/issues) to see if your idea or bug report already exists
3. Look at [pull requests](https://github.com/didvc/stable-diffusion-api-automate/pulls) to see what's being worked on
4. Review this contributing guide thoroughly

## Development Setup

### Prerequisites

- Node.js (version 18 or higher)
- npm or yarn package manager
- Stable Diffusion WebUI with API enabled
- Git for version control

### Local Development

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/stable-diffusion-api-automate.git
   cd stable-diffusion-api-automate
   ```

3. Install dependencies:
   ```bash
   npm install
   ```

4. Set up your environment:
   ```bash
   cp .env.example .env
   # Edit .env with your local WebUI settings
   ```

5. Create a new branch for your changes:
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/your-bug-fix
   ```

## How to Contribute

### Reporting Bugs

When reporting bugs, please include:

- **Clear title** describing the issue
- **Steps to reproduce** the problem
- **Expected behavior** vs **actual behavior**
- **Environment details**:
  - Operating system and version
  - Node.js version
  - WebUI version
  - Tool version
- **Configuration files** (with sensitive data removed)
- **Error messages** or logs
- **Screenshots** if applicable

### Suggesting Enhancements

For feature requests, please provide:

- **Clear description** of the proposed feature
- **Use case** explaining why this would be valuable
- **Implementation ideas** if you have any
- **Alternatives considered**
- **Backward compatibility** considerations

### Types of Contributions Welcome

- **Bug fixes**
- **Feature enhancements**
- **Documentation improvements**
- **Performance optimizations**
- **Test coverage improvements**
- **Code quality improvements**
- **Translation and localization**
- **Example configurations and templates**

## Code Style and Standards

### TypeScript Guidelines

- Use TypeScript for all new code
- Follow existing code patterns and naming conventions
- Use meaningful variable and function names
- Add type annotations where helpful for clarity
- Avoid `any` types when possible

### Code Formatting

- Use 2 spaces for indentation
- Use semicolons consistently
- Use single quotes for strings
- Maximum line length of 100 characters
- Use trailing commas in multiline structures

### Best Practices

- **Error handling**: Always handle errors gracefully
- **Logging**: Use the existing logging system with appropriate levels
- **Configuration**: Make features configurable when reasonable
- **Documentation**: Update documentation for new features
- **Backward compatibility**: Avoid breaking changes when possible

### Example Code Style

```typescript
interface GenerationConfig {
  prompt: string;
  negativePrompt: string;
  width: number;
  height: number;
  steps: number;
}

async function processConfig(config: GenerationConfig): Promise<void> {
  try {
    log('Processing configuration...', 'info');
    // Implementation here
  } catch (error) {
    log(`Error processing config: ${error}`, 'error');
    throw error;
  }
}
```

## Testing

### Running Tests

```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage
```

### Writing Tests

- Write tests for new features and bug fixes
- Use descriptive test names
- Test both success and failure cases
- Mock external dependencies (WebUI API calls)
- Keep tests focused and independent

### Test Structure

```typescript
describe('Configuration Loading', () => {
  it('should load valid JSONL configuration file', async () => {
    // Test implementation
  });

  it('should handle invalid JSON gracefully', async () => {
    // Test implementation
  });
});
```

## Submitting Changes

### Pull Request Process

1. **Update documentation** if needed
2. **Add tests** for new functionality
3. **Ensure all tests pass**
4. **Update CHANGELOG.md** with your changes
5. **Commit with descriptive messages**

### Commit Message Format

Use conventional commits format:

```
type(scope): brief description

Longer description if needed

Fixes #123
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes
- `refactor`: Code refactoring
- `test`: Test additions or changes
- `chore`: Build process or auxiliary tool changes

**Examples:**
```
feat(cli): add support for custom output directories

fix(progress): resolve progress bar display issues on Windows

docs(readme): update installation instructions
```

### Pull Request Template

When submitting a PR, please:

- **Use a clear title** describing the change
- **Fill out the PR template** completely
- **Link related issues** using `Fixes #123` or `Closes #123`
- **Add screenshots** for UI changes
- **Request review** from appropriate maintainers

## Contributor License Agreement (CLA)

By submitting a pull request or contribution, you agree to the following:

> You grant the project founder a **non-exclusive, irrevocable, worldwide, royalty-free license** to use, modify, sublicense, and relicense your contribution, including the right to incorporate it into dual-licensed or commercial versions of the project.

This ensures that the project can grow sustainably while preserving creator rights.  
If you are contributing on behalf of a company or organization, please contact us in advance.

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment for all contributors, regardless of:

- Age, body size, disability, ethnicity, gender identity and expression
- Level of experience, education, socio-economic status
- Nationality, personal appearance, race, religion
- Sexual identity and orientation

### Expected Behavior

- **Be respectful** and considerate in all interactions
- **Be constructive** in feedback and discussions
- **Focus on the code and ideas**, not the person
- **Help others learn** and grow
- **Be patient** with newcomers

### Unacceptable Behavior

- Harassment, discrimination, or offensive comments
- Personal attacks or trolling
- Publishing private information without permission
- Spam or irrelevant content
- Any other conduct that could reasonably be considered inappropriate

### Enforcement

Instances of unacceptable behavior may be reported to the project maintainers. All complaints will be reviewed and investigated promptly and fairly.

## Getting Help

### Communication Channels

- **GitHub Issues**: For bug reports and feature requests
- **GitHub Discussions**: For general questions and community discussion
- **Pull Request Comments**: For code review and implementation discussion

### Documentation

- [README.md](README.md): Project overview and basic usage
- [API Documentation](docs/api.md): Detailed API reference
- [Configuration Guide](docs/configuration.md): Advanced configuration options
- [Troubleshooting Guide](docs/troubleshooting.md): Common issues and solutions

### Development Questions

When asking for help:

1. **Search existing issues** and discussions first
2. **Provide context** about what you're trying to achieve
3. **Include relevant code** or configuration
4. **Share error messages** or unexpected behavior
5. **Mention your environment** (OS, Node.js version, etc.)

## Recognition

Contributors will be recognized in:

- **CHANGELOG.md** for their specific contributions
- **README.md** in the contributors section
- **Release notes** for significant contributions

We appreciate all contributions, from code to documentation to bug reports!

---

Thank you for contributing to Stable Diffusion API Automate! Your contributions help make this tool better for everyone in the community.