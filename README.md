## Single-agent system
A single-agent system uses an AI model, a defined set of tools, and a comprehensive system prompt to autonomously handle a user request or to complete a specific task. In this fundamental pattern, the agent relies on the model's reasoning capabilities to interpret a user's request, plan a sequence of steps, and decide which tools to use from a defined set. The system prompt shapes the agent's behavior by defining its core task, persona, and operations, and the specific conditions for using each tool.

The following diagram shows a high-level view of a single agent pattern:

<img src= https://docs.cloud.google.com/static/architecture/images/choose-design-pattern-agentic-ai-system-single-agent.svg> 

A single-agent system is ideal for tasks that require multiple steps and access to external data. For example, a customer support agent must query a database to find an order status, or a research assistant needs to call APIs to summarize recent news. A non-agentic system can't perform these tasks because it can't autonomously use tools or execute a multi-step plan to synthesize a final answer.

If you're early in your agent development, we recommend that you start with a single agent. When you start your agent development with a single-agent system, you can focus on refining the core logic, prompt, and tool definitions of your agent before adding more complex architectural components.

A single agent's performance can be less effective when it uses more tools and when tasks increase in complexity. You might observe this as increased latency, incorrect tool selection or use, or a failure to complete the task. You can often mitigate these issues by refining the agent's reasoning process with techniques like the Reason and Act (ReAct) pattern. However, if your workflow requires an agent to manage several distinct responsibilities, these techniques might not be sufficient. For these cases, consider a multi-agent system, which can improve resilience and performance by delegating specific skills to specialized agents.

## Multi Agent Systems

A multi-agent system orchestrates multiple specialized agents to solve a complex problem that a single agent can't easily manage. The core principle is to decompose a large objective into smaller sub-tasks and assign each sub-task to a dedicated agent with a specific skill. These agents then interact through collaborative or hierarchical workflows to achieve the final goal. Multi-agent patterns provide a modular design that can improve the scalability, reliability, and maintainability of the overall system compared to a single agent with a monolithic prompt.

In a multi-agent system, each agent requires a specific context to perform its task effectively. Context can include documentation, historical preferences, relevant links, conversational history, or any operational constraints. The process of managing this information flow is called context engineering. Context engineering includes strategies such as isolating context for a specific agent, persisting information across multiple steps, or compressing large amounts of data to improve efficiency.

Building a multi-agent system requires additional evaluation, security, reliability, and cost considerations when compared to a single-agent system. For example, multi-agent systems must implement precise access controls for each specialized agent, design a robust orchestration system to ensure reliable inter-agent communication, and manage the increased operational costs from the computational overhead of running multiple agents

## Sequential pattern
The multi-agent sequential pattern executes a series of specialized agents in a predefined, linear order where the output from one agent serves as the direct input for the next agent. This pattern uses a sequential workflow agent that operates on predefined logic without having to consult an AI model for the orchestration of its subagents.

The following diagram shows a high-level view of a multi-agent sequential pattern:

<img 
  src="https://docs.cloud.google.com/static/architecture/images/choose-design-pattern-agentic-ai-system-sequential.svg"
  width="900"
  height = "350"
/>

Use the sequential pattern for highly structured, repeatable processes where the sequence of operations doesn't change. For example, a data processing pipeline might use this pattern to first have a data extraction agent pull raw data, then pass that data to a data cleaning agent for formatting, which in turn passes the clean data to a data loading agent to save it in a database.

The sequential pattern can reduce latency and operational costs compared to a pattern that uses an AI model to orchestrate task workflow. However, this efficiency comes at the cost of flexibility. The rigid, predefined structure of the pipeline makes it difficult to adapt to dynamic conditions or to skip unnecessary steps, which can cause inefficient processing or lead to higher cumulative latency if an unneeded step is slow.

## Parallel pattern
The multi-agent parallel pattern, also known as a concurrent pattern, multiple specialized subagents perform a task or sub-tasks independently at the same time. The outputs of the subagents are then synthesized to produce the final consolidated response. Similar to a sequential pattern, the parallel pattern uses a parallel workflow agent to manage how and when the other agents run without having to consult an AI model to orchestrate its subagents.

The following diagram shows a high-level view of a multi-agent parallel pattern:

<img 
  src="https://docs.cloud.google.com/static/architecture/images/choose-design-pattern-agentic-ai-system-parallel.svg"
  width="600"
  height = "900"
/>

Use the parallel pattern when sub-tasks can be executed concurrently to reduce latency or gather diverse perspectives, such as gathering data from disparate sources or evaluating several options at once. For example, to analyze customer feedback, a parallel agent might fan out a single feedback entry to four specialized agents at the same time: a sentiment analysis agent, a keyword extraction agent, a categorization agent, and an urgency detection agent. A final agent gathers these four outputs into a single, comprehensive analysis of that feedback.

The parallel pattern can reduce overall latency compared to a sequential approach because it can gather diverse information from multiple sources at the same time. However, this approach introduces trade-offs in cost and complexity. Running multiple agents in parallel can increase immediate resource utilization and token consumption, which leads to higher operational costs. Furthermore, the gather step requires complex logic to synthesize potentially conflicting results, which adds to the development and maintenance overhead of the system.
## Loop pattern

The multi-agent loop agent pattern repeatedly executes a sequence of specialized subagents until a specific termination condition is met. This pattern uses a loop workflow agent that, like other workflow agents, operates on predefined logic without consulting an AI model for orchestration. After all of the subagents complete their tasks, the loop agent evaluates whether an exit condition is met. The condition can be a maximum number of iterations or a custom state. If the exit condition isn't met, then the loop agent starts the sequence of subagents again. You can implement a loop pattern where the exit condition is evaluated at any point in the flow. Use the loop pattern for tasks that require iterative refinement or self-correction, such as generating content and having a critic agent review it until it meets a quality standard.

The following diagram shows a high-level view of a multi-agent loop pattern:

<img 
  src="https://docs.cloud.google.com/static/architecture/images/choose-design-pattern-agentic-ai-system-loop.svg"
  width="500"
  height = "600"
/>

The loop agent pattern provides a way to build complex, iterative workflows. It enables agents to refine their own work and continue processing until a specific quality or state is achieved. However, this pattern's primary trade-off is the risk of an infinite loop. If the termination condition isn't correctly defined or if the subagents fail to produce the state that's required to stop, the loop can run indefinitely. This can lead to excessive operational costs, high resource consumption, and potential system hangs.

## Review and critique pattern

The multi-agent review and critique pattern, also known as the generator and critic pattern, improves the quality and reliability of generated content by using two specialized agents, typically in a sequential workflow. The review and critique pattern is an implementation of the loop agent pattern.

In the review and critique pattern, a generator agent creates an initial output, such as a block of code or a summary of a document. Next, a critic agent evaluates this output against a predefined set of criteria, such as factual accuracy, adherence to formatting rules, or safety guidelines. Based on the evaluation, the critic can approve the content, reject it, or return it to the generator with feedback for revision.

The following diagram shows a high-level view of a multi-agent review and critique pattern:

<img 
  src="https://docs.cloud.google.com/static/architecture/images/choose-design-pattern-agentic-ai-system-review-critique.svg"
  width="500"
  height = "600"
/>










