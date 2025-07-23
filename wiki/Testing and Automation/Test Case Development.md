<details>
<summary>Relevant source files</summary>

The following files were used as context for generating this wiki page:

- [docs/testing/building-test-cases.mdx](https://github.com/agattani123/magnitude/blob/main/docs/testing/building-test-cases.mdx)
- [docs/testing/test-configuration.mdx](https://github.com/agattani123/magnitude/blob/main/docs/testing/test-configuration.mdx)

</details>

# Test Case Development

## Introduction

Test Case Development in Magnitude involves creating and configuring test cases that represent user flows in a web application. Each test case navigates to a URL, executes a series of steps, and verifies expected conditions (checks) along the way. The purpose is to ensure the application behaves as intended under various scenarios.

Sources: [docs/testing/building-test-cases.mdx](https://github.com/agattani123/magnitude/blob/main/docs/testing/building-test-cases.mdx:1-11)

## Test Case Structure

A test case in Magnitude is defined using the `test` function, which takes a description and an asynchronous callback function that receives an `agent` object. The `agent` is used to perform actions (`act`) and assertions (`check`) within the test case.

```typescript
test('can add and remove todos', async (agent) => {
    await agent.act('Add a todo');
    await agent.act('Remove the todo');
});
```

Sources: [docs/testing/building-test-cases.mdx:14-19](https://github.com/agattani123/magnitude/blob/main/docs/testing/building-test-cases.mdx:14-19)

### Test Case Configuration

Test cases can be configured with a different starting URL by providing an options object as the second argument to the `test` function.

```typescript
test('can add and remove todos', { url: "https://mytodoapp.com" }, async (agent) => {
    await agent.act('Add a todo');
    await agent.act('Remove the todo');
});
```

Sources: [docs/testing/building-test-cases.mdx:24-28](https://github.com/agattani123/magnitude/blob/main/docs/testing/building-test-cases.mdx:24-28)

## Test Steps

Test steps represent individual actions or user flows within a test case. Each step is defined by providing a description to the `agent.act` method.

```typescript
test('example', async (agent) => {
    await agent.act('Log in'); // step description
});
```

Step descriptions should be specific enough to understand the intended action but not overly detailed.

Sources: [docs/testing/building-test-cases.mdx:32-38](https://github.com/agattani123/magnitude/blob/main/docs/testing/building-test-cases.mdx:32-38)

### Checks

Checks are natural language visual assertions that can be added to any step in a test case. They are used to verify the expected state of the application after a step is executed.

```typescript
test('example', async (agent) => {
    await agent.act('Log in');
    await agent.check('Dashboard is visible');
});
```

Examples of valid checks include verifying the visibility of elements, validating text content, or ensuring the correctness of responses.

Sources: [docs/testing/building-test-cases.mdx:41-52](https://github.com/agattani123/magnitude/blob/main/docs/testing/building-test-cases.mdx:41-52)

### Test Data

Test data can be provided to steps using the `data` option. This data can be in the form of key-value pairs or a freeform string.

```typescript
test('example', async (agent) => {
    await agent.act('Log in', { data: { email: "foo@bar.com", password: "foo" } });
    await agent.check('Dashboard is visible');
});

test('example', async (agent) => {
    await agent.act('Add 3 todos', { data: 'Use "Take out trash" for the first todo and make up the other 2' });
});
```

Sources: [docs/testing/building-test-cases.mdx:56-67](https://github.com/agattani123/magnitude/blob/main/docs/testing/building-test-cases.mdx:56-67)

### Custom LLM Prompting

Custom instructions can be provided to the Large Language Model (LLM) used by Magnitude for any `act` call using the `prompt` option. This can be done at the test case level, test group level, or individual step level.

```typescript
test('example', async (agent) => {
    await agent.act('create 3 todos', { prompt: 'all todos must be animal-related' });
});

test.group('todo list', { prompt: 'Each todo should be exactly 5 words'}, () => {
    test('can add todos', { url: 'https://magnitodo.com', prompt: 'All todos should be animal related' }, async (agent) => {
        await agent.act('create 3 todos', { prompt: 'the first and last word on the todo must start with the same letter'});
    });
});
```

Sources: [docs/testing/building-test-cases.mdx:70-83](https://github.com/agattani123/magnitude/blob/main/docs/testing/building-test-cases.mdx:70-83)

### Example: Migrating a Playwright Test Case

The documentation provides an example of migrating a Playwright test case to Magnitude, showcasing the difference in syntax and approach.

```typescript
// Playwright
test('should allow me to add todo items', async ({ page }) => {
    const newTodo = page.getByPlaceholder('What needs to be done?');

    await newTodo.fill(TODO_ITEMS[0]);
    await newTodo.press('Enter');

    await expect(page.getByTestId('todo-title')).toHaveText([
        TODO_ITEMS[0]
    ]);

    await newTodo.fill(TODO_ITEMS[1]);
    await newTodo.press('Enter');

    await expect(page.getByTestId('todo-title')).toHaveText([
        TODO_ITEMS[0],
        TODO_ITEMS[1]
    ]);
});

// Magnitude
test('should allow me to add todo items', async (agent) => {
    await agent.act('Create todo', { data: TODO_ITEMS[0] });
    await agent.check('First todo appears in list');
    await agent.act('Create another todo', { data: TODO_ITEMS[1] });
    await agent.check('List has two todos');
});
```

Sources: [docs/testing/building-test-cases.mdx:86-108](https://github.com/agattani123/magnitude/blob/main/docs/testing/building-test-cases.mdx:86-108)

## Test Runner Configuration

Magnitude provides a configuration file (`magnitude.config.ts`) to customize various aspects of the test runner.

### Default URL

The `url` option in the configuration file sets the default URL that all test cases will use if not specified individually.

```typescript
import { type MagnitudeConfig } from 'magnitude-test';

export default {
    url: "http://localhost:5173"
} satisfies MagnitudeConfig;
```

Sources: [docs/testing/test-configuration.mdx:7-12](https://github.com/agattani123/magnitude/blob/main/docs/testing/test-configuration.mdx:7-12)

### Browser Options

The `browser.contextOptions` option allows customizing the Playwright browser context options, such as viewport size or video recording.

```typescript
import { type MagnitudeConfig } from 'magnitude-test';

export default {
    url: "http://localhost:5173",
    browser: {
        contextOptions: {
            viewport: { width: 800, height: 600 },
            recordVideo: {
                dir: './videos/',
                size: { width: 800, height: 600 }
            }
        }
    }
} satisfies MagnitudeConfig;
```

Sources: [docs/testing/test-configuration.mdx:16-28](https://github.com/agattani123/magnitude/blob/main/docs/testing/test-configuration.mdx:16-28)

### Development Web Server

Magnitude can automatically launch a development web server when tests run. The `webServer` option configures the command to start the server, the URL it listens on, and other related settings.

```typescript
import { type MagnitudeConfig } from 'magnitude-test';

export default {
    url: 'http://localhost:3000',
    webServer: {
        command: 'npm run start',
        url: 'http://localhost:3000',
        timeout: 120_000,
        reuseExistingServer: true
    }
} satisfies MagnitudeConfig;
```

Sources: [docs/testing/test-configuration.mdx:32-43](https://github.com/agattani123/magnitude/blob/main/docs/testing/test-configuration.mdx:32-43)

### Test URL Resolution

The URL used for each test is resolved based on the following order of precedence:

1. Environment variable (`MAGNITUDE_TEST_URL`)
2. Global configuration (`magnitude.config.ts`)
3. Test group options
4. Individual test options

Relative paths provided in the `url` option are appended to the upper level's URL, while full URLs override the upper level.

Sources: [docs/testing/test-configuration.mdx:47-52](https://github.com/agattani123/magnitude/blob/main/docs/testing/test-configuration.mdx:47-52)

### Telemetry Opt-Out

By default, Magnitude collects basic anonymized telemetry during test runs. To opt-out of telemetry collection, set the `telemetry` option to `false` in the configuration file.

```typescript
import { type MagnitudeConfig } from 'magnitude-test';

export default {
    url: "http://localhost:5173",
    telemetry: false
} satisfies MagnitudeConfig;
```

Sources: [docs/testing/test-configuration.mdx:56-63](https://github.com/agattani123/magnitude/blob/main/docs/testing/test-configuration.mdx:56-63)

## Conclusion

Test Case Development in Magnitude involves creating and configuring test cases that represent user flows in a web application. Test cases are composed of steps (`act`) and assertions (`check`), with the ability to provide test data and custom prompts. The test runner can be configured to customize various aspects, such as the default URL, browser options, development web server, and telemetry opt-out. By following the provided documentation, developers can effectively write and run test cases to ensure the correct behavior of their web applications.