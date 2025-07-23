<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [docs/customizing/self-hosting-moondream.mdx](https://github.com/agattani123/magnitude/blob/main/docs/customizing/self-hosting-moondream.mdx)
</details>

# Deployment and Infrastructure

## Introduction

This wiki page covers the deployment and infrastructure options for running the Moondream server, which is a crucial component of the Magnitude project. Moondream is a powerful AI model used for generating and understanding visual representations of web applications, enabling efficient testing and automation. The page provides an overview of different deployment strategies, including running Moondream locally, self-hosting on a private infrastructure or Virtual Private Cloud (VPC), and leveraging cloud platforms like Modal for GPU-accelerated deployments.

## Local Deployment

Running Moondream locally on the same machine where Magnitude tests are executed can be a convenient option for local development, especially on high-end machines. To set up a local Moondream server, follow these steps:

1. Download the appropriate Moondream server executable for your operating system (macOS or Linux) from the official website: https://moondream.ai/c/moondream-server
2. Run the downloaded executable, following the provided instructions.
3. Configure the `magnitude.config.ts` file with the local base URL, as shown in the example below:

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

It's important to note that local inference on a CPU can be slow, and newer macOS machines or a GPU are recommended for better performance. If performance is a concern, consider deploying Moondream on a cloud platform with GPU support, such as Modal.

Sources: [docs/customizing/self-hosting-moondream.mdx:11-24]()

## Self-hosting on a VPC

If your company has strict requirements regarding infrastructure hosting, you may need to self-host Moondream within your company's Virtual Private Cloud (VPC) on your preferred cloud provider. The specific setup will depend on the cloud provider, but generally, the following requirements must be met:

1. A compute instance with a GPU within your VPC.
2. A Linux distribution with CUDA configured on the compute instance.
3. Running the Moondream server executable on the instance.
4. Ensuring that the instance is accessible via port 2020 to your CI runners or development machines.
5. Configuring test runners to connect to the appropriate URL, as shown in the example below:

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

If you are an enterprise trying to configure Magnitude in your company, you can reach out to the Magnitude team directly at founders@magnitude.run for assistance.

Sources: [docs/customizing/self-hosting-moondream.mdx:31-45]()

## Self-hosting on Modal

One of the easiest ways to deploy Moondream is by hosting it on Modal, a cloud platform that provides GPU-accelerated compute resources. The Magnitude project provides a deployment script (`infra/moondream.py`) to automate the process of deploying Moondream on Modal.

### Setting up Modal

1. Create an account at [modal.com](https://modal.com).
2. Install the Modal Python package by running `pip install modal`.
3. Authenticate with Modal by running `modal setup` (or `python -m modal setup` if the previous command doesn't work).

For more information, refer to the Modal documentation: https://modal.com/docs/guide

### Deploying Moondream

1. Clone the Magnitude repository: `git clone https://github.com/magnitudedev/magnitude.git`
2. Navigate to the `infra` directory: `cd magnitude/infra`
3. Deploy the Moondream server using the provided script: `modal deploy moondream.py`

This script will automatically download the Moondream server and model weights to a Modal volume and deploy the endpoint. Your Moondream endpoint will be available at `https://<your-modal-username>--moondream.modal.run/v1`.

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

Note that there may be some cold start times since Modal automatically scales down containers when not in use.

Sources: [docs/customizing/self-hosting-moondream.mdx:51-74]()

### Customizing the Modal Deployment

You can customize the Modal deployment by modifying the `moondream.py` deployment script according to your needs. Some common options you may want to change include:

- `gpu`: GPU configuration. See Modal's [pricing page](https://modal.com/pricing) for details on different available GPUs and their costs.
- `scaledown_window`: Time in seconds a container will wait to shut down after receiving no requests. A higher value will allow you to run tests after longer periods without needing the container to cold-start again.
- `min_containers`: By setting this option, you force Modal to keep a minimum number of containers open to handle requests. This eliminates cold-starts but also means you will be billed for those containers all the time.

Sources: [docs/customizing/self-hosting-moondream.mdx:78-84]()

### GPU Comparison

The following table provides a breakdown of how quickly different GPUs available on Modal can handle typical requests made by Magnitude to Moondream, along with their approximate inference times and Modal's cost per hour:

| GPU         | Approximate inference time per action | Modal cost per hour |
| ----------- | ------------------------------------------------ | ------------------- |
| H100        | ~200ms                                           | $3.95               |
| A100 (40GB) | ~300ms                                           | $2.10               |
| A10G        | ~500ms                                           | $1.10               |
| T4          | ~800ms                                           | $0.59               |

Since Magnitude needs to wait for the page to stabilize, an option like the A10G may provide a good balance between price and performance, but any of these GPUs should work well.

Sources: [docs/customizing/self-hosting-moondream.mdx:87-95]()

## Conclusion

This wiki page covered the various deployment and infrastructure options for running the Moondream server within the Magnitude project. Whether you choose to run Moondream locally, self-host it on your private infrastructure or VPC, or leverage cloud platforms like Modal for GPU-accelerated deployments, the provided information and configuration examples should help you set up and configure Moondream according to your specific needs and requirements.