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
<img src="https://docs.cloud.google.com/static/architecture/images/choose-design-pattern-agentic-ai-system-sequential.svg" width="900"
  height="500>
