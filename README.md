---
title: AI Comic Factory
emoji: 👩‍🎨
colorFrom: red
colorTo: yellow
sdk: docker
pinned: true
app_port: 3000
---

# AI Comic Factory

## Running the project at home

First, I would like to highlight that everything is open-source (see [here](https://huggingface.co/spaces/jbilcke-hf/ai-comic-factory/tree/main), [here](https://huggingface.co/spaces/jbilcke-hf/VideoChain-API/tree/main), [here](https://huggingface.co/spaces/hysts/SD-XL/tree/main), [here](https://github.com/huggingface/text-generation-inference)).

However the project isn't a monolithic Space that can be duplicated and ran immediately:
it requires various components to run for the frontend, backend, LLM, SDXL etc.

If you try to duplicate the project, you will see it requires some variables:

- `LLM_ENGINE`: can be either "INFERENCE_API" or "INFERENCE_ENDPOINT"
- `HF_API_TOKEN`: necessary if you decide to use an inference api model or a custom inference endpoint
- `HF_INFERENCE_ENDPOINT_URL`: necessary if you decide to use a custom inference endpoint
- `RENDERING_ENGINE`: can only be "VIDEOCHAIN" for now, unless you code your custom solution
- `VIDEOCHAIN_API_URL`: url to the VideoChain API server
- `VIDEOCHAIN_API_TOKEN`: secret token to access the VideoChain API server
 
Please read the `.env` default config file for more informations.
To customise a variable locally, you should create a `.env.local`
(do not commit this file as it will contain your secrets).

-> If you intend to run it with local, cloud-hosted and/or proprietary models **you are going to need to code 👨‍💻**.

## The LLM API (Large Language Model)

Currently the AI Comic Factory uses [Llama-2 70b](https://huggingface.co/blog/llama2) through an [Inference Endpoint](https://huggingface.co/docs/inference-endpoints/index).

You have three options:

### Option 1: Use an Inference API model

This is a new option added recently, where you can use one of the models from the Hugging Face Hub. By default we suggest to use CodeLlama 34b as it will provide better results than the 7b model.

To activate it, create a `.env.local` configuration file:

```bash
LLM_ENGINE="INFERENCE_API"

HF_API_TOKEN="Your Hugging Face token"

# codellama/CodeLlama-7b-hf" is used by default, but you can change this
# note: You should use a model able to generate JSON responses,
# so it is storngly suggested to use at least the 34b model
HF_INFERENCE_API_MODEL="codellama/CodeLlama-7b-hf"
```

### Option 2: Use an Inference Endpoint URL

If you would like to run the AI Comic Factory on a private LLM running on the Hugging Face Inference Endpoint service, create a `.env.local` configuration file:

```bash
LLM_ENGINE="INFERENCE_ENDPOINT"

HF_API_TOKEN="Your Hugging Face token"

HF_INFERENCE_ENDPOINT_URL="path to your inference endpoint url"
```

To run this kind of LLM locally, you can use [TGI](https://github.com/huggingface/text-generation-inference) (Please read [this post](https://github.com/huggingface/text-generation-inference/issues/726) for more information about the licensing).

### Option 3: Fork and modify the code to use a different LLM system

Another option could be to disable the LLM completely and replace it with another LLM protocol and/or provider (eg. OpenAI, Replicate), or a human-generated story instead (by returning mock or static data).


### Notes

It is possible that I modify the AI Comic Factory to make it easier in the future (eg. add support for OpenAI or Replicate)

## The Rendering API

This API is used to generate the panel images. This is an API I created for my various projects at Hugging Face.

I haven't written documentation for it yet, but basically it is "just a wrapper ™" around other existing APIs:

- The [hysts/SD-XL](https://huggingface.co/spaces/hysts/SD-XL?duplicate=true) Space by [@hysts](https://huggingface.co/hysts)
- And other APIs for making videos, adding audio etc.. but you won't need them for the AI Comic Factory

### Option 1: Deploy VideoChain yourself

You will have to [clone](https://huggingface.co/spaces/jbilcke-hf/VideoChain-API?duplicate=true) the [source-code](https://huggingface.co/spaces/jbilcke-hf/VideoChain-API/tree/main)

Unfortunately, I haven't had the time to write the documentation for VideoChain yet.
(When I do I will update this document to point to the VideoChain's README)

### Option 2: Use another SDXL API

If you fork the project you will be able to modify the code to use the Stable Diffusion technology of your choice (local, open-source, your custom HF Space etc)

### Notes

It is possible that I modify the AI Comic Factory to make it easier in the future (eg. add support for Replicate)
