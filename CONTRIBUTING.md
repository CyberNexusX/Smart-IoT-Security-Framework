# Contributing to Smart IoT Security Framework

Thank you for your interest in contributing to the Smart IoT Security Framework! This document provides guidelines and instructions for contributing.

## Code of Conduct

By participating in this project, you agree to maintain a respectful and inclusive environment for everyone.

## How Can I Contribute?

### Reporting Bugs

- Check the issue tracker to see if the bug has already been reported
- Use the bug report template when creating a new issue
- Include detailed steps to reproduce the bug
- Provide information about your environment (OS, Python version, etc.)

### Suggesting Enhancements

- Check existing enhancement suggestions before creating a new one
- Provide a clear description of the enhancement
- Explain why this enhancement would be useful to most users

### Pull Requests

1. Fork the repository
2. Create a new branch for your feature or bugfix
3. Make your changes
4. Add or update tests as necessary
5. Make sure all tests pass
6. Submit a pull request

## Development Setup

1. Clone your fork of the repository
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   pip install -r requirements-dev.txt
   ```
3. Set up pre-commit hooks:
   ```bash
   pre-commit install
   ```

## Coding Guidelines

- Follow PEP 8 style guidelines
- Write docstrings for all functions, classes, and methods
- Add type hints where appropriate
- Keep functions small and focused on a single responsibility
- Write unit tests for new functionality

## Testing

- Run the test suite before submitting a pull request:
  ```bash
  pytest
  ```
- Aim for high test coverage for new code

## Documentation

- Update documentation for any changes to the API or functionality
- Keep the README up to date
- Add examples for new features

## Security Considerations

- Be cautious when handling security-related code
- Never commit sensitive information (API keys, credentials, etc.)
- Report security vulnerabilities directly to the maintainers rather than opening public issues

## Commit Messages

- Use clear and descriptive commit messages
- Structure commit messages as follows:
  ```
  Short summary (limit 50 chars)

  More detailed explanatory text, if necessary. Wrap it to about 72
  characters. The blank line separating the summary from the body is
  critical.
  ```

## Pull Request Process

1. Update the README.md with details of changes if applicable
2. Update the documentation if necessary
3. The PR should work on the main supported Python versions
4. Your PR needs approval from at least one maintainer before merging

## Questions?

If you have any questions about contributing, please open an issue labeled 'question'.

Thank you for your contributions!
