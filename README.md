

# EXP 5: COMPARATIVE ANALYSIS OF DIFFERENT TYPES OF PROMPTING PATTERNS AND EXPLAIN WITH VARIOUS TEST SCENARIOS

# Aim: To test and compare how different pattern models respond to various prompts (broad or unstructured) versus basic prompts (clearer and more refined) across multiple scenarios.  Analyze the quality, accuracy, and depth of the generated responses 

### AI Tools Required: 

# Explanation: 
Define the Two Prompt Types:

Write a basic Prompt: Clear, detailed, and structured prompts that give specific instructions or context to guide the model.
Based on that pattern type refined the prompt and submit that with AI tool.
Get the ouput and write the report.

Prepare Multiple Test Scenarios:
Select various scenarios such as:
Generating a creative story.
Answering a factual question.
Summarizing an article or concept.
Providing advice or recommendations.
Or Any other test scenario
For each scenario, create both a naïve and a basic prompt. Ensure each pair of prompts targets the same task but with different levels of structure.
Run Experiments with ChatGPT:
Input the naïve prompt for each scenario and record the generated response.
Then input the corresponding basic prompt and capture that response.
Repeat this process for all selected scenarios to gather a full set of results.
Evaluate Responses : 
	Compare how ChatGPT performs when given naïve versus basic prompts and analyze the output based on Quality,Accuracy and Depth. Also analyse does ChatGPT consistently provide better results with basic prompts? Are there scenarios where naïve prompts work equally well?
Deliverables:
A table comparing ChatGPT's responses to naïve and basic prompts across all scenarios.
Analysis of how prompt clarity impacts the quality, accuracy, and depth of ChatGPT’s outputs.
Summary of findings with insights on how to structure prompts for optimal results when using ChatGPT.


# OUTPUT

# RESULT: The prompt for the above said problem executed successfully
Study of Prompt Templating Techniques for Automated Maintenance Report Generation

1. Introduction

Automated maintenance report generation is increasingly used in industries (manufacturing, aviation, power plants, automotive, etc.) to ensure real-time monitoring, predictive maintenance, and streamlined documentation. Traditionally, technicians manually fill out reports, but with LLMs and prompt engineering, reports can be automatically generated from IoT data, sensor logs, or technician input.


---

2. Prompt Templating Techniques

Prompt templating involves designing reusable, structured input formats that guide the LLM to produce consistent, accurate, and context-aware reports.

(a) Static Templates

Pre-defined fixed structure (like forms).

Example:


Maintenance Report Template:
- Equipment ID: {equipment_id}
- Issue Observed: {issue}
- Action Taken: {action}
- Spare Parts Used: {spares}
- Next Maintenance Due: {next_due}

Pros: Ensures consistency

Cons: Low flexibility for unusual cases



---

(b) Dynamic Templates (Slot Filling)

Template adapts to available data.

Example:


Generate a concise maintenance report for {equipment_type}. 
If sensor readings are available, summarize anomalies. 
If manual inputs exist, merge them into the report.

Pros: Flexible, handles missing data

Cons: Higher risk of vague output if prompts not well-designed



---

(c) Instruction + Context Templates

Separates task instructions from data context.

Example:


Instruction: Generate a formal maintenance report in tabular format.  
Context: {sensor_logs}, {technician_notes}, {previous_reports}

Pros: Clear separation, improves reliability



---

(d) Chain-of-Prompts Templates

Multi-step prompting:

1. Summarize logs


2. Identify faults


3. Draft structured report



Example:


Step 1: Summarize abnormal readings from {sensor_data}.  
Step 2: Suggest possible causes.  
Step 3: Generate a formatted maintenance report.

Pros: High accuracy, modular

Cons: More computational cost



---

(e) Hybrid Templates with Few-Shot Examples

Embeds sample reports as demonstration.

Example:


Example:
Equipment: Pump-12  
Issue: Excessive vibration  
Action: Bearing replaced  
Next Due: 30 days  

Now generate a report for: {equipment_data}

Pros: Produces human-like, domain-specific reports

Cons: Needs curated examples



---

3. Prompt Engineering in Command Pattern

The Command Pattern (from software design patterns) encapsulates a request as an object, allowing it to be parameterized, queued, logged, or undone.

When applied to prompt engineering, each prompt template = a Command object, and the report generator = the Invoker.

Mapping:

Command Object → A reusable prompt template (e.g., "Generate Summary Report").

Receiver → LLM (executes the command and generates output).

Invoker → Automation system that decides which prompt to run (based on sensor/technician input).

Client → Maintenance engineer/system user.



---

Example Implementation

Command Interface:

class ReportCommand:
    def execute(self, data):
        pass

Concrete Commands (Prompt Templates):

class SummaryReportCommand(ReportCommand):
    def execute(self, data):
        prompt = f"""
        Generate a summary maintenance report.
        Equipment: {data['equipment']}
        Issues: {data['issues']}
        Actions: {data['actions']}
        """
        return LLM.generate(prompt)

class DetailedReportCommand(ReportCommand):
    def execute(self, data):
        prompt = f"""
        Generate a detailed technical maintenance report in tabular format.
        Include: Equipment ID, Observed Faults, Root Cause Analysis, Spare Parts Used, Next Due Date.
        Data: {data}
        """
        return LLM.generate(prompt)

Invoker (Automation System):

class ReportGenerator:
    def __init__(self):
        self.commands = []

    def add_command(self, command):
        self.commands.append(command)

    def run(self, data):
        reports = []
        for cmd in self.commands:
            reports.append(cmd.execute(data))
        return reports


---

4. Benefits of Command Pattern in Prompt Engineering

Reusability → Same prompt template can be applied across equipment types.

Scalability → Easy to add new prompt commands (Summary, Root Cause, Predictive Report).

Flexibility → Invoker can decide execution order (e.g., generate summary first, then detailed).

Maintainability → Templates are modular and version-controlled.

🔹 Flowchart: Automated Maintenance Report Generation with Prompt Templates

flowchart TD
    A[Sensor Logs + Technician Notes] --> B[Automation System (Invoker)]
    B -->|Selects Command| C[Prompt Template (Command Object)]
    C --> D[LLM (Receiver)]
    D --> E[Generated Report]
    E --> F[Maintenance Engineer Review]


---

🔹 Diagram: Command Pattern in Prompt Engineering

classDiagram
    class Client {
        +requestReport()
    }
    class Invoker {
        +addCommand(cmd)
        +run(data)
    }
    class Command {
        <<interface>>
        +execute(data)
    }
    class SummaryReportCommand {
        +execute(data)
    }
    class DetailedReportCommand {
        +execute(data)
    }
    class LLMReceiver {
        +generate(prompt)
    }

    Client --> Invoker
    Invoker --> Command
    Command <|-- SummaryReportCommand
    Command <|-- DetailedReportCommand
    Command --> LLMReceiver


---

🔹 Example Visual Template for Reports

Static Template Example

Equipment ID	Issue Observed	Action Taken	Spare Parts Used	Next Maintenance Due

Pump-12	Excessive vibration	Bearing replaced	Bearing Type-X	30 days
---

🔹 Example Hybrid Template with Few-Shot Prompting

Prompt to LLM:

Example:
Equipment: Pump-12  
Issue: Excessive vibration  
Action: Bearing replaced  
Next Due: 30 days  

Now generate a report for:  
Equipment: Motor-7  
Issue: Overheating  
Action: Cooling fan replaced  
Next Due: 45 days

Output:

Equipment ID	Issue Observed	Action Taken	Spare Parts Used	Next Maintenance Due

Motor-7	Overheating	Cooling fan replaced	Fan Model-FX	45 days
---

✅ With flowcharts + class diagram + sample templates, this becomes a visually structured report.

---

5. Conclusion

By combining prompt templating techniques with Command Pattern, automated maintenance report generation can be made:

Consistent (structured templates),

Adaptable (dynamic slots, few-shot),

Maintainable (modular command objects).


This approach transforms LLMs into plug-and-play reporting engines that scale with industrial needs.
