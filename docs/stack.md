# Tech Stack

This document provides a breakdown of the technologies used to build CareerCraft AI, along with the reasoning for each choice.

## Core Technologies

-   **Framework**: [Next.js](https://nextjs.org/) (v15)
    -   **Why**: The App Router provides a powerful foundation for building modern web applications. Server Components by default lead to faster page loads, and Server Actions simplify client-server communication, creating a more cohesive development experience.
    -   **Alternatives Considered**: Vite + React. While faster for pure client-side apps, Next.js provides a more integrated full-stack solution out of the box.

-   **Language**: [TypeScript](https://www.typescriptlang.org/)
    -   **Why**: TypeScript enhances code quality and maintainability by adding static types to JavaScript. This was crucial for defining clear data structures for AI model inputs/outputs and for ensuring type safety between the client and server.

-   **Styling**: [Tailwind CSS](https://tailwindcss.com/)
    -   **Why**: A utility-first CSS framework that allows for rapid UI development without writing custom CSS. It enables the creation of a consistent design system directly within the markup.

-   **UI Components**: [ShadCN UI](https://ui.shadcn.com/)
    -   **Why**: ShadCN provides beautifully designed, accessible, and unstyled components that can be copied directly into the project. This gives full control over the code and styling, avoiding the "black box" nature of traditional component libraries.

## Backend & AI

-   **Backend-as-a-Service (BaaS)**: [Firebase](https://firebase.google.com/)
    -   **Why**: Firebase provides a comprehensive suite of tools that are easy to set up and scale.
        -   **Authentication**: Secure, out-of-the-box support for social providers like Google, handling all the complexities of session management.
        -   **Firestore**: A scalable NoSQL database with real-time capabilities, perfect for keeping user data synchronized between the client and server.
        -   **App Hosting**: A managed, serverless platform for hosting full-stack web applications with features like automatic CI/CD, SSL, and global CDN.

-   **AI Orchestration**: [Genkit](https://firebase.google.com/docs/genkit)
    -   **Why**: Genkit is an open-source framework that simplifies the development of AI-powered features. It allows for defining self-contained "flows" that handle prompting, structured output, and error handling, decoupling the AI logic from the main application code. This makes the AI features more modular, testable, and maintainable.

-   **Generative AI Model**: [Google Gemini](https://deepmind.google.com/technologies/gemini/)
    -   **Why**: Gemini models are highly capable, especially at following complex instructions and generating structured JSON output. The ability to define an output schema with Zod and have the model adhere to it is a game-changing feature that eliminates the need for fragile text parsing.

## Key Libraries & Integrations

-   **Payments**: [Paystack](https://paystack.com/)
    -   **Why**: A popular and developer-friendly payment processor. The `react-paystack` library simplifies the client-side integration, and the webhook support is robust for secure server-side payment confirmation.

-   **File Parsing (Client-Side)**:
    -   `pdf.js`: For parsing `.pdf` files directly in the browser.
    -   `mammoth`: For extracting raw text from `.docx` files in the browser.
    -   **Why**: Handling file parsing on the client-side is a critical privacy feature. It ensures that users' sensitive resume files are never uploaded to a server.

-   **PDF Generation**: `react-to-print`
    -   **Why**: A simple but powerful library that captures a React component and prepares it for printing or saving as a PDF, enabling the high-fidelity export of resume templates.

-   **Schema Validation**: [Zod](https://zod.dev/)
    -   **Why**: Used extensively with Genkit to define the input and output schemas for AI flows. This ensures that the data flowing into and out of the AI model is always in the correct shape, preventing runtime errors.
