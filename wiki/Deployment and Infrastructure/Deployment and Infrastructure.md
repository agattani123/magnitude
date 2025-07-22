<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [docs/customizing/self-hosting-moondream.mdx](https://github.com/agattani123/magnitude/blob/main/docs/customizing/self-hosting-moondream.mdx)
</details>

# Deployment and Infrastructure

## Introduction

This page covers the deployment and infrastructure options for running the Moondream server, a crucial component of the Magnitude testing framework. Moondream is a powerful AI model that powers Magnitude's visual testing capabilities. The page outlines three main approaches to hosting Moondream: running it locally, self-hosting on a private infrastructure or VPC, and self-hosting on the Modal cloud platform. Each approach is tailored to different use cases and requirements, such as local development, security and compliance needs, or rapid deployment with GPU acceleration.

## Running Moondream Locally

Running Moondream locally involves downloading the server executable from the official website and running it on the same machine where Magnitude tests are executed. This approach is suitable for local development on high-end machines with sufficient computational resources.

### Local Setup

1. Download the appropriate Moondream server executable for your operating system (macOS or Linux) from https://moondream.ai/c/moondream-server.
2. Follow the instructions provided on the website to run the downloaded executable.
3. Configure the `magnitude.config.ts` file with the local base URL:

```ts
import { type MagnitudeConfig } from 'magnitude-test';

export default {
    url: "http://localhost:5173",
    executor: {
        provider: 'moondream',
        options: {
            baseUrl: 'http://localhost:2020/v1'
        }
    }
} satisfies MagnitudeConfig;
```

With this configuration, Magnitude tests can be run using the locally running Moondream server.

> **Warning:** Local inference on CPU is very slow. Newer macOS machines or a GPU is recommended for this approach. Otherwise, consider self-hosting on Modal for better performance.

Sources: [docs/customizing/self-hosting-moondream.mdx:6-21]()

## Self-Hosting on a VPC

For companies with strict security and compliance requirements, Moondream can be self-hosted within the company's Virtual Private Cloud (VPC) on a cloud provider of choice. This approach ensures that the infrastructure remains within the company's controlled environment.

### VPC Hosting Requirements

1. A compute instance within the VPC with a GPU.
2. A Linux distribution with CUDA configured on the compute instance.
3. Running the Moondream server executable on the compute instance.
4. Ensuring the instance is accessible via port 2020 to CI runners or development machines.
5. Configuring test runners to connect to the appropriate URL:

```ts
import { type MagnitudeConfig } from 'magnitude-test';

export default {
    url: "http://localhost:5173",
    executor: {
        provider: 'moondream',
        options: {
            baseUrl: 'https://some-host-in-my-vpc/v1'
        }
    }
} satisfies MagnitudeConfig;
```

For enterprise-level configuration assistance, reach out to the Magnitude team at founders@magnitude.run.

Sources: [docs/customizing/self-hosting-moondream.mdx:24-41]()

## Self-Hosting on Modal

Modal is a cloud platform that provides an easy way to self-host Moondream with GPU acceleration. Magnitude provides a deployment script to automate the process of deploying Moondream on Modal.

### Setting up Modal

1. Create an account at [modal.com](https://modal.com).
2. Install the Modal Python package: `pip install modal`.
3. Authenticate with Modal: `modal setup` (or `python -m modal setup`).

> **Tip:** Modal offers $30 in free credits per month.

Sources: [docs/customizing/self-hosting-moondream.mdx:45-51]()

### Deploying Moondream on Modal

1. Clone the Magnitude repository: `git clone https://github.com/magnitudedev/magnitude.git`.
2. Navigate to the `infra` directory: `cd magnitude/infra`.
3. Deploy the Moondream server using the provided script: `modal deploy moondream.py`.

This script downloads the Moondream server and model weights to a Modal volume and deploys the endpoint. The Moondream endpoint will be available at `https://<your-modal-username>--moondream.modal.run/v1`.

4. Configure the `magnitude.config.ts` file with the Modal base URL:

```ts
import { type MagnitudeConfig } from 'magnitude-test';

export default {
    url: "http://localhost:5173",
    executor: {
        provider: 'moondream',
        options: {
            baseUrl: 'https://<your-modal-username>--moondream.modal.run/v1'
        }
    }
} satisfies MagnitudeConfig;
```

> **Warning:** The Modal server will be publicly accessible. Magnitude is working on a better way to make it configurable with a custom API key.

Sources: [docs/customizing/self-hosting-moondream.mdx:53-73]()

### Customizing Modal Deployment

The `moondream.py` deployment script can be modified to customize the deployment based on specific needs. Common options to consider:

- `gpu`: GPU configuration (see Modal's [pricing page](https://modal.com/pricing) for details on available GPUs and their cost).
- `scaledown_window`: Time in seconds a container will wait to shut down after receiving no requests. Higher values allow running tests after longer periods without needing a container cold-start.
- `min_containers`: Minimum number of containers to keep open to handle requests. This eliminates cold-starts but incurs continuous billing for the containers.

Sources: [docs/customizing/self-hosting-moondream.mdx:76-83]()

### GPU Comparison

The following table compares the approximate inference time per action and the Modal cost per hour for different GPU configurations available on Modal:

| GPU         | Approximate inference time per action | Modal cost per hour |
| ----------- | ------------------------------------------------ | ------------------- |
| H100        | ~200ms                                           | $3.95               |
| A100 (40GB) | ~300ms                                           | $2.10               |
| A10G        | ~500ms                                           | $1.10               |
| T4          | ~800ms                                           | $0.59               |

Since Magnitude needs to wait for the page to stabilize, the A10G GPU may provide a good balance between performance and cost, but any of these options can work well.

Sources: [docs/customizing/self-hosting-moondream.mdx:86-94]()

## Conclusion

This wiki page covered the various deployment and infrastructure options for running the Moondream server, a crucial component of the Magnitude testing framework. The options include running Moondream locally for development purposes, self-hosting on a private VPC for security and compliance requirements, and self-hosting on the Modal cloud platform for rapid deployment with GPU acceleration. Each approach is tailored to different use cases and requirements, providing flexibility in how Moondream is integrated into the Magnitude testing workflow.