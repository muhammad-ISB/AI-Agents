Agents -> Agentic Automations

Role playing, focus, tools, interaction, cooperation, guardrails, memory

Fuzzy inputs -> transfromations -> Outputs then Notes -> fuzzy all as we dont know the type of input/transformation/output

Crew of AI agents -> do research / do data collection, comparision, scoring, talking points

What are agents?

Task -> Agent -> ANswer
Task -> Agent(tools like gathering data, summurizing etc) -> ANswer

Multi agent -> multiple agents -> tast with task _.-> Agents(1.Researcher(llama 3) 2. Writer(GPT 4)) -> Answer

CREW AI -> platform build to bring into production -> framework and platform
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
from crewai import Agent, Task, Crew

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Creating Agents
Define your Agents, and provide them a role, goal and backstory. additionally ->  allow_delegation, verbose
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Task: -> takes atleast 3 variables -> description, expected_output and agent
)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Creating the Crew
Create your crew of Agents
Pass the tasks to be performed by those agents.
Note: For this simple example, the tasks will be performed sequentially (i.e they are dependent on each other), so the order of the task in the list matters.
verbose=2 allows you to see all the logs of the execution.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
crew = Crew(
    agents=[planner, writer, editor],
    tasks=[plan, write, edit],
    verbose=2
)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
result = crew.kickoff(inputs={"topic": "Artificial Intelligence"})

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The Agent class is the cornerstone for implementing AI solutions in CrewAI. Here's an updated overview reflecting the latest codebase changes:

Attributes:
role: Defines the agent's role within the solution.
goal: Specifies the agent's objective.
backstory: Provides a background story to the agent.
llm: Indicates the Large Language Model the agent uses. By default, it uses the GPT-4 model defined in the environment variable "OPENAI_MODEL_NAME".
function_calling_llm Optional: Will turn the ReAct crewAI agent into a function calling agent.
max_iter: Maximum number of iterations for an agent to execute a task, default is 15.
memory: Enables the agent to retain information during and a across executions. Default is False.
max_rpm: Maximum number of requests per minute the agent's execution should respect. Optional.
verbose: Enables detailed logging of the agent's execution. Default is False.
allow_delegation: Allows the agent to delegate tasks to other agents, default is True.
tools: Specifies the tools available to the agent for task execution. Optional.
step_callback: Provides a callback function to be executed after each step. Optional.
cache: Determines whether the agent should use a cache for tool usage. Default is True.
