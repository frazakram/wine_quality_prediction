
# Integrating AgentNeo with CrewAI

This guide provides a step-by-step walkthrough for integrating AgentNeo with CrewAI to enable efficient deployment, monitoring, and management of a collaborative AI agent team, as shown below.

---

### Prerequisites
- Python 3.9+ installed on your machine, as it is necessary for compatibility.
- A `.env` file to keep your API keys secure.

---

## Step 1: Set Up AgentNeo Environment

**First up, install AgentNeo and CrewAI** as follows:
   ```bash
   pip install agentneo==1.2.1 
   pip install crewai==0.70.1
   ```
**Set your API key as an `.env` variable for easy access**, as shown below:
   Set `OPENAI_API_KEY` as an environment variable:
   ```python
   OPENAI_API_KEY=<your_openai_api_key>
   ```

## Step 2: Initialize AgentNeo Session

The session is your workspace where all monitoring data is collected and analyzed. It’s crucial to set this up correctly, as follows:
```python
from agentneo import AgentNeo, Tracer, Evaluation, launch_dashboard
# Start your session (give it a name that makes sense to you)
neo_session = AgentNeo(session_name="your session name")
neo_session.create_project(project_name="your project name")
# Start keeping track of what's happening
tracer = Tracer(session=neo_session)
tracer.start()
```

## Step 3: Define Agent Roles in CrewAI
 Create Role Function  
   Define each agent’s, LLM’s, and task’s function in CrewAI based on the project requirements, and track them with `tracer`, as shown below:
   ```python
   @tracer.trace_llm("my_llm_call")
   async def my_llm_function():
       # Your LLM call here
       pass

   @tracer.trace_tool("my_tool")
   def my_tool_function():
       # Your tool logic here
       pass

   @tracer.trace_agent("my_agent")
   def my_agent_function():
       # Your agent logic here
       pass
   ```

## Step 4: Integrate CrewAI with Tracer

   Integrate the role functions in CrewAI’s configuration file, as follows:

   ```python
   from crewai import Crew, Process
   # Create a crew and execute the tasks
   crew = Crew(
       llm=[your llm name],
       agents=[your agent name],
       tasks=[your task name],
       process=Process.process_name,
       verbose=True/False,
   )
   ```
Here, you can integrate all the role functions with CrewAI, as the role functions are traced by `Tracer` of `AgentNeo`, i.e., CrewAI and AgentNeo are integrated now.

## Step 5: Kick off the Crew

**Kick off the crew** using `crew.kickoff()`, which initiates the analysis pipeline, as shown below:
```python
result = crew.kickoff()
```
After the tasks are completed, the `result` will contain the output of each task.

#### Stop the Tracer

```python
tracer.stop()
```
If tracing is active, the system will log the process, and calling `tracer.stop()` at the end will stop the logging.

### Step6 Need a Visual? Just Launch the Dashboard

You can visualize the workflow, as shown below:

```python
neo_session.launch_dashboard()
```

It will redirect you to a new tab in your default browser where you can see Detailed Analysis of your project 
