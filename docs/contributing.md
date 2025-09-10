# Contributing to CareerCraft AI

First off, thank you for considering contributing! This project is a demonstration of modern, full-stack application development, and your contributions can help make it an even better resource for the community.

##  Ways to Contribute

-   **Reporting Bugs**: If you find a bug, please open an issue and provide as much detail as possible.
-   **Suggesting Enhancements**: Have an idea for a new feature or an improvement to an existing one? Open an issue to start a discussion.
-   **Pull Requests**: If you're ready to contribute code, we welcome your pull requests.

## Development Workflow

1.  **Fork the Repository**: Start by forking the project to your own GitHub account.
2.  **Clone Your Fork**: Clone your forked repository to your local machine.
    ```bash
    git clone https://github.com/your-username/careercraft-ai.git
    ```
3.  **Create a Branch**: Create a new branch for your feature or bug fix. Use a descriptive name.
    ```bash
    # For a new feature
    git checkout -b feature/add-new-template

    # For a bug fix
    git checkout -b fix/fix-parsing-error
    ```
4.  **Make Changes**: Make your changes to the codebase.
5.  **Follow Commit Conventions**: We follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification. This helps us automate changelog generation and makes the commit history easier to read.

    -   **feat**: A new feature (e.g., `feat: add cover letter generator`).
    -   **fix**: A bug fix (e.g., `fix: correct PDF parsing logic`).
    -   **docs**: Changes to documentation only (e.g., `docs: update setup guide`).
    -   **style**: Changes that do not affect the meaning of the code (e.g., `style: format code with Prettier`).
    -   **refactor**: A code change that neither fixes a bug nor adds a feature (e.g., `refactor: simplify useUser hook`).
    -   **chore**: Changes to the build process or auxiliary tools (e.g., `chore: update npm dependencies`).

    Example commit message:
    ```
    feat: add minimalist resume template

    - Adds a new, clean template focused on ATS compatibility.
    - Includes component, preview image, and registration in the template library.
    ```

6.  **Push to Your Fork**: Push your changes to your forked repository.
    ```bash
    git push origin feature/add-new-template
    ```
7.  **Open a Pull Request**: Go to the original repository on GitHub and open a pull request from your forked branch.
    -   Provide a clear title and description for your PR.
    -   Reference any issues that your PR resolves (e.g., "Fixes #123").

## Code Style

-   This project uses **Prettier** for automatic code formatting. Please ensure your code is formatted before committing.
-   We use **TypeScript** for type safety. Please add types where appropriate.
-   Follow the existing coding patterns and style used throughout the project.

Thank you again for your interest in contributing!
