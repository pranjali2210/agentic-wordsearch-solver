User–System Interaction Document
1. Overview

This document describes how users interact with the Agentic AI WordSearch Solver system. It defines expected behavior, happy paths, and edge cases before implementation.
  1.1 The system accepts:
  Image URL OR Base64 image of a word search puzzle
  
  List of target words

  1.2 The system returns:
  Word coordinates
  
  Highlighted HTML visualization

2. User Personas
  2.1 Persona 1: AI/ML Student (Primary User)

   Background: Computer Engineering / AI student

   Experience: Familiar with Langflow and LLMs

   Goal: Solve word search puzzles programmatically

   Access Level: Can provide image URLs or Base64 data

  2.2 Persona 2: Puzzle Enthusiast (Secondary User)

   Background: Non-technical

   Experience: Basic web interface usage

   Goal: Upload puzzle image and get highlighted solution

   Access Level: Only image upload and word list input

  2.3 Persona 3: Developer / Maintainer

   Background: AI Engineer

   Experience: Langflow and Groq API

   Goal: Extend or debug system

   Access Level: Full system modification

3. Happy Path Workflows
  3.1 Happy Path 1 – Image via URL
  Scenario

   User provides a public image URL and word list.

  User:
   "I want to solve this puzzle."

  Provides:

   Image URL

  Words: thor, hulk, hawkeye, black widow

  3.2 System Execution Flow
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

  3.3 Happy Path 2 – Base64 Image Input

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

1. System Overview

  The WordSearch Solver follows a multi-agent architecture where each agent has a clear responsibility. Agents communicate sequentially. No agent performs unrelated responsibilities.

  Each agent has a single responsibility.

2. Image Processing Agent

 2.1 Role

   Convert image input into processable format.

 2.2 Responsibilities

  Fetch image from URL

  Validate Base64 input

  Convert image to standardized format

  Return image bytes

 2.3 Capabilities

  Can read image

  Cannot perform search

  Cannot generate HTML

3. Grid Extraction Agent
  3.1 Role

   Extract and validate 2D letter grid.

  3.2 Responsibilities

   Convert image to text grid

   Format as 2D list

   Validate dimensions

   Ensure characters only

  3.3 Capabilities

   Returns 2D list

   Cannot modify word list

   Cannot generate HTML

4. WordSearch Engine Agent
  4.1 Role

   Perform precise directional scanning.

  4.2 Responsibilities

   Accept grid and word list

   Search in 8 directions

   Return coordinates

   Identify direction

  4.3 Capabilities

   Deterministic logic

   Precise coordinate detection

   Cannot fetch images

   Cannot generate HTML

5. HTML Generator Agent
  5.1 Role

   Convert grid and coordinates into styled HTML output.

  5.2 Responsibilities

   Create HTML table

   Apply .found class

   Highlight solved letters

   Generate CSS

  5.3 Capabilities

   HTML generation

   Cannot modify search logic

   Cannot fetch images
