<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [README.md](https://github.com/agattani123/magnitude/blob/main/README.md)

</details>

# Introduction to Magnitude

Magnitude is a vision AI-powered browser automation tool that enables users to control their browsers using natural language. It leverages visually grounded language models to understand and interact with web interfaces, allowing for seamless navigation, interaction, data extraction, and verification tasks.

## Overview

Magnitude aims to solve two key problems in the browser automation space:

1. **Generalization**: Most browser agents rely on numbered boxes around page elements, which can be brittle and fail to generalize well on complex modern websites. Magnitude's vision-first architecture uses a visually grounded language model to specify pixel coordinates, enabling true generalization independent of DOM structure.

2. **Controllability and Repeatability**: Many browser agents follow a "high-level prompt + tools = work until done" approach, which works well for demos but may not be suitable for production environments. Magnitude offers flexible abstraction levels (granular actions vs. flows), custom actions and prompts at the agent and action levels, and a deterministic run via a native caching system (in progress).

## Key Features

### Navigation

Magnitude can understand and navigate any interface by visually interpreting the user interface and planning out the necessary actions.

Sources: [README.md:17](https://github.com/agattani123/magnitude/blob/main/README.md#L17)

### Interaction

The tool can execute precise actions using mouse and keyboard inputs, enabling seamless interaction with web applications.

Sources: [README.md:18](https://github.com/agattani123/magnitude/blob/main/README.md#L18)

### Data Extraction

Magnitude can intelligently extract useful structured data from web pages, making it easier to gather and process information.

Sources: [README.md:19](https://github.com/agattani123/magnitude/blob/main/README.md#L19)

### Verification

Magnitude includes a built-in test runner with powerful visual assertions, allowing users to verify the correctness of their web applications.

Sources: [README.md:20](https://github.com/agattani123/magnitude/blob/main/README.md#L20)

## Use Cases

Magnitude can be used for various purposes, including:

- Automating tasks on the web
- Integrating between applications without APIs
- Extracting data from web pages
- Testing web applications
- Building custom browser agents

Sources: [README.md:22-24](https://github.com/agattani123/magnitude/blob/main/README.md#L22-L24)

## Getting Started

### Running Browser Automation

To run your first browser automation with Magnitude, you can use the `create-magnitude-app` command:

```bash
npx create-magnitude-app
```

This command will create a new project and guide you through the setup process. It will also generate an example script that you can run immediately.

Sources: [README.md:29-33](https://github.com/agattani123/magnitude/blob/main/README.md#L29-L33)

### Using the Test Runner

To install the test runner for use in an existing web application, you can run the following commands:

```bash
npm i --save-dev magnitude-test && npx magnitude init
```

This will create a `tests/magnitude` directory with the following files:

- `magnitude.config.ts`: Magnitude test configuration file
- `example.mag.ts`: An example test file

For more information on running tests and integrating with CI/CD, refer to the [documentation](https://docs.magnitude.run/core-concepts/running-tests).

Sources: [README.md:36-44](https://github.com/agattani123/magnitude/blob/main/README.md#L36-L44)

### Language Model Requirements

Magnitude requires a large visually grounded language model for optimal performance. The recommended model is Claude Sonnet 4, but Magnitude is also compatible with Qwen-2.5VL 72B. For more information on language model configuration, refer to the [documentation](https://docs.magnitude.run/customizing/llm-configuration).

Sources: [README.md:46-48](https://github.com/agattani123/magnitude/blob/main/README.md#L46-L48)

## Conclusion

Magnitude is a powerful vision AI-powered browser automation tool that offers a unique vision-first architecture and flexible abstraction levels for controllable and repeatable automation. With its ability to navigate, interact, extract data, and verify web applications, Magnitude provides a comprehensive solution for various browser automation needs.