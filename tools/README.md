# Zero Source LLM Bootstrapper

A powerful tool that generates complete software implementations from structured README files.

## What is Zero Source LLM Bootstrapper?

Zero Source LLM Bootstrapper is a Node.js-based tool that uses Large Language Models (LLMs) to automatically generate fully functional software implementations based on specifications defined in a structured README.md file. It bridges the gap between documentation and implementation by turning project specifications into working code.

## What Does It Do?

The LLM Bootstrapper:

1. **Analyzes** a Zero Source README.md file to understand project requirements
2. **Plans** the software architecture and component structure
3. **Generates** complete code implementation including:
   - Source code files for all components
   - Project configuration (package.json)
   - Tests
   - Documentation
4. **Validates** README files against the Zero Source specification

The tool leverages the power of Large Language Models to interpret natural language descriptions and convert them into working code, following best practices and architectural patterns.

## Required README Format

A Zero Source README.md must include the following sections:

- **Description** - Overview of the project
- **Functionality** - Features and capabilities
- **Technical Implementation** - Technical details and requirements

Additional sections may be included for more detailed specifications.

## Installation

```bash
npm install -g @zerosource/bootstrapper
```

Or use directly with npx:

```bash
npx @zerosource/bootstrapper <path-to-readme> [options]
```

## Usage

### Basic Usage

```bash
zerosource-bootstrapper path/to/README.md
```

This will analyze the README and generate a complete implementation in a 'generated' subdirectory.

### Options

| Option | Description |
|--------|-------------|
| `--output, -o <directory>` | Specify output directory for generated code |
| `--model, -m <model>` | Specify LLM model to use (default: gpt-4) |
| `--verbose, -v` | Show detailed generation process |
| `--validate` | Only validate the README, don't generate code |
| `--interactive` | Run in interactive mode with step-by-step guidance |
| `--lang <language>` | Specify preferred programming language |
| `--platform <platform>` | Specify target platform (web, mobile, desktop) |
| `--template <template>` | Use a specific project template |
| `--stats` | Show statistics about the generated code |
| `--cache` | Use cached generations when available |
| `--help, -h` | Show help message |

### Examples

#### Generate a Project

```bash
npx @zerosource/bootstrapper my-app/README.md --output my-app/implementation --lang JavaScript
```

This command will:
1. Read and analyze the README.md file in the my-app directory
2. Generate a complete JavaScript implementation in the my-app/implementation directory
3. Create all necessary files including source code, tests, and configuration

#### Validate a README Only

```bash
npx @zerosource/bootstrapper my-app/README.md --validate
```

This will check if the README follows the Zero Source specification without generating any code.

#### Interactive Mode

```bash
npx @zerosource/bootstrapper my-app/README.md --interactive
```

This launches an interactive prompt to guide you through the generation process with additional customization options.

## Example Workflow

1. **Create a Zero Source README.md file**:

```markdown
# My Todo App

## Description
A simple web-based todo application that allows users to add, edit, delete, and mark tasks as complete.

## Functionality
- Add new tasks with a title and optional description
- Mark tasks as complete
- Edit existing tasks
- Delete tasks
- Filter tasks by status (all, active, completed)
- Persist tasks in local storage

## Technical Implementation
- Frontend: HTML, CSS, JavaScript
- No backend required
- Local storage for data persistence
- Responsive design for mobile and desktop
```

2. **Generate the implementation**:

```bash
npx @zerosource/bootstrapper todo-app/README.md --output todo-app/src
```

3. **Run the generated application**:

```bash
cd todo-app/src
npm install
npm start
```

## Environment Variables

- `OPENAI_API_KEY`: Required for LLM-based generation and advanced validation

## Notes

- The generated code is a starting point and may require further refinement
- The quality of the generated code depends on the clarity and detail in your README
- LLM-based code generation works best with clear, specific requirements
