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
planner = Agent(
    role="Content Planner",
    goal="Plan engaging and factually accurate content on {topic}",
    backstory="You're working on planning a blog article "
              "about the topic: {topic}."
              "You collect information that helps the "
              "audience learn something "
              "and make informed decisions. "
              "Your work is the basis for "
              "the Content Writer to write an article on this topic.",
    allow_delegation=False,
	verbose=True
)
writer = Agent(
    role="Content Writer",
    goal="Write insightful and factually accurate "
         "opinion piece about the topic: {topic}",
    backstory="You're working on a writing "
              "a new opinion piece about the topic: {topic}. "
              "You base your writing on the work of "
              "the Content Planner, who provides an outline "
              "and relevant context about the topic. "
              "You follow the main objectives and "
              "direction of the outline, "
              "as provide by the Content Planner. "
              "You also provide objective and impartial insights "
              "and back them up with information "
              "provide by the Content Planner. "
              "You acknowledge in your opinion piece "
              "when your statements are opinions "
              "as opposed to objective statements.",
    allow_delegation=False,
    verbose=True
)
editor = Agent(
    role="Editor",
    goal="Edit a given blog post to align with "
         "the writing style of the organization. ",
    backstory="You are an editor who receives a blog post "
              "from the Content Writer. "
              "Your goal is to review the blog post "
              "to ensure that it follows journalistic best practices,"
              "provides balanced viewpoints "
              "when providing opinions or assertions, "
              "and also avoids major controversial topics "
              "or opinions when possible.",
    allow_delegation=False,
    verbose=True
)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Task: -> takes atleast 3 variables -> description, expected_output and agent

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
plan = Task(
    description=(
        "1. Prioritize the latest trends, key players, "
            "and noteworthy news on {topic}.\n"
        "2. Identify the target audience, considering "
            "their interests and pain points.\n"
        "3. Develop a detailed content outline including "
            "an introduction, key points, and a call to action.\n"
        "4. Include SEO keywords and relevant data or sources."
    ),
    expected_output="A comprehensive content plan document "
        "with an outline, audience analysis, "
        "SEO keywords, and resources.",
    agent=planner,
)

write = Task(
    description=(
        "1. Use the content plan to craft a compelling "
            "blog post on {topic}.\n"
        "2. Incorporate SEO keywords naturally.\n"
		"3. Sections/Subtitles are properly named "
            "in an engaging manner.\n"
        "4. Ensure the post is structured with an "
            "engaging introduction, insightful body, "
            "and a summarizing conclusion.\n"
        "5. Proofread for grammatical errors and "
            "alignment with the brand's voice.\n"
    ),
    expected_output="A well-written blog post "
        "in markdown format, ready for publication, "
        "each section should have 2 or 3 paragraphs.",
    agent=writer,
)

edit = Task(
    description=("Proofread the given blog post for "
                 "grammatical errors and "
                 "alignment with the brand's voice."),
    expected_output="A well-written blog post in markdown format, "
                    "ready for publication, "
                    "each section should have 2 or 3 paragraphs.",
    agent=editor
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
Display the results of your execution as markdown in the notebook.
from IPython.display import Markdown
Markdown(result)
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

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Great Agent -> roles, goals and backstory
6 elements - 
1. Role playing
you - give me an analysis on tesla stock
you - you are a FINRA approved Financial analyst. Give me an analysis on tesla stock.
2. Focus
context/content -> trying to achieve -> many agents to achieve task
3. Tools
key tools to perform job
4. Cooperation/Collaboration
Take feedback, delegate task
5. Guardrails
define a framework ~ prevent hallucinations etc
AI applications fuzzy -> inputs, processing,  outputs
6. Memory
1. long term (in and after execusion, self improving and learning), 
2. short term (shared memory with other agents) 
3. Entity memory memory ( subject object)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Agents perform better when role playing
you should focus on goals and expectations
one agent can do multiple tasks
tasks and agents should be granular
tasks can be executed in different ways
its pretty easy to create multi-agents with crewai
