# Project-The-Historical-Court-Multi-Agent-System-
This project utilizes Google ADK to build a Multi-Agent System that simulates a 'Historical Court.' By assigning agents to research opposing viewpoints (Admirer vs. Critic) via Wikipedia, the system aims to produce the most unbiased and balanced historical verdict possible.

System Architecture & Requirement Mapping
This project implements a Multi-Agent System using Google ADK, designed to simulate a "Historical Court." The workflow follows the specific architecture requirements to ensure a balanced and neutral historical analysis.

1. The Inquiry (Sequential)
Requirement: Accept a topic from the user.

Implementation: The root_agent ("historical_court_clerk") serves as the entry point. It captures the user's input (e.g., "Napoleon Bonaparte") and stores it in the global state under the key PROMPT before passing control to the court_system.

2. The Investigation (Parallel)
Requirement: Two agents working simultaneously to find conflicting information.

Implementation: The investigation_team is a ParallelAgent that executes two sub-agents concurrently:

Agent A (The Admirer): Scrapes Wikipedia specifically for positive achievements and success stories, appending results to the pos_data state key.

Agent B (The Critic): Scrapes Wikipedia specifically for controversies, failures, and negative aspects, appending results to the neg_data state key.

3. The Trial & Review (Loop)
Requirement: A Judge agent reviews the data and loops back if information is insufficient.

Implementation: The trial_process is a LoopAgent that iterates through the investigation and the judicial review:

Agent C (The Judge): Analyzes the balance between pos_data and neg_data.

Loop Logic:

If the evidence is unbalanced or insufficient, the Judge writes specific instructions to the judge_feedback state key (which the Researcher agents read in the next iteration) and continues the loop.

If the evidence is sufficient, the Judge executes the exit_loop tool to break the cycle.

4. The Verdict (Output)
Requirement: Generate a neutral report and save it as a text file.

Implementation: The verdict_writer agent synthesizes the conflicting evidence from pos_data and neg_data into a comprehensive, neutral summary. It then utilizes the write_file tool to save the final output as a .txt file in the court_records directory.

Technical Constraints Implementation
Wiki Research: implemented via WikipediaQueryRun. Agents utilize dynamic instructions (referencing {judge_feedback}) to refine search keywords based on the Judge's requests.

State Management: The system strictly separates context using distinct keys (pos_data for the Admirer and neg_data for the Critic) to prevent information bias.

Loop Logic: The termination of the research phase is controlled programmatically via the exit_loop tool, ensuring the loop ends only when the Judge is satisfied, not just by token limits.
