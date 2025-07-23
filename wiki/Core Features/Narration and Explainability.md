<details>
<summary>Relevant source files</summary>

The following files were used as context for generating this wiki page:

- [packages/magnitude-core/src/agent/narrator.ts](https://github.com/agattani123/magnitude/blob/main/packages/magnitude-core/src/agent/narrator.ts)

</details>

# Narration and Explainability

The "Narration and Explainability" feature in this project provides a way to log and visualize the actions, thoughts, and events that occur during the execution of an Agent or BrowserAgent. It helps developers understand the inner workings of the agent, track its progress, and debug any issues that may arise.

## Agent Narration

The `narrateAgent` function is responsible for setting up event listeners on an `Agent` instance and logging relevant information to the console. Here's a breakdown of the different events and their corresponding log messages:

### Agent Lifecycle Events

1. **Start Event:** When the agent starts, it logs a message indicating the start of the agent and the models being used.

```
▶ [start] agent started with <model_description>
```

2. **Stop Event:** When the agent stops, it logs a message indicating the end of the agent's execution, along with the total token usage and cost (if available).

```
■ [stop] agent stopped
Total usage: <input_tokens> input tokens [(<cache_write_tokens> cache write, <cache_read_tokens> cache read)] / <output_tokens> output tokens
Cost: $<total_cost> (or "None - using Claude Pro or Max subscription" if applicable)
```

### Agent Execution Events

1. **Thought Event:** When the agent has a thought, it logs the thought message.

```
⚙︎ <thought_message>
```

2. **Act Started Event:** When the agent starts a new task or action, it logs a message indicating the task name.

```
◆ [act] <task_name>
```

3. **Action Started Event:** When the agent starts executing a specific action, it logs a message describing the action.

```
<action_description>
```

The `narrateAgent` function also keeps track of various token usage metrics, such as total input tokens, output tokens, cached write and read tokens, and their corresponding costs. These metrics are displayed in the "Stop Event" log message.

## BrowserAgent Narration

The `narrateBrowserAgent` function extends the `narrateAgent` functionality by adding additional event listeners specific to the `BrowserAgent` class. It sets up the following event listeners:

1. **Navigation Event:** When the browser agent navigates to a new URL, it logs a message indicating the URL.

```
⛓︎ [nav] <url>
```

2. **Extract Started Event:** When the browser agent starts extracting data based on instructions and a schema, it logs a message indicating the instructions.

```
⛏ [extract] <instructions>
```

3. **Extract Done Event:** When the browser agent finishes extracting data, it logs the extracted data.

```
<extracted_data>
```

## Mermaid Diagrams

### Agent Narration Flow

```mermaid
flowchart TD
    subgraph Agent Narration
        start[Agent Start] -->|narrateAgent| setupListeners[Set up Event Listeners]
        setupListeners --> thoughtListener[Thought Event Listener]
        setupListeners --> actStartedListener[Act Started Event Listener]
        setupListeners --> actionStartedListener[Action Started Event Listener]
        setupListeners --> stopListener[Stop Event Listener]
        setupListeners --> startListener[Start Event Listener]
        startListener -->|Log Start Message| logStart
        thoughtListener -->|Log Thought| logThought
        actStartedListener -->|Log Act Started| logActStarted
        actionStartedListener -->|Log Action Started| logActionStarted
        stopListener -->|Log Stop Message| logStop
    end
    logStop -->|Display Token Usage and Cost| end[Agent Stop]

classDiagram
    class Agent {
        +events: EventEmitter
        +models: ModelManager
        +identifyAction(action: Action): ActionDefinition
    }
    class EventEmitter {
        +on(event: string, listener: Function): void
    }
    class ModelManager {
        +describe(): string
        +numUniqueModels: number
    }
    class Action {
        
    }
    class ActionDefinition {
        +render(action: Action): string
    }
    Agent *-- EventEmitter
    Agent *-- ModelManager
    Agent ..> Action
    Agent ..> ActionDefinition

Sources: [packages/magnitude-core/src/agent/narrator.ts:1-84]()
```

### BrowserAgent Narration Flow

```mermaid
sequenceDiagram
    participant BrowserAgent
    participant Agent
    participant NarratorModule

    NarratorModule->>Agent: narrateAgent(agent)
    Agent-->>NarratorModule: Set up Agent event listeners
    NarratorModule->>BrowserAgent: narrateBrowserAgent(browserAgent)
    BrowserAgent-->>NarratorModule: Set up BrowserAgent event listeners
    BrowserAgent->>NarratorModule: nav(url)
    NarratorModule-->>BrowserAgent: Log navigation event
    BrowserAgent->>NarratorModule: extractStarted(instructions, schema)
    NarratorModule-->>BrowserAgent: Log extract started event
    BrowserAgent->>NarratorModule: extractDone(instructions, data)
    NarratorModule-->>BrowserAgent: Log extracted data

Sources: [packages/magnitude-core/src/agent/narrator.ts:85-103]()
```

The `narrateBrowserAgent` function extends the `narrateAgent` functionality by adding event listeners specific to the `BrowserAgent` class. It sets up listeners for navigation, extract started, and extract done events, and logs the corresponding information to the console.

## Tables

### Agent Events

| Event | Description |
| --- | --- |
| `start` | Emitted when the agent starts. |
| `stop` | Emitted when the agent stops. |
| `thought` | Emitted when the agent has a thought. |
| `actStarted` | Emitted when the agent starts a new task or action. |
| `actionStarted` | Emitted when the agent starts executing a specific action. |
| `tokensUsed` | Emitted when the agent uses tokens, providing token usage metrics. |

Sources: [packages/magnitude-core/src/agent/narrator.ts:6-24](), [packages/magnitude-core/src/agent/narrator.ts:37-52]()

### BrowserAgent Events

| Event | Description |
| --- | --- |
| `nav` | Emitted when the browser agent navigates to a new URL. |
| `extractStarted` | Emitted when the browser agent starts extracting data based on instructions and a schema. |
| `extractDone` | Emitted when the browser agent finishes extracting data. |

Sources: [packages/magnitude-core/src/agent/narrator.ts:92-100]()

## Conclusion

The "Narration and Explainability" feature in this project provides a way to log and visualize the actions, thoughts, and events that occur during the execution of an Agent or BrowserAgent. It helps developers understand the inner workings of the agent, track its progress, and debug any issues that may arise. The `narrateAgent` and `narrateBrowserAgent` functions set up event listeners and log relevant information to the console, including token usage metrics, cost, and extracted data. This feature is essential for understanding and debugging the behavior of agents in the project.