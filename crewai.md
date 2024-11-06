# Integration Guide for AgentNeo and CrewAI

This guide provides a comprehensive overview of how to integrate AgentNeo with CrewAI, focusing on setup, integration points, configuration, and troubleshooting. 
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
### Initialize CrewAI
```python
from crewai import Agent, Task, Crew, Process
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

### Define Tasks
Create tasks that each agent will perform.

### Create and Execute Crew
Connect agents and tasks into a crew and execute it.

```python
from crewai import Crew, Process

# Create a crew and execute the tasks
crew = Crew(
    agents=[],
    tasks=[],
    process=,
    verbose=,
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


