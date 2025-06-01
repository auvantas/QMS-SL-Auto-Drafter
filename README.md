# QMS Auto-Drafter

**QMS Auto-Drafter** is a dynamic web application designed to guide users through the creation of Quality Management System (QMS) documentation. It utilizes a structured Question & Answer format, leverages the Google Gemini API for AI-assisted content drafting and refinement, and incorporates web search capabilities for enhanced information gathering.

The application helps organizations, particularly those aiming to align with standards like ISO 9001:2015, to systematically build foundational QMS documents such as Quality Manuals and Codes of Conduct.

## Core Features

*   **Guided Q&A:** Walks users through structured sections and questions relevant to QMS development.
*   **Company Information Management:** Centralized input for company details, automatically populating relevant sections of documents.
*   **AI-Powered Drafting (Gemini API):**
    *   **AI Draft Assist:** Generates draft content based on user answers and predefined prompts.
    *   **Strategic Refinement:** Helps refine user inputs, such as transforming a list of plans into a cohesive strategic direction.
    *   **AI Search Info:** Performs targeted web searches using Gemini to provide relevant examples, definitions, and regulatory information.
*   **Dynamic Document Compilation:** Generates QMS documents (e.g., Quality Manual, Code of Conduct) in Markdown format based on user inputs and AI suggestions.
*   **Interactive UI:**
    *   Clear navigation through different QMS sections.
    *   Modal pop-ups for AI-generated content and search results.
    *   Option to use, edit, or discard AI suggestions.
    *   Copy to clipboard and download functionality for generated documents.
*   **ISO 9001:2015 Information:** Provides a dedicated section with an overview of ISO 9001 principles and benefits.
*   **Responsive Design:** Built with Tailwind CSS for a responsive experience across devices.
*   **Context-Aware Help:** Provides help text and placeholders for questions, and guidance on distinguishing between plans and strategies.

## Technology Stack

*   **Frontend:** React, TypeScript
*   **Styling:** Tailwind CSS
*   **AI Integration:** `@google/genai` for Google Gemini API
*   **State Management:** React Context API
*   **Build/Development:** esbuild (via `index.html` script type module and esm.sh for CDN imports)

## Project Structure

A brief overview of key files and directories:

*   `index.html`: The main HTML file, includes Tailwind CSS CDN and import maps.
*   `index.tsx`: The entry point for the React application.
*   `App.tsx`: The main application component, handling routing and layout.
*   `components/`: Contains all React components (e.g., `SidebarComponent`, `QuestionComponent`, `DocumentViewComponent`).
*   `contexts/`:
    *   `QmsContext.tsx`: Manages the global state of the application (user answers, AI suggestions, company info).
*   `services/`:
    *   `geminiService.ts`: Handles all interactions with the Google Gemini API.
*   `constants.ts`: Defines the structure of QMS sections, questions, document templates, and other static data.
*   `types.ts`: Contains TypeScript type definitions used throughout the application.
*   `metadata.json`: Contains metadata about the application, including permissions.

## Prerequisites

*   A modern web browser.
*   A Google Gemini API Key.

## Getting Started

### 1. Environment Configuration (API Key)

The application requires a Google Gemini API key to function correctly. This key **must** be provided as an environment variable.

**Important:** The application is designed to run in an environment where `process.env.API_KEY` is already set. The application itself does **not** provide a UI for entering the API key.

If you are running this project in an environment that supports setting environment variables (e.g., a Node.js server environment serving the static files, or a platform like Vercel/Netlify), ensure `API_KEY` is set there.

**For local development where you might be serving `index.html` directly or using a simple static server that doesn't easily inject `process.env` into client-side JavaScript:**

The current setup using `esm.sh` and direct browser modules means `process.env.API_KEY` as used in `geminiService.ts` might not be directly resolvable in a purely client-side static hosting scenario without a build step.

To make it work in such a scenario, you would typically:
1.  Introduce a build step (e.g., Vite, Create React App, Parcel) that can replace `process.env.API_KEY` with an actual key from a `.env` file during the build process.
2.  Or, for a simplified local setup **not suitable for production**, you might temporarily modify `geminiService.ts` to directly use a key (again, **not recommended for production or version control**).

**Assuming an environment where `process.env.API_KEY` is accessible:**

No further client-side configuration is needed for the API key beyond ensuring the environment variable is set in the execution context where the JavaScript that initializes `GoogleGenAI` runs.

### 2. Running the Application

1.  Ensure you have a web server capable of serving static files.
2.  Place the project files (`index.html`, `index.tsx`, and other related `.ts`, `.tsx` files) in the web server's root directory or a subdirectory.
3.  Access `index.html` through your web browser via the web server.

    *Example using a simple local server (if you have Node.js and npm):*
    ```bash
    # Install a simple HTTP server globally (if you haven't already)
    npm install -g http-server

    # Navigate to your project directory
    cd path/to/your/qms-auto-drafter

    # Start the server
    http-server .
    ```
    Then open the provided URL (e.g., `http://localhost:8080`) in your browser.

## Using the Application

1.  **Company Information:** Upon first launch, you'll be directed to the "Company Information" page. Fill this out, as it's used to pre-populate your documents.
2.  **Dashboard:** Provides an overview and quick links.
3.  **Navigate Sections:** Use the sidebar to select a QMS section (e.g., "Laying the Foundation").
4.  **Answer Questions:** Fill in the text areas for each question. Help text and placeholders are provided.
5.  **Use AI Features:**
    *   **AI Draft Assist:** Click this button to have Gemini generate a draft answer or content for the current question.
    *   **AI Search Info:** Click this to get Gemini to search the web for information related to the question or your current answer. This can include examples, definitions, or best practices.
    *   **Refine to Strategy (for specific questions):** If your input for strategic direction seems more like a plan, an AI option will appear to help you rephrase it strategically.
6.  **Review AI Suggestions:** AI-generated content and search results will appear in a modal. You can choose to "Use this Draft" (which copies it into your answer field) or close the modal.
7.  **View Generated Documents:** Navigate to "View Generated Documents" to see a list of available QMS documents. Click to compile and view a document based on your current answers and AI suggestions. You can copy the content or download it as a Markdown file.
8.  **ISO Info:** Visit the "About ISO 9001:2015" section for general information on the standard.

## Customizing QMS Content

The core QMS structure (sections, questions, document templates, AI prompts) is defined in `constants.ts`. You can modify this file to:

*   Add, remove, or edit QMS sections.
*   Change question text, help text, placeholders.
*   Update `aiPromptTemplate` functions to tailor the prompts sent to the Gemini API for drafting.
*   Update `searchQueryTemplate` functions to refine AI-powered web searches.
*   Modify `DocumentDefinition` objects, including their `compileContent` functions, to change the structure and content of generated documents.

## AI Integration (Google Gemini API)

This application heavily relies on the Google Gemini API for its intelligent features.

*   **`geminiService.ts`:** This service encapsulates all API calls to Gemini.
    *   `draftContent`: Uses the `gemini-2.5-flash-preview-04-17` model for general text generation.
    *   `searchWithGemini`: Uses the `gemini-2.5-flash-preview-04-17` model with Google Search grounding enabled to fetch and summarize web information, including sources.
*   **API Key:** Access to the Gemini API is controlled by an API key. **Ensure `process.env.API_KEY` is correctly set in your deployment environment.** Without a valid API key, AI features will be disabled or return errors.

## Contributing

Contributions are welcome! If you'd like to contribute, please consider:

1.  Forking the repository.
2.  Creating a new branch for your feature or bug fix.
3.  Making your changes.
4.  Testing your changes thoroughly.
5.  Submitting a pull request with a clear description of your changes.

## License

This project is open-source. GNU General Public License version 3.0 (GPLv3).

---
