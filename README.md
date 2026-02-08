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


## Iterative refinement pattern

The iterative refinement pattern uses a looping mechanism to progressively improve an output over multiple cycles. The iterative refinement pattern is an implementation of the loop agent pattern.

In this pattern, one or more agents work within a loop to modify a result that's stored in the session state during each iteration. The process continues until the output meets a predefined quality threshold or it reaches a maximum number of iterations, which prevents infinite loops.

The following diagram shows a high-level view of a multi-agent iterative refinement pattern:

<img 
  src="https://docs.cloud.google.com/static/architecture/images/choose-design-pattern-agentic-ai-system-iterative-refinement.svg"
  width="500"
  height = "600"
/>

This pattern is suitable for complex generation tasks where the output is difficult to achieve in a single step. Examples of such tasks include writing and debugging a piece of code, developing a detailed multi-part plan, or drafting and revising a long-form document. For example, in a creative writing workflow, an agent might generate a draft of a blog post, critique the draft for flow and tone, and then rewrite the draft based on that critique. This process repeats in a loop until the agent's work meets a predefined quality standard or until the repetition reaches a maximum number of iterations.

The iterative refinement pattern can produce highly complex or polished outputs that would be difficult to achieve in a single step. However, the looping mechanism directly increases latency and operational costs with each cycle. This pattern also adds architectural complexity, because it requires carefully designed exit conditions—such as a quality evaluation or a maximum iteration limit—to prevent excessive costs or uncontrolled execution.

## Coordinator pattern

The multi-agent coordinator pattern uses a central agent, the coordinator, to direct a workflow. The coordinator analyzes and decomposes a user's request into sub-tasks, and then it dispatches each sub-task to a specialized agent for execution. Each specialized agent is an expert in a specific function, such as querying a database or calling an API.

A distinction of the coordinator pattern is its use of an AI model to orchestrate and dynamically route tasks. By contrast, the parallel pattern relies on a hardcoded workflow to dispatch tasks for simultaneous execution without the need for AI model orchestration.

The following diagram shows a high-level view of a multi-agent coordinator pattern:

<img 
  src="https://docs.cloud.google.com/static/architecture/images/choose-design-pattern-agentic-ai-system-coordinator.svg"
  width="500"
  height = "600"
/>

Use the coordinator pattern for automating structured business processes that require adaptive routing. For example, a customer service agent can act as the coordinator. The coordinator agent analyzes the request to determine whether it's an order status request, product return, or refund request. Based on the type of request, the coordinator routes the task to the appropriate specialized agent.

The coordinator pattern offers flexibility compared to more rigid, predefined workflows. By using a model to route tasks, the coordinator can handle a wider variety of inputs and adapt the workflow at runtime. However, this approach also introduces trade-offs. Because the coordinator and each specialized agent rely on a model for reasoning, this pattern results in more model calls than a single-agent system. Although the coordinator pattern can lead to higher-quality reasoning, it also increases token throughput, operational costs, and overall latency when compared to a single-agent system.

## Reason and act (ReAct) pattern

The ReAct pattern is an approach that uses the AI model to frame its thought processes and actions as a sequence of natural language interactions. In this pattern, the agent operates in an iterative loop of thought, action, and observation until an exit condition is met.

1 . Thought: The model reasons about the task and it decides what to do next. The model evaluates all of the information that it's gathered in order to determine whether the user's request has been fully answered.
2 . Action: Based on its thought process, the model takes one of two actions:
If the task isn't complete, it selects a tool and then it forms a query to gather more information.
If the task is complete, it formulates the final answer to send to the user, which ends the loop.
3 . Observation: The model receives the output from the tool and it saves relevant information in its memory. Because the model saves relevant output, it can build on previous observations, which helps to prevent the model from repeating itself or losing context.
The iterative loop terminates when the agent finds a conclusive answer, reaches a preset maximum number of iterations, or encounters an error that prevents it from continuing. This iterative loop lets the agent dynamically build a plan, gather evidence, and adjust its approach as it works toward a final answer.

The following diagram shows a high-level view of the ReAct pattern:

<img 
  src="https://docs.cloud.google.com/static/architecture/images/choose-design-pattern-agentic-ai-system-react.svg"
  width="500"
  height = "600"
/>

Use the ReAct pattern for complex, dynamic tasks that require continuous planning and adaptation. For example, consider a robotics agent that must generate a path to transition from an initial state to a goal state:

1 . Thought: The model reasons about the optimal path to transition from its current state to the goal state. During the thought process, the model optimizes for metrics like time or energy.
2 . Action: The model executes the next step in its plan by moving along a calculated path segment.
3 . Observation: The model observes and saves the new state of the environment. The model saves its new position and any changes to the environment that it perceives.
This loop allows the agent to adhere to dynamic constraints, such as avoiding new obstacles or following traffic regulations, by constantly updating its plan based on new observations. The agent continues through its iterative loop until it reaches its goal or encounters an error.

A single ReAct agent can be simpler and more cost-effective to implement and maintain than a complex multi-agent system. Model thinking provides a transcript of the model's reasoning, which helps with debugging. However, this flexibility introduces trade-offs. The iterative, multi-step nature of the loop can lead to higher end-to-end latency compared to a single query. Furthermore, the agent's effectiveness is highly dependent on the quality of the AI model's reasoning. Therefore, an error or a misleading result from a tool in one observation step can propagate and cause the final answer to be incorrect.

## Human-in-the-loop pattern

The human-in-the-loop pattern integrates points for human intervention directly into an agent's workflow. At a predefined checkpoint, the agent pauses its execution and calls an external system to wait for a person to review its work. This pattern lets a person approve a decision, correct an error, or provide necessary input before the agent can continue.

The following diagram shows a high-level view of a human-in-the-loop pattern:

<img 
  src="https://docs.cloud.google.com/static/architecture/images/choose-design-pattern-agentic-ai-system-human-in-the-loop.svg"
  width="500"
  height = "600"
/>

Use the human-in-the-loop pattern for tasks that require human oversight, subjective judgment, or final approval for critical actions. Such actions include approving a large financial transaction, validating the summary of a sensitive document, or providing subjective feedback on generated creative content. For example, an agent might be tasked with anonymizing a patient dataset for research. The agent would automatically identify and redact all protected health information, but it would pause at a final checkpoint. It would then wait for a human compliance officer to manually validate the dataset and approve its release, which helps to ensure that no sensitive data is exposed.

The human-in-the-loop pattern improves safety and reliability by inserting human judgment into critical decision points within the workflow. This pattern can add significant architectural complexity because it requires you to build and maintain the external system for user interaction.

















