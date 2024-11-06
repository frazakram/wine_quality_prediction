# Integration Guide for AgentNeo and CrewAI

This guide provides a comprehensive overview of how to integrate AgentNeo with CrewAI, focusing on setup, integration points, configuration, and troubleshooting. 

## Table of Contents
- [Setup Steps for AgentNeo and CrewAI](#setup-steps-for-agentneo-and-crewai)
- [Key Integration Points](#key-integration-points)
- [Configuration Guidelines](#configuration-guidelines)
- [Common Troubleshooting Tips](#common-troubleshooting-tips)

## Setup Steps for AgentNeo and CrewAI

### Prerequisites
- Python 3.9 or higher
- Administrative access to install packages
- A `.env` file for environment variables containing necessary API keys and configurations.

### Installation
1. **Install Required Packages**:
   ```bash
   pip install  agentneo==1.2.1
   pip install crewai==0.70.1
   ```

2. **Create a .env File**:
   Create a `.env` file in your project directory with the required environment variables, such as API keys and configuration settings. For example:
   ```
   OPENAI_API_KEY=<your_openai_api_key>
   ```

3. **Load Environment Variables**:
   Prepare to load environment variables in your Python script:
   ```python
   from dotenv import load_dotenv
   load_dotenv("path_to_your_env/.env")
   ```

### Initialize AgentNeo Session
```python
from agentneo import AgentNeo, Tracer ,Evaluation, launch_dashboard

# Initialize the AgentNeo session
neo_session = AgentNeo(session_name="marketing_campaign_example")

# Connect to a project
neo_session.create_project(project_name="marketing_campaign_project")

# Create a tracer to track metrics
tracer = Tracer(session=neo_session)
tracer.start()
```

## Key Integration Points

### Build Agents
Define agents involved in your campaign and trace their interactions using the `Tracer`.
### Example

```python
from crewai import Agent

# Define a marketing strategist agent
@tracer.trace_agent("strategist")
def create_strategist():
    return Agent(
        role='Marketing Strategist',
        goal='Develop a high-impact marketing campaign for a new AI product',
        backstory="""You are a seasoned marketing strategist at a leading tech company.
        You excel at creating data-driven campaigns that resonate with target audiences.""",
        verbose=True,
        allow_delegation=False,
    )

# Define a creative director agent
@tracer.trace_agent("designer")
def create_designer():
    return Agent(
        role='Creative Director',
        goal='Design visually appealing assets for a marketing campaign',
        backstory="""You are a Creative Director with a flair for developing innovative designs.
        You translate marketing concepts into visual masterpieces.""",
        verbose=True,
        allow_delegation=True,
    )

# Create agent instances
strategist = create_strategist()
designer = create_designer()
```

### Define Tasks
Create tasks that each agent will perform.

```python
from crewai import Task

# Define tasks for the agents
task1 = Task(
    description="""Develop a comprehensive marketing strategy for launching a new AI product.
    Consider SEO, target demographics, and competition analysis.""",
    expected_output="Full marketing strategy document",
    agent=strategist
)

task2 = Task(
    description="""Create design assets (e.g., social media banners, infographics) based on the marketing strategy.
    Ensure consistency with the product's brand identity.""",
    expected_output="Designs in PNG format",
    agent=designer
)
```

### Create and Execute Crew
Connect agents and tasks into a crew and execute it.

```python
from crewai import Crew, Process

# Create a crew and execute the tasks
crew = Crew(
    agents=[strategist, designer],
    tasks=[task1, task2],
    process=Process.sequential,
    verbose=True,
)

# Kick off the crew
result = crew.kickoff()
print(result)

# Stop the tracer
tracer.stop()
```

### Evaluating Metrics
You can evaluate the success of the integration using metrics.

```python
exe = Evaluation(session=neo_session, trace_id=tracer.trace_id)

# run a single metric
exe.evaluate(metric_list=['goal_decomposition_efficiency', 
                         'goal_fulfillment_rate', 
                         'tool_call_correctness_rate', 
                         'tool_call_success_rate'])

# Print metric results
results = exe.get_results()
results
```

### Launch Dashboard
For monitoring the session metrics, launch the AgentNeo dashboard.

```python
# Launch the dashboard 
neo_session.launch_dashboard()
```

## Configuration Guidelines
- Ensure that the API keys in your `.env` file are correct and have necessary access rights.
- Validate that all required packages are installed and up-to-date.
- Regularly monitor for updates to `AgentNeo` and `CrewAI` libraries to leverage new features and fixes.
- Tailor agents' roles and tasks to fit the specific outcomes needed for your marketing campaigns.

## Common Troubleshooting Tips
- **Environment Variables Not Loaded**: Ensure the path to the `.env` file is correct and that it is formatted properly.
- **API Key Errors**: Validate that the API keys are correctly set up in the environment variables and have necessary permissions.
- **Connection Issues**: Verify that your internet connection is stable and that the APIs are operational.
- **Code Exceptions**: Always check for error messages in the console that can guide you to the root cause, such as module not found, incorrect parameters, etc.
- **Metrics Not Reflecting**: If metrics aren’t displaying correctly, ensure tracing is correctly implemented, and the environment is appropriately set up.

By following these guidelines, you can effectively integrate AgentNeo and CrewAI for your marketing campaigns, leveraging the unique capabilities of both platforms.
