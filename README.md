# AI Collaboration Manifesto (v1.0)

## Table of Contents

1. [Overview](#1-overview "null")

   - [1.0. Preamble](#10-preamble "null")

   - [1.1. Purpose of This Manifesto](#11-purpose-of-this-manifesto "null")

   - [1.2. Guiding Principles for Broader Applications](#12-guiding-principles-for-broader-applications "null")

   - [1.3. TLDR: The Basic Collaboration Flow](#13-tldr-the-basic-collaboration-flow "null")

2. [Core Principles](#2-core-principles "null")

3. [Communication Protocol & Data Format](#3-communication-protocol--data-format "null")

   - [3.1. Manifest File (`YYYYMMDD_HHMMSS_UNIQUEID_aitaskinput.json`)](#31-manifest-file-yyyymmdd_hhmmss_uniqueid_aitaskinputjson "null")

   - [3.2. Content Chunks File (`YYYYMMDD_HHMMSS_UNIQUEID_project_content.txt`)](#32-content-chunks-file-yyyymmdd_hhmmss_uniqueid_project_contenttxt "null")

4. [File Handling Specifics](#4-file-handling-specifics "null")

   - [4.1. New Files & Root Folder Files](#41-new-files--root-folder-files "null")

   - [4.2. Updating Existing Files (`update_text_file`)](#42-updating-existing-files-update_text_file "null")

   - [4.3. Project Files (`.csproj`, `.sln`)](#43-project-files-csproj-sln "null")

   - [4.4. Binary Files (Small & Large Multipart)](#44-binary-files-small--large-multipart "null")

   - [4.5. Handling Non-Code and Structured Data](#45-handling-non-code-and-structured-data "null")

5. [Workflow](#5-workflow "null")

6. [Considerations for AI Agent Implementation](#6-considerations-for-ai-agent-implementation "null")

7. [Safety, Ethics, and Responsible AI Use](#7-safety-ethics-and-responsible-ai-use "null")

   - [7.1. Content Sensitivity and Appropriateness](#71-content-sensitivity-and-appropriateness "null")

   - [7.2. Data Privacy and User Information](#72-data-privacy-and-user-information "null")

   - [7.3.](#73-source-attribution-and-verification-for-factual-content "null") Source Attribution and Verification [(for Factual Content)](#73-source-attribution-and-verification-for-factual-content "null")

   - [7.4. User Feedback on Content Quality and Safety](#74-user-feedback-on-content-quality-and-safety "null")

8. [AI Agent Capabilities and Constraints](#8-ai-agent-capabilities-and-constraints "null")

9. [User Environment and Tooling](#9-user-environment-and-tooling "null")

10. [Customization, Usage Scenarios, and Future Directions](#10-customization-usage-scenarios-and-future-directions "null")

    - [10.1. Customizing the Manifesto](#101-customizing-the-manifesto "null")

    - [10.2. Possible Future Ideas & Enhancements (Beyond v1.0)](#102-possible-future-ideas--enhancements-beyond-v10 "null")

    - [10.3. Usage Notes & Best Practices](#103-usage-notes--best-practices "null")

    - [10.4. Concrete Usage Scenarios (Examples)](#104-concrete-usage-scenarios-examples "null")

11. [Repository Contents & Examples](#11-repository-contents--examples "null")

    - [11.1. Key Files in this Repository](#111-key-files-in-this-repository "null")

    - [11.2. Using the Examples](#112-using-the-examples "null")

12. [License](#12-license "null")

13. [Contribution Guidelines](#13-contribution-guidelines "null")

14. [Frequently Asked Questions (FAQ)](#14-frequently-asked-questions-faq "null")

15. [Appendices](#15-appendices "null")

    - [15.1. Appendix A: `aitaskinput.json` (v1.0) - Detailed Structure and Task Types](#151-appendix-a-aitaskinputjson-v10---detailed-structure-and-task-types "null")

    - [15.2.](#152-appendix-b-ai-agent-capabilities-v10---detailed-fields "null") Appendix B: AI Agent Capabilities (v1.0) [- Detailed Fields](#152-appendix-b-ai-agent-capabilities-v10---detailed-fields "null")

## 1. Overview

### 1.0. Preamble

Recognizing the transformative potential of collaboration between human users and Artificial Intelligence (AI) agents, and desiring to establish a clear, consistent, and responsible framework for such interactions, this Manifesto is set forth. It aims to govern the exchange of information and deliverables, ensuring clarity, traceability, safety, and adaptability across a multitude of applications, from software development to general life assistance. This document (Version 1.0) is intended as a foundational yet comprehensive guide, designed to promote mutual understanding and efficient workflows, and to evolve through community input and technological advancement.

### 1.1. Purpose of This Manifesto

This document serves as a comprehensive guide for a structured and responsible collaboration between a human user and an AI agent. It outlines a standardized process and data format for the AI to deliver various types of assets—from software code and project files to research summaries, structured data, and lifestyle plans. The aim is to streamline how users receive and integrate AI-generated content, fostering efficient project setup, updates, and ongoing management, while emphasizing safety, clarity, and adaptability across diverse applications.

### 1.2. Guiding Principles for Broader Applications

Beyond code, this framework is intended to support a wide array of tasks, including research assistance, data organization, creative generation, and lifestyle management. Key principles include:

- **Adaptability:** The system should be flexible enough to handle diverse content types and user needs.

- **User Well-being:** Prioritize the generation of helpful, harmless, and appropriate content, especially for personal or sensitive topics.

- **Privacy by Design:** Emphasize user consent and transparent data handling practices when personal information is involved.

- **Responsible Information Provision:** For factual content, strive for accuracy and provide means for verification where possible.

The collaboration process detailed herein relies on three key components for each delivery iteration:

1. **A Manifest File (e.g., `YYYYMMDD_HHMMSS_UNIQUEID_aitaskinput.json`):** A JSON file detailing the current delivery's scope. It includes:

   - Overall project/task context.

   - A list of tasks for *this specific delivery*.

   - Metadata for each asset (e.g., file paths).

   - A unique `current_run_correlation_id` for tracking this interaction.

   - A `manifest_format_version` (referring to this Manifesto's version, e.g., "1.0").

   - **Important:** File paths **must** use forward slashes (`/`) for cross-platform compatibility.

2. **A Content Chunks File (e.g., `YYYYMMDD_HHMMSS_UNIQUEID_project_content.txt`):** This text file contains the actual data for the tasks listed in the accompanying manifest file.

   - It **must** begin with a "run log" chunk, which includes the `run_correlation_id` and AI notes for the current delivery.

   - Subsequent chunks contain the content for other tasks, each clearly delimited.

3. **A Client-Side Script (e.g., `AIDumpEnhanced.ps1`):** A versioned PowerShell script (or a similar tool chosen by the user) that reads the manifest and content chunks file to create or update files and data on the user's system.

### 1.3. TLDR: The Basic Collaboration Flow

For those looking for a quick overview:

1. **User Asks AI:** You request something from the AI (e.g., "write code for X," "summarize Y," "plan Z").

2. **AI Prepares Delivery:** The AI plans the work and creates two main files, typically named with a shared timestamp and unique ID (e.g., `20250528_131500_dEfGhI_aitaskinput.json` and `20250528_131500_dEfGhI_project_content.txt`):

   - The `..._aitaskinput.json` file: A "to-do list" for *this specific delivery*, including the unique `current_run_correlation_id`.

   - The `..._project_content.txt` file: The actual content, starting with a log of what the AI did for this run (including the correlation ID) and any notes, followed by the requested information/files.

3. **AI Delivers Files:** You receive these two files from the AI.

4. **User Processes Files:** You save these files and then run a local script (like the provided `AIDumpEnhanced.ps1`). This script reads the `..._aitaskinput.json` file and uses the `..._project_content.txt` file to create/update files on your computer or present data.

5. **User Reviews:** You check the results, including the AI's notes in the log file.

6. **Iterate:** If more work is needed, you ask the AI again, possibly referring to the unique `current_run_correlation_id` for context. The AI then prepares a new set of files for the next part of the work.

## 2. Core Principles

- **Structured Data:** All information is passed in a well-defined, machine-readable format (JSON, delimited text).

- **Run-Specific Context:** Each delivery run is identifiable by a unique `current_run_correlation_id`, and the AI provides notes on its generation process for that run.

- **Dynamic Task Management:** The `aitaskinput.json` for each run lists only the tasks being delivered *in that run*. The AI manages the overall project scope and what's pending for future runs.

- **Atomicity (for files/outputs):** Each output (file, data snippet) is treated as a complete unit for that delivery. Updates may include embedded change markers (for code) or detailed explanations (for prose/data) in the run log.

- **Idempotency (for script execution):** Running the client script multiple times with the same input files should ideally produce the same state on the user's system (e.g., by overwriting files).

- **Scalability:** The system is designed to handle both small, quick tasks and larger, multi-part projects.

- **User Control & Resumability:** The `aitaskinput.json` and run logs allow users to track progress and understand each delivery.

- **Git Compatibility:** For code, embedded conflict markers in updates work seamlessly with Git tools. The Markdown format of this Manifesto is also inherently Git-friendly.

- **Cross-Platform Paths:** File paths in the manifest use `/`, ensuring client scripts can adapt them for any OS.

- **Versioning:** Both this Manifesto format (e.g., "1.0") and any client-side scripts should be versioned to manage changes and ensure compatibility.

## 3. Communication Protocol & Data Format

The AI-user collaboration relies on a pair of files for each delivery iteration: a manifest file and a content chunks file.

### 3.1. Manifest File (`YYYYMMDD_HHMMSS_UNIQUEID_aitaskinput.json`)

This JSON file is the central control document for each delivery. It specifies the tasks to be performed by the client-side script using content from the associated content chunks file.

- **Filename Convention:** The manifest filename should incorporate a timestamp and a unique identifier (e.g., `20250528_131500_dEfGhI_aitaskinput.json`). This unique identifier should match that of the corresponding content chunks file.

- **Path Separators:** All `file_path` entries within the JSON structure **must** use forward slashes (`/`).

- **Core Structure Overview:**
  
  - `manifest_format_version`: (String) Version of this Manifesto structure (e.g., "1.0").
  
  - `project_name`: (String) A human-readable name for the overall project or task.
  
  - `description`: (String) A brief description of the current delivery.
  
  - `current_run_correlation_id`: (String) A unique ID generated by the AI for this specific delivery run.
  
  - `solution_root_hint`: (String, Optional) A suggested base path for file creation.
  
  - `content_chunk_file`: (String) The exact filename of the associated content chunks file.
  
  - `ai_agent_profile`: (Object) Information about the AI agent (see Section 8 and Appendix B).
  
  - `tasks`: (Array of Objects) A list of task objects for *this specific delivery*.
  
  - `post_processing_commands`: (Array of Strings, Optional) Suggested commands for the user.
  
  - `agent_capabilities`: (Object, Optional) A snapshot or reference to the AI agent's capabilities.

For a detailed breakdown of the `aitaskinput.json` structure, task object fields, and available task types for v1.0, refer to **Appendix** A: `aitaskinput.json` (v1.0) - Detailed Structure **and Task Types**.

### 3.2. Content Chunks File (`YYYYMMDD_HHMMSS_UNIQUEID_project_content.txt`)

This plain text file contains the actual data (code, prose, structured data, Base64 strings) for the tasks defined in the manifest file.

- **Filename Convention:** The content chunks filename should incorporate the same timestamp and unique identifier as its corresponding manifest file (e.g., `20250528_131500_dEfGhI_project_content.txt`).

- **Structure:** The file consists of one or more "chunks," each clearly delimited.
  
  - **Run Log Chunk:** The **first chunk** in this file **must** be the run log for the current delivery. Its `content_chunk_id` (defined in the delimiter) will match the `content_chunk_id` of the log task in the manifest.
  
  - **Content Delimiters:** Each chunk starts with `// --- START CHUNK: <content_chunk_id> ---` on a new line and ends with `// --- END CHUNK: <content_chunk_id> ---` on a new line. The `<content_chunk_id>` must be unique within this file and match a `content_chunk_id` specified in one of the tasks in the manifest.
  
  - **Content:** The lines between the start and end delimiters constitute the actual content for that chunk.

**Example** Run Log Chunk (within the content chunks **file):**

```
// --- START CHUNK: run_log_content_20250528_131500_dEfGhI ---
AI Collaboration Run Log
Timestamp: 2025-05-28 13:15:00 UTC
Run Correlation ID: run_20250528131500_dEfGhI
Content Target: project_logs/20250528_131500_dEfGhI_ai_collab_run.log
AI Agent: Gemini Pro 1.2.2
Manifest Format Version Used: 1.0

--- AI Notes for This Run ---
Summary of Generation:
This run delivers the initial Python script `main.py` and a Markdown meal plan.
...
Errors/Warnings During Generation:
...
Next Steps / Pending Tasks (from AI perspective):
...
Tasks Delivered in this Chunk File (20250528_131500_dEfGhI_project_content.txt):
- log_run_20250528131500_dEfGhI (this log)
- task_001_main_script_py
- task_002_weekly_menu
Disclaimer:
...
---
Associated aitaskinput.json (filename: 20250528_131500_dEfGhI_aitaskinput.json) lists tasks included in *this* delivery.
The AI will generate a new manifest for any subsequent deliveries, reflecting tasks still pending.
// --- END CHUNK: run_log_content_20250528_131500_dEfGhI ---
```

## 4. File Handling Specifics

### 4.1. New Files & Root Folder Files

New files are created based on tasks like `create_text_file`, `generate_prose`, or `generate_structured_data` defined in the manifest. The client script is responsible for creating any necessary subdirectories specified in the `file_path`.

### 4.2. Updating Existing Files (`update_text_file`)

For `update_text_file` tasks, the AI provides the *complete new content* for the file in the corresponding content chunk.

- For code files, this content may include Git-style diff markers (e.g., `<<<<<<<` AI `Update ... ======= ... >>>>>>> Local Original`) to visually highlight changes. The client script writes this entire content (including markers) to the local file. Users can then leverage Git tools or IDE features to review, accept, or merge these changes.

- For prose, structured data, or other non-code content where diff markers are less suitable, the AI should provide the complete new content and clearly explain the changes in the "AI Notes for This Run" section of the run log.

### 4.3. Project Files (`.csproj`, `.sln`)

For project configuration files like `.csproj` or `.sln` files (common in .NET projects), it's often better for the user to generate these using native development tools (e.g., `dotnet new sln`). The AI can assist by providing the *content* for these files as guides, using the `create_project_file_guide` task type. This typically saves the content with a `.txt` extension (e.g., `MyProject.csproj.txt`), which the user can then reference.

### 4.4. Binary Files (Small & Large Multipart)

- **Small Binary Files:** Use the `create_binary_file_base64` task type. The content chunk contains the full Base64 encoded string of the binary file.

- **Large Binary Files:** Use the `create_binary_file_multipart_base64` task type for each part. The AI splits the Base64 encoded binary into manageable parts. Each part is a separate task in the manifest, linked by a common `assembly_id`. The `part_number` and `total_parts` fields are crucial for the client script to reassemble the parts in the correct order.

### 4.5. Handling Non-Code and Structured Data

Tasks like `generate_prose` and `generate_structured_data` produce content intended for specific file types (e.g., `.md`, `.json`, `.csv`, `.txt`). The `file_path` in the manifest task should use the appropriate extension. The optional `output_format_hint` field in the task can provide additional context for the user or client script about the expected data structure (e.g., "json_array_of_objects", "markdown_table"). Updates to these types of files are typically handled by the AI regenerating the entire content chunk based on user feedback, with changes explained in the run log.

## 5. Workflow

The workflow described below outlines the typical interaction, with the AI choosing appropriate task types (see Appendix A) and adhering to the naming and versioning conventions specified in this Manifesto.

1. **User Request.**

2. **AI Processing:**

   - AI determines files/outputs for the current delivery batch, selecting appropriate task types.

   - **Correlation ID & Versioning:** Generates `current_run_correlation_id`. Notes current `manifest_format_version`. Generates a unique identifier string (e.g., `dEfGhI`) for filenames.

   - **Log File Generation:** Prepares `file_path` (e.g., `project_logs/YYYYMMDD_HHMMSS_UNIQUEID_ai_collab_run.log`) and content for the run log.

   - **Manifest & Content File Preparation:** AI prepares the `YYYYMMDD_HHMMSS_UNIQUEID_aitaskinput.json` listing *only* tasks for this delivery and the corresponding `YYYYMMDD_HHMMSS_UNIQUEID_project_content.txt`.

3. **AI Response:** AI delivers the uniquely named `..._aitaskinput.json` and `..._project_content.txt`.

4. **User Action:** User saves files, runs client script, passing the name of the `..._aitaskinput.json` file. The script infers or is told the name of the content file (e.g., via the `content_chunk_file` field).

5. **Script Execution:** Client script processes files based on the manifest.

6. **User Review & Integration:** User reviews outputs, using run log for context.

## 6. Considerations for AI Agent Implementation

- **Path Separators:** Mandatory: Use forward slashes (`/`) for `file_path` in the manifest.

- **Diff Markers/Update Explanations:** Use Git-style diff markers for code updates. For non-code content, explain updates clearly in the run log notes.

- **Dynamic `aitaskinput.json` Management:** The `aitaskinput.json` provided with each delivery should list *only* the tasks whose content is in the accompanying content chunks file. The AI manages the overall master list of pending tasks internally and generates a new, relevant `aitaskinput.json` for each subsequent delivery.

- **Run Correlation ID, Filename Correlation & Versioning:**
  
  - Generate a unique `current_run_correlation_id` for each delivery run and include it in the manifest and run log.
  
  - Generate a shared unique identifier (timestamp + random string) to be part of both the manifest filename and the content chunks filename for easy pairing.
  
  - Include the current `manifest_format_version` ("1.0" for this version of the Manifesto) in `aitaskinput.json`.
  
  - The AI agent should also have its own versioning for its capabilities profile.
  
  - Client-side scripts (like `AIDumpEnhanced.ps1`) should also be versioned by their maintainers.

- **Run Log File Generation:**
  
  - Log file names must start with date and include the shared unique ID for sorting and correlation (e.g., `project_logs/YYYYMMDD_HHMMSS_UNIQUEID_ai_collab_run.log`).
  
  - Log content chunk must be first in the content chunks file.
  
  - Include `run_correlation_id`, timestamp, AI agent info, `manifest_format_version` used, AI notes, and disclaimers.

- **Clarity for Non-Technical Users:** AI notes and instructions in the run log should use clear, accessible language, avoiding jargon where possible.

- The `content_chunk_file` field in `aitaskinput.json` must exactly match the actual filename of the delivered content chunks file.

- Encoding, Error Handling, Chunk Size, Security: As previously defined in earlier sections of this Manifesto.

## 7. Safety, Ethics, and Responsible AI Use

This section outlines key considerations for safety, ethics, and responsible AI use.

### 7.1. Content Sensitivity and Appropriateness

- The AI agent should state its policy or approach to handling potentially sensitive topics (e.g., medical, financial, legal, psychological advice) in its capabilities profile (Section 8 and Appendix B).

- For information that could be misconstrued as professional advice, the AI should, if capable, generate and include appropriate disclaimers (e.g., in the run log or directly in the content).

- A mechanism or instruction should be provided for users to flag or provide feedback on content they perceive as inappropriate, harmful, or biased.

### 7.2. Data Privacy and User Information

- The AI agent **must** declare its data handling practices (regarding user inputs, generated content, storage, use for training, anonymization) in its capabilities profile (Section 8 and Appendix B) via a summary or link to a privacy policy.

- For tasks involving personal information (e.g., meal plans based on health data, personal research notes, shopping lists reflecting household details), the AI should:
  
  - Minimize collection of PII to only what is essential for the task.
  
  - Seek explicit user consent if personal data needs to be "remembered" across sessions for personalization, explaining purpose and duration.

- Users should be advised against sharing highly sensitive personal information unless they fully understand and consent to the AI's data handling practices.

### 7.3. Source Attribution and Verification (for Factual Content)

- For tasks involving research, factual summaries, or product comparisons, the AI should make a best effort to:
  
  - Provide sources, references, or links within the generated content or in the "AI Notes" section of the run log, if its capabilities allow (`source_attribution_capability`).
  
  - Clearly indicate when information is estimated, simulated, or from a non-verifiable source.

- The AI may suggest that users independently verify critical information, especially for decisions with significant consequences.

### 7.4. User Feedback on Content Quality and Safety

- The AI or its platform should provide a clear channel for users to report issues regarding content accuracy, safety, bias, or overall quality. This feedback is crucial for improving the AI's performance and responsibility.

## 8. AI Agent Capabilities and Constraints

An AI agent participating in this collaboration protocol should declare its capabilities and operational constraints as detailed below and in Appendix B. This information can be provided within the `ai_agent_profile` and `agent_capabilities` sections of the `aitaskinput.json` file, or in a separate, referenced document. This transparency helps users understand what to expect from the AI.

**Example Structure Snippet for `aitaskinput.json`:**

```
// In aitaskinput.json:
// "ai_agent_profile": { /* ... */ },
// "agent_capabilities": {
//    "core_capabilities_v1": { // Matches capabilities_ref
//        "manifesto_versions_supported": ["1.0"], // Versions of this Manifesto it supports
//        /* ... other capabilities from Appendix B ... */
//        "notes_for_user": "This agent supports Manifesto v1.0. Each delivery includes a run log with a correlation ID..."
//    }
// }
```

For a detailed list and description of recommended capability fields for v1.0, refer to **Appendix B: AI Agent Capabilities (v1.0) - Detailed Fields**.

## 9. User Environment and Tooling

This section outlines typical user environment and tooling considerations. Users should ensure they have the necessary tools installed and configured to effectively use the outputs generated according to this Manifesto. Key tools often include:

- **Operating System:** Windows, macOS, or Linux. The client-side script should ideally be cross-platform or have versions for common OSs.

- **PowerShell (or chosen scripting environment):** If using the example `AIDumpEnhanced.ps1` script, PowerShell 7+ (Core) is recommended for cross-platform compatibility. If using a different client script, the relevant interpreter (e.g., Python, Node.js) is needed.

- **.NET SDK (Optional, for .NET projects):** Required for `dotnet` CLI commands if the AI provides guidance for .NET project setup.

- **Git:** Essential for version control, especially when dealing with code updates and reviewing changes highlighted by diff markers.

- **Integrated Development Environment (IDE) or Text Editor:** Visual Studio, VS Code (with relevant extensions for C#, Python, PowerShell, etc.), JetBrains Rider, or other editors suitable for the type of content being generated (code, Markdown, JSON).

## 10. Customization, Usage Scenarios, and Future Directions

This Manifesto provides a foundational framework (v1.0), designed for evolution.

### 10.1. Customizing the Manifesto

- **For Individual Users/Teams:** Users can adapt this Manifesto for specific project types or workflows. This might involve defining new custom `task_types`, specifying additional metadata fields, or tailoring the "AI Agent Capabilities" section.

- **For AI Developers:** AI agents can be designed to support specific subsets or extensions of this Manifesto. It's crucial to clearly document which version(s) of the Manifesto an agent supports (see `manifesto_versions_supported` in Appendix B).

- **Versioning Customizations:** If users create a significantly modified version, they should assign it a new version number (e.g., "MyOrgManifesto_v1.0") and ensure their client scripts are updated.

- **Formal Structure Adoption:** For organizations requiring a more formal governance document, this Manifesto's structure and content can be adapted to align with specific styles, such as those used for legal acts or internal standards, by reformatting and potentially expanding sections with more explicit articles or clauses.

### 10.2. Possible Future Ideas & Enhancements (Beyond v1.0)

- **Interactive Task Confirmation:** AI could list proposed tasks, and the user confirms/modifies them before content generation.

- **Dependency Management between Tasks:** A way to specify that one task depends on another's output.

- **More Granular Update Types:** Beyond `update_text_file`, specific types like `append_to_file`, `insert_at_line`, `replace_section_by_id`. This would require more complex client scripts.

- **Schema Validation for `generate_structured_data`:** AI could provide a JSON schema for the structured data it intends to generate, allowing for pre-validation.

- **Direct Cloud Storage Integration:** Tasks to directly save/retrieve files from user-specified cloud storage (requiring secure authentication mechanisms).

- **Enhanced Feedback Loop:** A structured way for users to send feedback on specific delivered tasks directly via a modified `aitaskinput.json` or a new `aifeedback.json` file.

- **Multi-Agent Collaboration:** Defining roles and handoff points if multiple specialized AI agents are involved in a project.

- **Client Script Capability Negotiation:** A pre-run check where the client script informs the AI about its capabilities (e.g., "can handle multipart base64," "understands diff markers").

- **Expanded Task Types:** Future versions might reintroduce more specialized task types like `generate_report_prose` and `recommendation_list` if the community finds the consolidated `generate_prose` in v1.0 too broad for certain use cases.

### 10.3. Usage Notes & Best Practices

- **Start Small:** For new projects or complex requests, begin with a few tasks to establish the workflow and ensure the AI understands the requirements.

- **Be Specific in Requests:** Clear, detailed prompts to the AI lead to better, more relevant outputs. Refer to `run_correlation_id`s from previous logs for context.

- **Iterative Refinement:** Use the AI notes and your review process to guide subsequent iterations. Don't expect perfection in the first run.

- **Client Script Maintenance:** If using a custom client script, keep it updated to handle any new task types or features introduced by the AI or Manifesto updates.

- **Security for Client Scripts:** Always review client scripts (like `AIDumpEnhanced.ps1`) from any source before running them, as they operate on your local file system.

- **Backup `aitaskinput.json`:** Periodically save versions of `aitaskinput.json` if you want a history of pending tasks beyond what the AI manages. The run logs provide a history of delivered tasks.

- **Understand AI Limitations:** Refer to the AI agent's declared capabilities (Section 8 and Appendix B) and the safety guidelines (Section 7). AI is a tool to assist, not replace, human oversight and critical thinking.

### 10.4. Concrete Usage Scenarios (Examples)

(These scenarios illustrate the application of Manifesto v1.0 task types.)

**Scenario 1: Code Development Project**

- **User Request:** "AI, please generate the initial structure for a Python Flask web application. It should have a basic 'Hello World' endpoint, a `requirements.txt` file, and a simple `README.md`."

- **AI Processing (Conceptual):**
  
  1. Generates `run_correlation_id` (e.g., `run_20250528140000_dev001`).
  
  2. Prepares `20250528_140000_dev001_aitaskinput.json` (v1.0) with tasks:

     - Task for `project_logs/20250528_140000_dev001_ai_collab_run.log`.

     - Task for `src/app.py` (`create_text_file`).

     - Task for `requirements.txt` (`create_text_file`).

     - Task for `README.md` (`create_text_file`).
  
  3. Prepares `20250528_140000_dev001_project_content.txt` with corresponding chunks.

- **User Action:** Runs client script. Files are created.

- **Next Iteration (User Request):** "AI (ref: `run_20250528140000_dev001`), please add a new endpoint `/data` to `src/app.py` that returns a simple JSON object. Also, update the README with setup instructions."

- **AI Processing:** Generates new run ID, new manifest with `update_text_file` tasks for `src/app.py` and `README.md`.

**Scenario** 2: Research Assistance

- **User Request:** "AI, I need a summary of the key challenges in renewable energy adoption, focusing on solar and wind power. Please provide it as a Markdown document and list any major reports or studies you referenced."

- **AI Processing (Conceptual):**
  
  1. Generates `run_correlation_id`.
  
  2. Prepares manifest with tasks:

     - Log file task.

     - Task for `research/renewable_energy_challenges.md` (`generate_prose`, `output_format_hint: "markdown"`).
  
  3. Prepares content file with run log (AI notes: "Summarized challenges... Simulated sources listed... Disclaimer...") and Markdown content.

**Scenario 3: Lifestyle Management (Meal Planning)**

- **User Request:** "AI, create a 7-day vegetarian meal plan. Output as a JSON file. Also, generate a shopping list as a simple text file."

- **AI Processing (Conceptual):**
  
  1. Generates `run_correlation_id`.
  
  2. Prepares manifest with tasks:

     - Log file task.

     - Task for `lifestyle/meal_plans/vegetarian_week1.json` (`generate_structured_data`, `output_format_hint: "json"`).

     - Task for `lifestyle/shopping_lists/vegetarian_week1_shop.txt` (`create_text_file`).
  
  3. Prepares content file with run log and corresponding data chunks.

**Scenario 4: Product Comparison**

- **User Request:** "AI, compare three laptops... Present it as a Markdown table."

- **AI Processing (Conceptual):**
  
  1. Generates `run_correlation_id`.
  
  2. Prepares manifest with tasks:

     - Log file task.

     - Task for `comparisons/laptops_graphic_design.md` (`generate_prose`, `output_format_hint: "markdown_table"`).
  
  3. Prepares content file with run log and Markdown table content.

## 11. Repository Contents & Examples

This GitHub repository serves as the home for the AI Collaboration Manifesto and related resources.

### 11.1. Key Files in this Repository

- **`README.md`**: This file, containing the AI Collaboration Manifesto (v1.0) itself.

- **`./examples/`**: This directory contains subdirectories for each task type defined in Appendix A of this Manifesto. Each subdirectory provides:
  
  - An example `..._aitaskinput.json` file.
  
  - An example ..._project_content.txt file.

    These examples illustrate how an AI (using "Gemini AI" as the agent_name for demonstration) would structure its output for that specific task type.
  
  - [Example: `create_text_file`](https://gemini.google.com/app/examples/create_text_file/ "null")
  
  - [Example: `update_text_file`](https://gemini.google.com/app/examples/update_text_file/ "null")
  
  - [Example: `generate_prose`](https://gemini.google.com/app/examples/generate_prose/ "null")
  
  - [Example: `generate_structured_data`](https://gemini.google.com/app/examples/generate_structured_data/ "null")
  
  - [Example:](https://gemini.google.com/app/examples/create_binary_file_base64/ "null") `create_binary_file_base64`
  
  - [Example: `create_binary_file_multipart_base64`](https://gemini.google.com/app/examples/create_binary_file_multipart_base64/ "null")
  
  - [Example: `create_project_file_guide`](https://gemini.google.com/app/examples/create_project_file_guide/ "null")

- **`./client_scripts/` (Recommended)**: This directory could house client-side parser scripts.
  
  - An example `AIDumpEnhanced.ps1` (PowerShell script) might be provided here or linked from another repository.

- **`LICENSE.md` (Recommended)**: Users of this Manifesto for their own projects should include a license file. See Section 12.

- **`CONTRIBUTING.md` (Recommended)**: If you wish to contribute to the evolution of this Manifesto, please refer to this file (once created). See Section 13.

### 11.2. Using the Examples

The files in the `./examples/` directory are intended to:

- Provide clear, runnable demonstrations of the data format for AI agents.

- Help users and developers understand how to implement client-side parsers.

- Serve as test cases for client-side scripts.

Each example pair (`..._aitaskinput.json` and `..._project_content.txt`) represents a single, self-contained delivery from an AI for a specific task type.

## 12. License

This AI Collaboration Manifesto (v1.0) itself is intended to be an open standard. If publishing this document or derivatives on platforms like GitHub, it is recommended to include an open-source license to clarify terms of use, modification, and distribution.

Consider licenses such as:

- **MIT License:** Permissive, allows for broad use and modification.

- **Apache License 2.0:** Permissive, includes patent grant provisions.

- **Creative Commons (e.g., CC BY 4.0):** Suitable for documentation, allowing sharing and adaptation with attribution.

The choice of license should reflect the goals of the community or organization adopting and evolving this Manifesto. A `LICENSE.md` file should typically be included in the root of a repository containing this document.

## 13. Contribution Guidelines

If this Manifesto is hosted in a public repository (e.g., on GitHub) and open to community contributions for its evolution, clear guidelines should be established. These might include:

- **How to Propose Changes:** (e.g., via Issues, Pull Requests).

- **Discussion Forums:** Where to discuss proposed changes or new features.

- **Versioning Strategy for Future Updates:** How new versions of the Manifesto will be numbered and released.

- **Code of Conduct:** Expectations for respectful and constructive collaboration among contributors.

- **Review Process:** How proposed changes are reviewed and accepted.

These guidelines help maintain the quality and coherence of the Manifesto as it evolves.

## 14. Frequently Asked Questions (FAQ)

Q1: Why are there two files (..._aitaskinput.json and ..._project_content.txt) for each delivery?

A1: The ..._aitaskinput.json (manifest) provides the instructions and metadata (the "what, where, and how"), while the ..._project_content.txt (content chunks file) provides the actual data (the "with what"). This separation keeps the instructional logic clean and allows for efficient handling of potentially large content.

Q2: What if the AI generates an aitaskinput.json my script doesn't understand?

A2: This is where versioning is key. The manifest_format_version in aitaskinput.json and the manifesto_versions_supported in the AI's capabilities (see Appendix B) help manage compatibility. Users should ensure their client script supports the Manifesto version used by the AI.

Q3: How are updates to existing files handled for non-code content?

A3: For prose or structured data where Git-style diff markers aren't ideal, the AI should provide the complete new content. The "AI Notes for This Run" section in the run log (part of ..._project_content.txt) should then clearly explain what changes were made compared to the previous version.

Q4: Can I use a tool other than PowerShell for the client-side script?

A4: Absolutely. This Manifesto defines the format of the input files. Users are free to implement a client script in any language (Python, Bash, JavaScript/Node.js, etc.) that can parse JSON and text files according to this Manifesto's specifications.

Q5: What is the purpose of the current_run_correlation_id?

A5: This ID uniquely identifies a specific delivery batch from the AI. It's included in the manifest and the run log. Users can refer to this ID when communicating with the AI about a particular set of delivered content, making it easier for the AI to recall context for follow-up requests or corrections related to that specific run.

Q6: Why do filenames for manifest and content files include a timestamp and unique ID?

A6: This convention (e.g., YYYYMMDD_HHMMSS_UNIQUEID_aitaskinput.json) helps in several ways:

- Easy Sorting: Files are sorted chronologically by default in most file explorers.

- Uniqueness: Prevents overwriting previous deliveries if saved in the same directory.

- Direct Correlation: Clearly pairs a specific manifest file with its corresponding content file.

Q7: How should the AI manage the overall list of pending tasks if aitaskinput.json only contains tasks for the current run?

A7: The AI agent is responsible for maintaining the master list of all pending tasks for the entire project or user request internally. For each delivery, it selects a subset of these tasks, generates the content, and creates an aitaskinput.json that only lists those tasks being delivered in that specific run. When the AI prepares the next delivery, it will consult its internal master list of remaining tasks.

## 15. Appendices

### 15.1. Appendix A: `aitaskinput.json` (v1.0) - Detailed Structure and Task Types

The `aitaskinput.json` file is the primary instruction set for the client-side script.

**Full Structure Example (v1.0):**

```
{
  "manifest_format_version": "1.0",
  "project_name": "Foundation Project Example",
  "description": "Illustrative v1.0 manifest.",
  "current_run_correlation_id": "run_20250601100000_xyz789",
  "solution_root_hint": "./project_output",
  "content_chunk_file": "20250601_100000_xyz789_project_content.txt",
  "ai_agent_profile": {
    "agent_name": "ManifestoCoreAI",
    "agent_version": "0.9.0",
    "capabilities_ref": "core_capabilities_v1"
  },
  "tasks": [
    {
      "task_id": "log_run_main",
      "type": "create_text_file",
      "file_path": "project_logs/20250601_100000_xyz789_ai_collab_run.log",
      "content_chunk_id": "run_log_content_main_v1"
    },
    {
      "task_id": "readme_init",
      "type": "create_text_file",
      "file_path": "README.md",
      "content_chunk_id": "readme_md_content_v1"
    },
    {
      "task_id": "update_script_core",
      "type": "update_text_file",
      "file_path": "src/core_logic.py",
      "content_chunk_id": "core_logic_py_update1"
    },
    {
      "task_id": "generate_project_overview",
      "type": "generate_prose", 
      "file_path": "docs/project_overview.md",
      "content_chunk_id": "project_overview_prose_v1",
      "output_format_hint": "markdown"
    },
    {
      "task_id": "generate_initial_config",
      "type": "generate_structured_data", 
      "file_path": "config/settings.json",
      "content_chunk_id": "settings_json_data_v1",
      "output_format_hint": "json_object"
    },
    {
      "task_id": "logo_asset_small",
      "type": "create_binary_file_base64",
      "file_path": "assets/logo_sm.png",
      "encoding": "base64",
      "content_chunk_id": "logo_sm_png_base64"
    },
    {
      "task_id": "large_archive_part1",
      "type": "create_binary_file_multipart_base64",
      "file_path": "archives/data_backup.zip",
      "encoding": "base64",
      "part_number": 1,
      "total_parts": 2,
      "assembly_id": "backup_zip_001",
      "content_chunk_id": "backup_zip_part1_base64"
    },
    {
      "task_id": "large_archive_part2",
      "type": "create_binary_file_multipart_base64",
      "file_path": "archives/data_backup.zip",
      "encoding": "base64",
      "part_number": 2,
      "total_parts": 2,
      "assembly_id": "backup_zip_001",
      "content_chunk_id": "backup_zip_part2_base64"
    },
    {
      "task_id": "project_file_guide_main",
      "type": "create_project_file_guide",
      "file_path": "MyApplication.csproj.txt",
      "content_chunk_id": "myapplication_csproj_guide"
    }
  ],
  "post_processing_commands": [
    "echo 'v1.0 Manifest processing complete.'"
  ],
  "agent_capabilities": {
    "core_capabilities_v1": {
        "manifesto_versions_supported": ["1.0"]
        // ... other capabilities as defined in Appendix B for v1.0 ...
    }
  }
}
```

**Task Object Fields (v1.0):**

- `task_id`: (String, Required) Unique identifier for the task within this manifest.

- `type`: (String, Required) The type of task. See "Task Types (v1.0)" below.

- `file_path`: (String, Required) Relative path (using `/`) for the output file.

- `content_chunk_id`: (String, Required for tasks needing content) ID of the content chunk in `..._project_content.txt`.

- `encoding`: (String, Optional) Primarily "base64" for binary tasks. Defaults to UTF-8 for text.

- `output_format_hint`: (String, Optional) Hint about the content format (e.g., "markdown", "json_object").

- **For `create_binary_file_multipart_base64` type only:**
  
  - `part_number`: (Integer, Required) Sequence number (1-indexed).
  
  - `total_parts`: (Integer, Required) Total number of parts.
  
  - `assembly_id`: (String, Required) Unique ID for all parts of the same file.

**Task Types (v1.0 Values for the `type` field):**

- `create_text_file`: Creates a new text file (e.g., code, plain text, run log). Overwrites if exists.

- `update_text_file`: Updates an existing text file. AI provides complete new content, potentially with Git-style diff markers for code.

- `generate_prose`: Generates textual content like summaries, articles, or detailed explanations (e.g., for `.md` or `.txt` files). This consolidates `generate_report_prose` and `recommendation_list` from later Manifesto versions for simplicity in v1.0.

- `generate_structured_data`: Generates data in structured formats like JSON, CSV. Content chunk contains the raw data.

- `create_binary_file_base64`: Creates a binary file from a single Base64 encoded string.

- `create_binary_file_multipart_base64`: Processes one part of a larger binary file split into multiple Base64 chunks. (Client script handles reassembly).

- `create_project_file_guide`: Provides content for project configuration files (e.g., `.csproj`) but saves them with a `.txt` extension to guide manual setup using native tools.

### 15.2. Appendix B: AI Agent Capabilities (v1.0) - Detailed Fields

The `agent_capabilities` object allows the AI agent to declare its operational parameters for Manifesto v1.0.

**Recommended Fields (v1.0):**

- `manifesto_versions_supported`: (Array of Strings, Required) Must include "1.0". (e.g., `["1.0"]`).

- `agent_name`: (String) Name of the AI agent.

- `agent_version`: (String) Version of the AI agent.

- `supported_task_types`: (Array of Strings, Required) Lists `type` values from Appendix A (Task Types v1.0) that the agent supports.

- `supported_output_formats`: (Array of Strings, Optional) Lists common `output_format_hint` values the agent can generate (e.g., `["text", "markdown", "json_object"]`).

- `max_single_file_size_text_kb`: (Integer, Optional) Max size for a text file content chunk.

- `max_base64_part_size_kb`: (Integer, Optional) Max size for each Base64 part in multipart.

- `supports_embedded_diff_markers`: (Boolean, Optional) True if agent can generate Git-style diff markers.

- `supports_dynamic_task_removal`: (Boolean, Required) True if agent correctly manages `tasks` in `aitaskinput.json` per run.

- `supports_run_log_generation`: (Boolean, Required) True if agent generates the run log as specified.

- `supports_run_correlation_id_and_notes`: (Boolean, Required) True if agent uses correlation ID and provides AI notes.

- `content_disclaimer_generation`: (Boolean, Optional) True if agent can include disclaimers.

- `source_attribution_capability`: (String, Optional) Describes source provision ability (e.g., "none", "best_effort_url_list").

- `data_privacy_policy_summary`: (String, Required) Brief summary or link to privacy policy.

- `sensitive_topic_handling_policy`: (String, Required) Policy on handling sensitive topics.

- `notes_for_user`: (String, Optional) General notes for the user.

This Manifesto is a living document, intended to evolve with the capabilities of AI and the needs of its users.
