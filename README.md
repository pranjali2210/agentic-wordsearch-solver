# agentic-wordsearch-solver
Agentic AI WordSearch Solver built using Langflow and Groq with modular multi-agent architecture.
User–System Interaction Document
Overview

This document describes how users interact with the Agentic AI WordSearch Solver system. It defines expected behavior, happy paths, and edge cases before implementation.

The system accepts:

Image URL OR Base64 image of a word search puzzle

List of target words

The system returns:

Word coordinates

Highlighted HTML visualization

User Personas
Persona 1: AI/ML Student (Primary User)

Background: Computer Engineering / AI student
Experience: Familiar with Langflow and LLMs
Goal: Solve word search puzzles programmatically
Access Level: Can provide image URLs or Base64 data
Persona 2: Puzzle Enthusiast (Secondary User)

Background: Non-technical
Experience: Basic web interface usage
Goal: Upload puzzle image and get highlighted solution
Access Level: Only image upload and word list input

Persona 3: Developer / Maintainer

Background: AI Engineer
Experience: Langflow, Groq API
Goal: Extend or debug system
Access Level: Full system modification

Happy Path Workflows
Happy Path 1 – Image via URL
Scenario

User provides a public image URL and word list.

User:
"I want to solve this puzzle."

Provides:

Image URL

Words: thor, hulk, hawkeye, black widow

System Execution Flow

Thinking Step 1

Validate URL format

Check accessibility of image

Action 1

Fetch image from URL

Thinking Step 2

Convert image to Base64

Pass image to Grid Extraction Agent

Action 2

Extract letter grid from image

Convert to 2D array

Thinking Step 3

Validate grid structure

Ensure it is a proper 2D list

Action 3

Send grid and word list to WordSearch Engine

Thinking Step 4

Perform deterministic grid scanning

Detect word coordinates

Store positions

Action 4

Generate structured output:

{
  "word": "thor",
  "positions": [[0,0],[0,1],[0,2],[0,3]],
  "direction": "horizontal"
}

Action 5

Generate HTML table

Apply .found CSS class

Highlight letters

Response to User

"Here is your solved puzzle."

Returns:

Highlighted HTML

JSON with word positions

Happy Path 2 – Base64 Image Input

User:

Uploads Base64 encoded image

System:

Validates Base64

Decodes image

Extracts grid

Runs search

Generates HTML

Returns result

Agent Specification Document
System Overview

The WordSearch Solver follows a multi-agent architecture where each agent has a clear responsibility. Agents communicate sequentially. No agent performs unrelated responsibilities.

Each agent has a single responsibility.

Image Processing Agent
Role

Convert image input into processable format.

Responsibilities

Fetch image from URL

Validate Base64 input

Convert image to standardized format

Return image bytes

Capabilities

Can read image

Cannot perform search

Cannot generate HTML

Grid Extraction Agent
Role

Extract and validate 2D letter grid.

Responsibilities

Convert image to text grid

Format as 2D list

Validate dimensions

Ensure characters only

Capabilities

Returns 2D list

Cannot modify word list

Cannot generate HTML

WordSearch Engine Agent
Role

Perform precise directional scanning.

Responsibilities

Accept grid and word list

Search in 8 directions

Return coordinates

Identify direction

Capabilities

Deterministic logic

Precise coordinate detection

Cannot fetch images

Cannot generate HTML

HTML Generator Agent
Role

Convert grid and coordinates into styled HTML output.

Responsibilities

Create HTML table

Apply .found class

Highlight solved letters

Generate CSS

Capabilities

HTML generation

Cannot modify search logic

Cannot fetch images
