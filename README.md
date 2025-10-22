# Building Towards Computer Use with Anthropic

## Introduction

This repository contains my personal notes, code, and projects from the "Building Towards Computer Use with Anthropic" course. This course is offered by **[DeepLearning.AI](https://www.deeplearning.ai/)** in collaboration with **[Anthropic](https://www.anthropic.com/)**.

"Computer Use" utilizes the capabilities of the latest models including image reasoning and tool use to enable an LLM-based agent to use a computer. Like a human user, the model processes an image of the screen, analyzes it to understand what’s going on, and navigates the computer by issuing mouse clicks and generating keyboard strokes to get things done.

In this course, you’ll learn the features that lead up to computer use from working with the Anthropic’s API, to multimodal prompting, prompt caching, and tool use, ending in a demo that combines these features to build an AI assistant that uses a computer.

In detail, you’ll:

  * Learn Anthropic’s approach to AI research, principles of AI safety, alignment, and interpretability while understanding the key differences between its models.
  * Make API requests to Claude, format messages for better responses, and control API parameters like system prompts, temperature, and $max\_tokens$ for optimal responses.
  * Write multi-modal prompts that combine text and image content blocks and build with streaming responses.
  * Learn effective prompting techniques such as using prompt templates, structuring prompts in XML, and providing examples to get consistent high-quality responses.
  * Learn to implement prompt caching and see how it can reduce costs and latency.
  * Understand tool-use workflows and build a chatbot that can call different tools in response to users’ queries.
  * See all these concepts come together in a demo that uses Anthropic Computer Use to achieve a task on a computer.

Start utilizing Anthropic’s family of models to build towards Computer Use applications.

-----

## Course Topics

This section contains a detailed breakdown of each module covered in the course.

<details>
<summary><strong>Module 1: Overview</strong></summary>

This foundational module introduces Anthropic's mission, research philosophy, and its family of large language models. It sets the context for *why* the tools are built the way they are, with a strong emphasis on safety and reliability.

  * **Anthropic's Mission:** The course begins by exploring Anthropic's "safety-first" approach to AI development. This includes an introduction to their research on AI safety, alignment, and interpretability, which is the bedrock of all their models.
  * **Constitutional AI:** You learn about the core concept of Constitutional AI (CAI), a method for training a harmless AI assistant without relying on large-scale human feedback for safety labeling. Instead, the model is trained to follow a set of explicit principles (a "constitution"), allowing it to self-correct and refine its behavior.
  * **Model Family:** The module details the Claude 3 model family:
      * **Opus:** The most powerful, "state-of-the-art" model, designed for highly complex tasks, in-depth analysis, and high-stakes problem-solving.
      * **Sonnet:** The "workhorse" model, offering an ideal balance of high performance and high speed. It's designed for most enterprise workloads, data processing, and scalable AI applications.
      * **Haiku:** The fastest and most compact model, designed for near-instant responsiveness, simple queries, and cost-effective applications.
  * **The Goal (Computer Use):** Finally, this overview connects these concepts to the course's ultimate goal: "Computer Use." It frames this capability as the next logical step for AI, moving from a text-based conversationalist to a system that can perceive, reason, and act within a digital environment.

</details>

<details>
<summary><strong>Module 2: Working with the API</strong></summary>

This module is a practical, hands-on guide to making your first calls to the Anthropic API. It covers the fundamental building blocks of interacting with Claude.

  * **Authentication & Setup:** You learn how to set up your development environment, install the official Python client library (`anthropic`), and securely manage your API keys.
  * **The Messages API:** The core of this module is the `messages.create` endpoint. You learn the new standard structure for conversations, which consists of a list of `message` objects, each with a `role` (`user` or `assistant`) and `content`. This is a more robust and natural way to handle multi-turn conversations compared to older prompt-based APIs.
  * **System Prompts:** You learn how to use the `system` parameter to provide high-level instructions, context, or persona-setting for the AI *before* the conversation starts. This is the most effective way to guide the model's behavior, tone, and constraints throughout the interaction.
  * **Key Parameters:** You get to experiment with the primary controls for model output:
      * `temperature`: Controls randomness. A low value (e.g., 0.2) is good for deterministic, factual answers, while a high value (e.g., 1.0) encourages creativity.
      * `max_tokens`: Sets a hard limit on the number of tokens in the generated response, which is crucial for managing costs and response length.
  * **Streaming Responses:** You implement streaming API calls. Instead of waiting for the entire response to be generated, you learn to process it token-by-token as it arrives. This is essential for creating a responsive, real-time "chatbot" feel for end-users.

</details>

<details>
<summary><strong>Module 3: Multimodal Requests</strong></summary>

This module explores Claude's "vision" capabilities, allowing it to understand and reason about images. This is a critical prerequisite for the "Computer Use" agent, which needs to "see" the screen.

  * **Content Blocks:** You learn how to structure the `content` in your API call as a list of "content blocks." This is how you combine different types of media. For example, a single `user` message can contain a `text` block and one or more `image` blocks.
  * **Image Formats:** The course covers the practical aspects of providing images, primarily by base64-encoding them and specifying the media type (e.g., `image/jpeg`, `image/png`).
  * **Prompting with Images:** You practice writing prompts that effectively leverage this new input. This includes tasks like:
      * **Image Description:** "Describe what is happening in this image."
      * **Data Extraction:** "Analyze this chart and summarize the key trends."
      * **Transcription:** "Transcribe the handwritten text in this note."
      * **UI Analysis:** "Given this screenshot of a webpage, what is the main call to action?" (This directly leads into the Computer Use case).
  * You learn the limitations and best practices, such as how to prompt the model to focus on specific parts of an image or to handle multiple images at once.

\</details\>

\<details\>
\<summary\>\<strong\>Module 4: Real World Prompting\</strong\>\</summary\>

This module goes deep into prompt engineering techniques specifically tailored for getting reliable, structured, and high-quality responses from Claude.

  * **Using XML Tags:** This is a key technique emphasized by Anthropic. You learn to structure your prompts using XML-like tags (e.g., `<document>`, `<instructions>`, `<example>`). This helps the model clearly distinguish between different parts of your prompt (like raw data vs. instructions on how to process that data), which dramatically improves accuracy, especially for complex inputs.
  * **Few-Shot Prompting:** You learn how to provide examples of "good" responses directly within your prompt. By showing the model a few input/output pairs, you can guide it to follow a specific format, style, or logic without needing to fine-tune the model itself.
  * **Prompt Templates:** You build reusable prompt templates (e.g., using Python's f-strings) that dynamically insert user-provided data into a well-structured prompt. This is the standard for building scalable applications that need to perform the same task repeatedly.
  * **Forcing Structured Output:** A key lesson is prompting the model to *only* respond in a specific format, such as JSON. You practice prompts like, "Respond *only* with a valid JSON object matching the following schema..." This is critical for building reliable application backends.

</details>

<details>
<summary><strong>Module 5: Prompt Caching</strong></summary>

This module introduces a powerful optimization feature for reducing both cost and latency in your applications.

  * **The Problem:** Many applications, especially conversational ones, use a very long and static `system` prompt or a large set of few-shot examples that are sent with *every single* API request. This is computationally wasteful, as the model has to re-process the same tokens over and over.
  * **The Solution:** Prompt caching allows the Anthropic API to cache the processed state of this large, static "prefix" of your prompt.
  * **Implementation:** You learn how to use the `Cache-Control` API headers to enable this feature. When you make a call with `Cache-Control: max-age=...`, the API caches the tokens in the `system` prompt and any initial `user`/`assistant` messages.
  * **Benefits:** On subsequent calls that use the *exact same* prefix, the API skips re-processing the cached tokens. You learn that this results in a significant reduction in "input tokens" billed and a much faster time-to-first-token (lower latency), as the model can start generating the new response almost immediately.

</details>

<details>
<summary><strong>Module 6: Tool Use</strong></summary>

This is where the model moves from a passive text generator to an active agent that can interact with the outside world. This module teaches you how to implement function calling with Claude.

  * **The Workflow:** You learn the complete, multi-step "tool use" loop:
    1.  **Define Tools:** You define a list of available tools (e.g., `get_current_weather`, `search_database`) using a specific JSON schema, and pass this definition to the API.
    2.  **Model Decides:** The user sends a prompt (e.g., "What's the weather in London?"). The model analyzes the prompt and, instead of answering, returns a special `tool_use` content block, indicating which tool it wants to call and with what arguments (e.g., `{"name": "get_current_weather", "input": {"location": "London"}}`).
    3.  **Application Executes:** Your own code (in Python, etc.) receives this request. You parse it, execute your *actual* `get_current_weather("London")` function, and get the result (e.g., `{"temperature": "15C"}`).
    4.  **Send Result Back:** You make a *second* API call, appending a new `user` message that contains a `tool_result` block with the output from your function.
    5.  **Model Synthesizes:** The model receives this new information and generates a final, natural-language response for the user (e.g., "The current weather in London is 15°C.").
  * You build a complete chatbot that can answer questions by intelligently deciding when to call external APIs for information.

</details>

<details>
<summary><strong>Module 7: Computer Use</strong></summary>

This is the capstone module where all previous concepts are combined to build an AI agent that can operate a computer.

  * **The Goal:** The objective is to give the model a high-level task (e.g., "Find the most recent email from my manager and summarize it.") and have it perform the necessary mouse clicks and keyboard actions to accomplish it.
  * **The "Vision-Action" Loop:** This is the core concept. The agent runs in a loop that combines all the course topics:
    1.  **See (Multimodal):** The agent takes a screenshot of the current computer screen. This image is passed to the model as `image` content.
    2.  **Think (Prompting):** The model receives the image along with a complex `system` prompt that contains the high-level goal, its "memory" of past actions, and its available "tools." The model analyzes the screen and decides on the *single next best action* to take.
    3.  **Act (Tool Use):** The model's "tools" are not web APIs, but functions that control the computer, such as:
          * `click(x, y)`
          * `double_click(x, y)`
          * `type_text("some string")`
          * `press_key("ENTER")`
          * `scroll(direction)`
          * `finish(result)`
            The model returns a `tool_use` request for one of these functions (e.g., `{"name": "click", "input": {"x": 450, "y": 230}}`).
  * **Execution:** Your code executes this command, (e.g., actually moving the mouse and clicking). The loop then repeats: a *new* screenshot is taken, and the model decides on the next action based on the new visual state.
  * The demo showcases this entire system, allowing Claude to navigate applications and websites to complete a task, effectively "using" the computer just as a human would.

</details>

-----

## Acknowledgements

This repository is for my personal learning purposes only. The course **[Building Towards Computer Use with Anthropic](https://www.deeplearning.ai/short-courses/building-towards-computer-use-with-anthropic/)** is a collaboration between **[DeepLearning.AI](https://www.deeplearning.ai/)** and **[Anthropic](https://www.anthropic.com/)**.

All course materials, content, and licenses are held by DeepLearning.AI and Anthropic. Please enroll in the official course to access the complete and authoritative content.