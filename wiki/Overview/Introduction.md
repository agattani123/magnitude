<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [README.md](https://github.com/agattani123/magnitude/blob/main/README.md)
</details>

# Introduction

Magnitude is a vision AI-powered browser automation tool that enables users to control their browsers using natural language commands. It leverages visually grounded language models to understand and interact with web interfaces, allowing for seamless navigation, interaction, data extraction, and verification tasks. Magnitude aims to provide a flexible and future-proof solution for automating web-based tasks, integrating between applications without APIs, extracting structured data, and testing web applications.

## Key Features

### Navigation

Magnitude's vision AI capabilities allow it to understand and navigate any interface by analyzing the visual elements on the page. It can plan and execute actions based on the visual context, enabling seamless navigation through complex web applications.

### Interaction

In addition to navigation, Magnitude can interact with web interfaces by executing precise actions using mouse and keyboard inputs. This feature enables automation of tasks that require user interactions, such as clicking buttons, filling out forms, or dragging and dropping elements.

### Data Extraction

Magnitude can intelligently extract useful structured data from web pages. By leveraging its understanding of the visual context and the provided data schemas, it can identify and extract relevant information, making it easier to work with data from various sources.

### Verification and Testing

Magnitude includes a built-in test runner with powerful visual assertions. This feature allows developers to create and run automated tests for their web applications, ensuring the correctness and reliability of their user interfaces.

## Architecture

### Vision-First Approach

Magnitude's architecture is built around a vision-first approach, which sets it apart from traditional browser automation tools. Instead of relying on DOM structures or numbered boxes around page elements, Magnitude uses visually grounded language models to specify pixel coordinates for interactions.

This approach provides true generalization capabilities, making Magnitude independent of the underlying DOM structure and future-proof for various applications, including desktop apps and virtual machines.

### Controllable and Repeatable Automation

Magnitude addresses the limitations of traditional "high-level prompt + tools = work until done" approaches by offering a more controllable and repeatable automation experience. It achieves this through the following features:

1. **Flexible Abstraction Levels**: Magnitude supports both granular actions and higher-level flows, allowing users to choose the appropriate level of abstraction for their automation tasks.

2. **Custom Actions and Prompts**: Users can define custom actions and prompts at both the agent and action levels, enabling greater control and customization.

3. **Deterministic Runs via Native Caching System**: Magnitude's native caching system (currently in progress) aims to provide deterministic and repeatable automation runs, ensuring consistent behavior across different execution environments.

### Execution Flow

Magnitude's execution flow is designed to handle both high-level tasks and low-level actions. Here's an example of how it works:

```ts
// Magnitude can handle high-level tasks
await agent.act('Create a task', {
    // Optionally pass data that the agent will use where appropriate
    data: {
        title: 'Use Magnitude',
        description: 'Run "npx create-magnitude-app" and follow the instructions',
    },
});

// It can also handle low-level actions
await agent.act('Drag "Use Magnitude" to the top of the in progress column');

// Intelligently extract data based on the DOM content matching a provided zod schema
const tasks = await agent.extract(
    'List in progress tasks',
    z.array(z.object({
        title: z.string(),
        description: z.string(),
        // Agent can extract existing data or new insights
        difficulty: z.number().describe('Rate the difficulty between 1-5')
    })),
);
```

In this example, Magnitude can handle high-level tasks like "Create a task" by understanding the provided data and executing the necessary actions. It can also handle low-level actions like "Drag 'Use Magnitude' to the top of the in progress column" by directly interacting with the web interface.

Additionally, Magnitude can intelligently extract structured data from web pages based on the provided Zod schema. It can extract existing data as well as generate new insights, such as rating the difficulty of a task on a scale of 1-5.

Sources: [README.md](https://github.com/agattani123/magnitude/blob/main/README.md)

## Getting Started

Magnitude provides two main ways to get started: running your first browser automation or using the test runner for an existing web application.

### Running Your First Browser Automation

To create a new Magnitude project and run your first browser automation, you can use the `create-magnitude-app` command:

```bash
npx create-magnitude-app
```

This command will create a new project and guide you through the setup process for Magnitude. It will also generate an example script that you can run immediately to see Magnitude in action.

Sources: [README.md:54-58](https://github.com/agattani123/magnitude/blob/main/README.md#L54-L58)

### Using the Test Runner

If you have an existing web application and want to integrate Magnitude's test runner, you can install it using the following commands:

```bash
npm i --save-dev magnitude-test && npx magnitude init
```

This will create a `tests/magnitude` directory in your project with the following files:

- `magnitude.config.ts`: Magnitude test configuration file
- `example.mag.ts`: An example test file

For more information on running tests and integrating Magnitude into your CI/CD pipeline, refer to the [official documentation](https://docs.magnitude.run/core-concepts/running-tests).

Sources: [README.md:62-71](https://github.com/agattani123/magnitude/blob/main/README.md#L62-L71)

## Language Model Requirements

Magnitude requires a large visually grounded language model to function optimally. The recommended model is Claude Sonnet 4 for the best performance, but Magnitude is also compatible with Qwen-2.5VL 72B. For more information on configuring the language model, please refer to the [official documentation](https://docs.magnitude.run/customizing/llm-configuration).

Sources: [README.md:74-76](https://github.com/agattani123/magnitude/blob/main/README.md#L74-L76)

## Key Advantages

Magnitude addresses two common problems faced by traditional browser agents:

### Problem 1: Lack of Generalization

Most browser agents draw numbered boxes around page elements, which doesn't generalize well due to the complexity of modern websites.

**Magnitude's Solution: Vision-First Architecture**

- Visually grounded language models specify pixel coordinates for interactions.
- True generalization independent of DOM structure.
- Future-proof architecture for desktop apps, virtual machines, etc.

Sources: [README.md:81-86](https://github.com/agattani123/magnitude/blob/main/README.md#L81-L86)

### Problem 2: Limitations of "High-Level Prompt + Tools" Approach

Many browser agents follow the "high-level prompt + tools = work until done" approach, which works well for demonstrations but may not be suitable for production environments.

**Magnitude's Solution: Controllable and Repeatable Automation**

- Flexible abstraction levels (granular actions vs. flows).
- Custom actions and prompts at the agent and action levels.
- Deterministic runs via a native caching system (in progress).

Sources: [README.md:89-94](https://github.com/agattani123/magnitude/blob/main/README.md#L89-L94)

## Additional Resources

For more information on building Magnitude automations, test cases, and best practices, please refer to the [official documentation](https://docs.magnitude.run).

Sources: [README.md:97](https://github.com/agattani123/magnitude/blob/main/README.md#L97)

## Contact and Community

If you are an enterprise and require additional features or support, you can reach out to the Magnitude team at founders@magnitude.run or schedule a call [here](https://cal.com/tom-greenwald/30min) to discuss your needs.

You can also join the [Magnitude Discord community](https://discord.gg/VcdpMh9tTy) for help, suggestions, or to engage with other users.

Sources: [README.md:100-104](https://github.com/agattani123/magnitude/blob/main/README.md#L100-L104)

In summary, Magnitude is a powerful vision AI-powered browser automation tool that offers a flexible and future-proof solution for automating web-based tasks, integrating between applications, extracting structured data, and testing web applications. Its vision-first architecture and controllable automation approach set it apart from traditional browser agents, providing true generalization capabilities and a more reliable automation experience.