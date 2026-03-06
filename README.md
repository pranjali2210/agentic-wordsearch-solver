# Agentic AI WordSearch Solver

A multi-agent system for solving word search puzzles from images — returning highlighted HTML visualizations and structured JSON output.

---

## Table of Contents

- [Overview](#overview)
- [System I/O](#system-io)
- [User Personas](#user-personas)
- [Happy Path Workflows](#happy-path-workflows)
- [Agent Architecture](#agent-architecture)

---

## Overview

The WordSearch Solver accepts an image of a word search puzzle along with a list of target words, then uses a pipeline of specialized agents to extract the grid, locate each word, and return a visual solution.

---

## System I/O

### Inputs

| Type | Description |
|------|-------------|
| `image_url` | Publicly accessible URL of the puzzle image |
| `base64_image` | Base64-encoded puzzle image |
| `word_list` | List of words to locate in the puzzle |

### Outputs

| Type | Description |
|------|-------------|
| `coordinates` | Grid positions for each found word |
| `html_visualization` | Highlighted HTML table of the solved puzzle |
| `json_output` | Structured JSON with word positions and directions |

---

## User Personas

### 🎓 Persona 1 — AI/ML Student *(Primary)*

| Field | Detail |
|-------|--------|
| Background | Computer Engineering / AI student |
| Experience | Familiar with Langflow and LLMs |
| Goal | Solve word search puzzles programmatically |
| Access | Image URLs or Base64 image data |

### 🧩 Persona 2 — Puzzle Enthusiast *(Secondary)*

| Field | Detail |
|-------|--------|
| Background | Non-technical |
| Experience | Basic web interface usage |
| Goal | Upload puzzle image and get highlighted solution |
| Access | Image upload and word list input only |

### 🛠️ Persona 3 — Developer / Maintainer

| Field | Detail |
|-------|--------|
| Background | AI Engineer |
| Experience | Langflow and Groq API |
| Goal | Extend, debug, or optimize the system |
| Access | Full system modification |

---

## Happy Path Workflows

### Path 1 — Image via URL

**User provides** a public image URL and a word list.

> *"I want to solve this puzzle."*

**Example input:**
```
Image URL: https://example.com/puzzle.png
Words:     thor, hulk, hawkeye, black widow
```

**Execution flow:**

```
Step 1 — Input Validation
  ├── Validate URL format
  └── Check image accessibility

Step 2 — Image Processing
  ├── Fetch image from URL
  ├── Convert to Base64
  └── Pass to Grid Extraction Agent

Step 3 — Grid Validation
  ├── Extract letter grid
  └── Convert to validated 2D array

Step 4 — Word Search
  ├── Scan grid in 8 directions
  └── Record coordinates per word

Step 5 — Output Generation
  ├── Build HTML table with highlights
  └── Return JSON + HTML to user
```

**Example JSON output:**
```json
{
  "word": "thor",
  "positions": [[0,0], [0,1], [0,2], [0,3]],
  "direction": "horizontal"
}
```

**Final response includes:**
- ✅ Highlighted HTML grid
- ✅ JSON with word positions

---

### Path 2 — Base64 Image Input

**User provides** a Base64-encoded image directly.

**Execution flow:**

```
1. Validate Base64 string
2. Decode image
3. Extract grid → run word search
4. Generate HTML visualization
5. Return solution
```

---

## Agent Architecture

The system follows a **multi-agent pipeline** where each agent has a single, well-defined responsibility. Agents communicate sequentially and never perform tasks outside their scope.

---

### 🖼️ Image Processing Agent

**Role:** Convert image input into a processable format.

| Responsibilities | Capabilities |
|-----------------|--------------|
| Fetch image from URL | ✅ Read images |
| Validate Base64 input | ❌ Cannot perform word search |
| Convert to standardized format | ❌ Cannot generate HTML |
| Return image bytes | |

---

### 🔡 Grid Extraction Agent

**Role:** Extract and validate the 2D letter grid from the image.

| Responsibilities | Capabilities |
|-----------------|--------------|
| Convert image to text grid | ✅ Returns validated 2D list |
| Format grid as 2D list | ❌ Cannot modify word list |
| Validate grid dimensions | ❌ Cannot generate HTML |
| Ensure characters only | |

---

### 🔎 WordSearch Engine Agent

**Role:** Perform precise directional scanning of the grid.

| Responsibilities | Capabilities |
|-----------------|--------------|
| Accept grid and word list | ✅ Deterministic search logic |
| Search in 8 directions | ✅ Precise coordinate detection |
| Return coordinates per word | ❌ Cannot fetch images |
| Identify word direction | ❌ Cannot generate HTML |

---

### 🎨 HTML Generator Agent

**Role:** Convert grid and coordinates into styled HTML output.

| Responsibilities | Capabilities |
|-----------------|--------------|
| Create HTML table | ✅ Generates styled output |
| Apply `.found` CSS class | ✅ Highlights solved letters |
| Highlight solved letters | |
| Generate CSS styles | |

<img width="1339" height="816" alt="Image" src="https://github.com/user-attachments/assets/78ff24e7-3980-477c-83ac-59e5ef7b1a6f" />

