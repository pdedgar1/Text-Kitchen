# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Text-Kitchen is a suite of browser-based text manipulation tools designed for creative writers to remix, analyze, and transform text. The project consists of standalone HTML pages with embedded JavaScript that process .txt files entirely client-side.

## Development Environment

This is a **static website with no build process, dependencies, or package manager**. All files are served directly to the browser:

- No `npm install`, `build`, or `test` commands exist
- No transpilation or bundling required
- Simply open HTML files in a browser to test locally
- Alternatively, use a local server: `python -m http.server 8000`

## Deployment

The site deploys automatically to GitHub Pages via Jekyll when changes are pushed to the `main` branch. The GitHub Actions workflow (`.github/workflows/jekyll-docker.yml`) handles this using a Docker container.

## Code Architecture

### Single-File Pattern
Each tool is completely self-contained in its own HTML file with three sections:
1. **Navigation bar** - Links to all tools (update when adding new tools)
2. **UI markup** - File upload controls, options, and output display
3. **Embedded `<script>` tag** - All JavaScript for that tool

### Common Technical Patterns

**File Upload Handling:**
All tools use the FileReader API to load .txt files client-side:
```javascript
const reader = new FileReader();
reader.onload = function(e) {
    const content = e.target.result;
    // Process content...
};
reader.readAsText(file);
```

**Text Processing Pipeline:**
1. User uploads .txt file via `<input type="file" accept=".txt">`
2. FileReader loads content into JavaScript
3. Tool-specific algorithms transform the text
4. Results display in a dedicated output section
5. Some tools show statistics (word count, frequency, etc.)

**State Management:**
Each tool stores state in module-level variables:
- `originalText` or `fileContent` - The uploaded file content
- Mode/option flags - Track current user selections
- Processed results - Cached transformations

### Shared Styles

`styles.css` provides common components:
- `.main-nav` - Navigation bar styling
- `.container` - Page layout wrapper
- `.file-input` - File upload button styling
- `.slider`, `.checkbox-container` - Form control styles
- `.output-section`, `.results-section` - Output display areas

## Tool Descriptions

1. **Eraser Lab** (`eraser-lab.html`) - Permutational erasure: removes every nth word or character
2. **Line Break Lab** (`line-break-lab.html`) - Inserts line breaks based on specific characters/strings
3. **Alphabetizer Lab** (`alphabetizer-lab.html`) - Sorts words alphabetically with frequency analysis
4. **Text Reversal Lab** (`text-reverse-lab.html`) - Reverses word/sentence order
5. **Markov Mashup Lab** (`markov-mashup-lab.html`) - Generates new text using Markov chains

## Adding New Tools

When creating a new tool:

1. Copy the structure from an existing tool HTML file
2. Update the `<title>` tag
3. Ensure the navigation bar includes the new tool (update ALL files' nav bars)
4. Implement the file upload handler using the FileReader pattern
5. Add tool-specific processing logic in the `<script>` section
6. Use existing CSS classes from `styles.css` for consistency
7. Update `index.html` to include the new tool in the landing page grid

## Context for AI Assistance

This is a **creative writing tool**, not a standard web application:
- Text transformations are intentionally experimental/artistic
- "Bugs" may actually be creative features (verify intent with author)
- Large text files (novels, corpora) are expected inputs
- Performance matters: tools should handle 100k+ word documents
- Output is meant for human creative exploration, not machine processing

The audience is poets, writers, and digital artists exploring computational text manipulation.
