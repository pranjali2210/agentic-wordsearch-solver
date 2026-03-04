User–System Interaction Document
1. Overview

This document describes how users interact with the Agentic AI WordSearch Solver system.
It defines expected behavior, happy paths, and edge cases before implementation.

1.1 System Inputs

The system accepts:

Image URL of a word search puzzle
OR

Base64 encoded image

Additionally, the user must provide:

List of target words

1.2 System Outputs

The system returns:

Word coordinates in the grid

Highlighted HTML visualization of the solved puzzle

Structured JSON output

2. User Personas
2.1 Persona 1 – AI/ML Student (Primary User)

Background: Computer Engineering / AI student
Experience: Familiar with Langflow and LLMs
Goal: Solve word search puzzles programmatically
Access Level: Can provide image URLs or Base64 image data

2.2 Persona 2 – Puzzle Enthusiast (Secondary User)

Background: Non-technical
Experience: Basic web interface usage
Goal: Upload puzzle image and get highlighted solution
Access Level: Only image upload and word list input

2.3 Persona 3 – Developer / Maintainer

Background: AI Engineer
Experience: Langflow and Groq API
Goal: Extend, debug, or optimize the system
Access Level: Full system modification

3. Happy Path Workflows
3.1 Happy Path 1 – Image via URL
Scenario

User provides a public image URL and a word list.

User Request

"I want to solve this puzzle."

Input Provided

Image URL

Words: thor, hulk, hawkeye, black widow

3.2 System Execution Flow
Thinking Step 1 – Input Validation

Validate URL format

Check accessibility of the image

Action 1

Fetch image from URL

Thinking Step 2 – Image Processing

Convert image to Base64

Pass image to Grid Extraction Agent

Action 2

Extract letter grid from image

Convert grid to 2D array

Thinking Step 3 – Grid Validation

Validate grid structure

Ensure it is a proper 2D list

Action 3

Send:

Grid

Word list

to WordSearch Engine

Thinking Step 4 – Word Search

Perform deterministic grid scanning

Detect word coordinates

Store positions

Action 4 – Structured Output

Example:

{
  "word": "thor",
  "positions": [[0,0], [0,1], [0,2], [0,3]],
  "direction": "horizontal"
}
Action 5 – HTML Generation

Generate HTML table

Apply .found CSS class

Highlight solved letters

Final Response to User
"Here is your solved puzzle."

Returns:

Highlighted HTML grid

JSON with word positions

3.3 Happy Path 2 – Base64 Image Input
User Input

User uploads Base64 encoded image

System Flow

Validate Base64

Decode image

Extract grid

Run word search

Generate HTML visualization

Return solution

Agent Specification Document
1. System Overview

The WordSearch Solver follows a multi-agent architecture.

Key principles:

Each agent has one clear responsibility

Agents communicate sequentially

No agent performs unrelated tasks

2. Image Processing Agent
2.1 Role

Convert image input into a processable format.

2.2 Responsibilities

Fetch image from URL

Validate Base64 input

Convert image into standardized format

Return image bytes

2.3 Capabilities

✔ Can read images
✘ Cannot perform word search
✘ Cannot generate HTML

3. Grid Extraction Agent
3.1 Role

Extract and validate the 2D letter grid.

3.2 Responsibilities

Convert image to text grid

Format grid as 2D list

Validate grid dimensions

Ensure characters only

3.3 Capabilities

✔ Returns validated 2D list
✘ Cannot modify word list
✘ Cannot generate HTML

4. WordSearch Engine Agent
4.1 Role

Perform precise directional scanning of the grid.

4.2 Responsibilities

Accept grid and word list

Search in 8 directions

Return coordinates

Identify word direction

4.3 Capabilities

✔ Deterministic search logic
✔ Precise coordinate detection
✘ Cannot fetch images
✘ Cannot generate HTML

5. HTML Generator Agent
5.1 Role

Convert grid and coordinates into styled HTML output.

5.2 Responsibilities

Create HTML table

Apply .found class

Highlight solved letters

Generate CSS styles
