# Integration Guide for AgentNeo and CrewAI
#CrewAI
CrewAI is a framework for easily building multi-agent applications.

This guide provides a comprehensive overview of how to integrate AgentNeo with CrewAI, focusing on setup, integration points, configuration, and troubleshooting. AgentNeo and CrewAI teamed up to make monitoring Crew agents dead simple.
## Setup Steps for AgentNeo and CrewAI

### Prerequisites
- Python 3.9 + on your machine
- A `.env` file for environment variables containing necessary API keys and configurations.

### Installation
1. **Install the AgentNeo and crewAI SDK**:
   ```bash
   pip install  agentneo==1.2.1
   pip install crewai==0.70.1
   ```

2. **Set your API key as an .env variable for easy access**: 
   Set `OPENAI_API_KEY` as an environment variable
   ```python
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
neo_session = AgentNeo(session_name="Your Session name")
# Connect to a project
neo_session.create_project(project_name="Your Project name")
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
    agents=[your agent name],
    tasks=[your task name],
    process=Process.process_name,
    verbose=True/False,
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
# run  metrics
exe.evaluate(metric_list=['goal_decomposition_efficiency', 
                         'goal_fulfillment_rate', 
                         'tool_call_correctness_rate', 
                         'tool_call_success_rate'])
# Print metric results
results = exe.get_results()
results
```
### Need a visual? Just launch the dashboard:

```python
# Launch the dashboard 
neo_session.launch_dashboard()
```
