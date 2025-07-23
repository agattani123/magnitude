<details>
<summary>Relevant source files</summary>

The following files were used as context for generating this wiki page:

- [packages/magnitude-core/src/ai/grounding.ts](https://github.com/agattani123/magnitude/blob/main/packages/magnitude-core/src/ai/grounding.ts)

</details>

# Data Extraction

## Introduction

The Data Extraction feature within the project is responsible for translating high-level web actions into precise, executable actions. It utilizes a small, fast vision agent called Moondream to locate and identify targets on web pages with pixel precision. This feature plays a crucial role in enabling the system to interact with web interfaces accurately and efficiently.

## Grounding Service

The `GroundingService` class is the core component responsible for data extraction and target localization. It serves as an interface to the Moondream client, which is a third-party vision model used for identifying and locating targets on web pages.

### Initialization

The `GroundingService` constructor initializes the service with the provided configuration options. It sets up the Moondream client with the appropriate API key and base URL. If no client configuration is provided, it defaults to using the Moondream client with the `MOONDREAM_API_KEY` environment variable.

```typescript
constructor(config: GroundingServiceConfig) {
    const clientOptions = { ...DEFAULT_CLIENT.options, ...(config.client?.options ?? {}) };
    const client = { ...DEFAULT_CLIENT, ...(config.client ?? {}), options: clientOptions };
    this.config = {...config, client: client };
    this.info = { provider: 'moondream', numCalls: 0 };
    this.logger = logger.child({ name: 'agent.grounding' });
    this.moondream = new MoondreamClient({ apiKey: this.config.client.options.apiKey, endpoint: this.config.client.options.baseUrl });
}
```

Sources: [grounding.ts:38-46]()

### Target Localization

The `locateTarget` method is responsible for locating a specific target on a web page using the Moondream client. It takes a screenshot of the web page and a target description as input, and returns the pixel coordinates of the target on the page.

```typescript
async locateTarget(screenshot: Image, target: string): Promise<PixelCoordinate> {
    return await retryOnError(
        async () => this._locateTarget(screenshot, target),
        { mode: 'retry_on_partial_message', errorSubstrings: ['429', '503', '524'], retryLimit: 20, delayMs: 1000, showWarnOnRetry: false }
    );
}
```

The `_locateTarget` method is the core implementation of the target localization process. It sends the screenshot and target description to the Moondream client's `point` API, which returns the relative coordinates of the target on the page.

```typescript
async _locateTarget(screenshot: Image, target: string): Promise<PixelCoordinate> {
    const start = Date.now();

    const response = await this.moondream.point({
        image: { imageUrl: await screenshot.toBase64() },
        object: target
    });
    this.info.numCalls++;

    // ... (handle multiple/no points returned)

    const relCoords = response.points[0];
    this.logger.trace(`locateTarget took ${Date.now()-start}ms`);

    const { width, height } = await screenshot.getDimensions();
    const pixelCoords = {
        x: relCoords.x * width,
        y: relCoords.y * height
    }

    return pixelCoords;
}
```

The method handles cases where Moondream returns multiple or no points, and converts the relative coordinates to pixel coordinates based on the dimensions of the screenshot.

Sources: [grounding.ts:49-59](), [grounding.ts:62-91]()

### Target Description Guidelines

The `moondreamTargetingInstructions` constant provides guidelines for constructing effective target descriptions for the Moondream client. It emphasizes the importance of creating a "minimal unique identifier" that uniquely identifies the target on the page, using specific text, shapes, colors, and positional information, while avoiding high-level concepts that Moondream may not understand.

```typescript
export const moondreamTargetingInstructions = `
Targets descriptions must be carefully chosen to be accurately picked up by Moondream, a small vision model.
Build a "minimal unique identifier" - a description that is as brief as possible that uniquely identifies the target on the page.
Use only the information needed, and prioritize in this order:
- specific text
- specific shapes and colors
- positional information
- high level information (Moondream cannot always understand high level concepts)
`;
```

Sources: [grounding.ts:25-33]()

## Error Handling and Retries

The `locateTarget` method utilizes the `retryOnError` utility function to handle potential errors and retry the target localization process. It retries the operation up to 20 times with a 1-second delay if specific error messages related to rate limiting (429), service unavailability (503), or timeouts (524) are encountered.

```typescript
async locateTarget(screenshot: Image, target: string): Promise<PixelCoordinate> {
    return await retryOnError(
        async () => this._locateTarget(screenshot, target),
        { mode: 'retry_on_partial_message', errorSubstrings: ['429', '503', '524'], retryLimit: 20, delayMs: 1000, showWarnOnRetry: false }
    );
}
```

Sources: [grounding.ts:49-54]()

## Conclusion

The Data Extraction feature, implemented through the `GroundingService` class, plays a crucial role in enabling the system to interact with web interfaces accurately and efficiently. By leveraging the Moondream vision model, it can locate and identify targets on web pages with pixel precision, translating high-level web actions into executable actions. The service provides error handling and retries, as well as guidelines for constructing effective target descriptions to ensure optimal performance.