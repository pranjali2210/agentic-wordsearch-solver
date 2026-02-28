# agentic-wordsearch-solver
Agentic AI WordSearch Solver built using Langflow and Groq with modular multi-agent architecture.
1️. User-System Interaction Document
 1. Overview
    This document describes how users interact with the Agentic AI WordSearch Solver system. It defines expected behavior, happy paths, and edge cases before implementation.
The system accepts:
a. Image URL OR Base64 image of wordsearch
b. List of target words
It returns:
a. Word coordinates
b. Highlighted HTML visualization

2. User Personas
Persona 1: AI/ML Student (Primary User)
Background: Computer Engineering / AI student
Experience: Familiar with Langflow and LLMs
Goal: Solve wordsearch puzzles programmatically
Access Level: Can provide image URLs or Base64 data

Persona 2: Puzzle Enthusiast (Secondary User)
Background: Non-technical
Experience: Basic web interface usage
Goal: Upload puzzle image and get highlighted solution
Access Level: Only image upload + word list input

Persona 3: Developer / Maintainer
Background: AI Engineer
Experience: Langflow, Groq API
Goal: Extend or debug system
Access Level: Full system modification

3. Happy Path Workflows
Happy Path 1 – Image via URL
Scenario: User provides a public image URL and word list.
User: "I want to solve this puzzle."
Provides: Image URL
Words: thor, hulk, hawkeye, black widow
System (Thinking Step 1): Validate URL format. Check accessibility of image
System (Action 1): Fetch image from URL
System (Thinking Step 2): Convert image to Base64. Pass image to Grid Extraction Agent
System (Action 2): Extract letter grid from image. Convert to 2D array
System (Thinking Step 3): Validate grid structure. Ensure it is a proper 2D list
System (Action 3): Send grid + word list to WordSearch Engine
System (Thinking Step 4): Perform deterministic grid scanning. Detect word coordinates. Store positions
System (Action 4):
Generate structured output:
{
  "word": "thor",
  "positions": [[0,0],[0,1],[0,2],[0,3]],
  "direction": "horizontal"
}
System (Action 5): Generate HTML table. Apply .found CSS class. Highlight letters
System (Response to User): "Here is your solved puzzle."
Returns:Highlighted HTML. JSON with word positions

Happy Path 2 – Base64 Image Input
User: Uploads Base64 encoded image.
System: Validates Base64. Decodes image. Extracts grid. Runs search. Generates HTML. Returns result

2️. Agent Specification Document

WordSearch Solver – Agent Specification
System Overview:
The WordSearch Solver follows a multi-agent architecture where each agent has a clear responsibility.
Agents communicate sequentially.
No agent performs unrelated responsibilities.
 1. Image Processing Agent
    Role:
    Convert image input into processable format.
    Responsibilities:
    a. Fetch image from URL
    b. Validate Base64
    c. Convert image to standardized format
    d. Return image bytes
    Capabilities:
    a. Read image
    b. Cannot perform search
    c. Cannot generate HTML
 2. Grid Extraction Agent
    Role:
    Extract and validate 2D letter grid.
    Responsibilities:
    a. Convert image to text grid
    b. Format as 2D list
    c. Validate dimensions
    d. Ensure characters only
    Capabilities:
    a. Return 2D list
    b. Cannot modify word list
    c. Cannot generate HTML
 3. WordSearch Engine Agent and HTML Generator Agent
    Role:
    Perform precise directional scanning.
    Convert grid + coordinates into styled HTML.
    Responsibilities:
    a. Accept grid + word list
    b. Search in 8 directions
    c. Return coordinates
    d. Identify direction
    e. Create <table>
    f. Apply .found class
    g. Highlight solved letters
    h. Generate CSS
    Capabilities:
    a. Deterministic logic
    b. Precise coordinate detection
    c. HTML generation


Planner Agent cannot modify grid structure.

Each agent has a single responsibility.
