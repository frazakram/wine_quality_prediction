Here's a more natural, conversational version of the integration guide:
Hey there! 👋 Let me walk you through getting AgentNeo and CrewAI working together. I've set this up a bunch of times, so I'll share what works best.
Getting Started
First things first, make sure you've got Python 3.9+ on your machine (anything older and you might run into weird issues). You'll need admin rights to install stuff too.
Quick Setup:

Fire up your terminal and run:
bashCopypip install agentneo==1.2.1 crewai==0.70.1

Create a .env file for your keys - trust me, this keeps things clean. Drop in your OpenAI key like this:
CopyOPENAI_API_KEY=your_key_here

In your Python script, grab those environment variables:
pythonCopyfrom dotenv import load_dotenv
load_dotenv("path_to_your_env/.env")  # points to wherever you put your .env


The Fun Part: Setting Everything Up
Let's get your agents ready to roll. First, pull in what you need:
pythonCopyfrom crewai import Agent, Task, Crew, Process
from agentneo import AgentNeo, Tracer, Evaluation, launch_dashboard

# Set up AgentNeo (keep the names simple but meaningful)
neo_session = AgentNeo(session_name="MyTestRun")
neo_session.create_project(project_name="CoolProject")

# Start tracking what's happening
tracer = Tracer(session=neo_session)
tracer.start()
Putting It All Together
Now for the magic - building your crew and getting them working:
pythonCopycrew = Crew(
    agents=[your_agent],  # plug in your agents here
    tasks=[your_task],    # what you want them to do
    process=Process.sequential,  # or parallel, your call
    verbose=True  # helps with debugging
)

# Let 'em loose!
result = crew.kickoff()
print(result)
tracer.stop()
Checking How It Went
Want to see how well everything worked? Here's how to check:
pythonCopyexe = Evaluation(session=neo_session, trace_id=tracer.trace_id)
exe.evaluate([
    'goal_decomposition_efficiency',
    'goal_fulfillment_rate',
    'tool_call_correctness_rate',
    'tool_call_success_rate'
])

# See what happened
results = exe.get_results()
print(results)
Need a visual? Just launch the dashboard:
pythonCopyneo_session.launch_dashboard()
That's pretty much it! The code's straightforward once you get the hang of it. Let me know if anything's unclear or if you get stuck - I've probably run into the same issues before.
Pro tip: Start small with a simple agent and task before building anything complex. Makes troubleshooting way easier! CopyRetryClaude does not have the ability to run the code it generates yet.
