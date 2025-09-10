# CareerCraft AI - Application Documentation

## 1. Overview

CareerCraft AI is a sophisticated web application designed to help users create, analyze, and optimize their resumes. By leveraging the power of Google's Gemini generative AI through the Genkit framework, the app provides users with intelligent feedback, content enhancements, and professional templates to craft the perfect resume for their target job.

The application is built as a guided, multi-step wizard to ensure a simple and intuitive user experience, from uploading a resume to exporting a polished, job-ready document.

---

## 2. Core Features

-   **Firebase Integration**: Full backend support with Firebase Authentication for user management and Firestore for data storage (profiles, usage, subscriptions).
-   **Multi-Step Resume Building**: A guided workflow that takes the user from resume import to final export.
-   **Local File Parsing**: Supports parsing resumes directly in the browser from various formats (`.pdf`, `.docx`, `.txt`). Files are never uploaded to a server, ensuring user privacy.
-   **AI-Powered Comprehensive Review**: A core feature (`reviewAndImprove` flow) that provides a holistic analysis of the resume, including a quality score, key recommendations, content analysis, ATS compatibility, and job description matching.
-   **Interactive Results Panel**: A rich, tabbed interface to explore the AI's feedback.
-   **Professional Resume Templates**: A library of professionally designed templates to preview and export the final resume.
-   **PDF Export**: Users can export their enhanced resume using the selected template as a print-ready PDF.
-   **Subscription & Usage Management**: A real subscription system tied to Firestore, managing user plans (Free, Starter, Pro) and daily usage allowances.
-   **Admin Dashboard**: A dedicated dashboard for administrators to view user analytics, subscription trends, and usage statistics.

---

## 3. Tech Stack

-   **Framework**: [Next.js](https://nextjs.org/) (with App Router)
-   **Language**: [TypeScript](https://www.typescriptlang.org/)
-   **Styling**: [Tailwind CSS](https://tailwindcss.com/)
-   **UI Components**: [ShadCN UI](https://ui.shadcn.com/)
-   **Generative AI**: [Google Gemini](https://deepmind.google.com/technologies/gemini/)
-   **AI Orchestration**: [Genkit](https://firebase.google.com/docs/genkit)
-   **Backend & Deployment**: [Firebase](https://firebase.google.com/) (Authentication, Firestore, App Hosting)
-   **Payments**: [Paystack](https://paystack.com/)

---

## 4. Application Architecture

CareerCraft AI is built on a modern, serverless architecture that leverages the power of Next.js and Firebase for a scalable and efficient system. The architecture prioritizes client-side rendering for a rich, interactive experience while offloading heavy computation and AI tasks to server-side components.

<img src="https://placehold.co/800x350.png" alt="Application Architecture Diagram" data-ai-hint="architecture diagram cloud" />

1.  **Frontend (Client - Next.js/React)**:
    *   **Responsibility**: Manages the user interface and experience. This includes a multi-step wizard for resume processing, a guided builder for creating resumes from scratch, and an interactive results panel for viewing AI feedback.
    *   **Technology**: Built with React, Next.js App Router, ShadCN UI, and Tailwind CSS.
    *   **State Management**: Uses React hooks (`useState`, `useContext`). A central `UserProvider` (`src/hooks/useUser.ts`) manages the global authentication state and user data from Firestore, making it available throughout the app.
    *   **Communication**: Invokes Server Actions to communicate with the AI backend. It uses the Firebase SDK for direct interaction with Auth and for real-time data updates from Firestore.
    *   **Privacy**: All file parsing (`.pdf`, `.docx`) is handled locally in the user's browser; files are never uploaded to a server.

2.  **Backend (Firebase & Genkit)**:
    *   **Authentication**: Firebase Authentication handles user sign-up, sign-in (Google provider), and session management. The `useUser` hook provides a real-time listener to the auth state.
    *   **Database**: Firestore stores all persistent data in a `users` collection, which includes profile information, subscription status, and daily AI usage counts. This data is kept in sync with the client through real-time listeners in the `useUser` hook.
    *   **AI Orchestration (Genkit)**: Genkit is the core of the AI functionality, managing all interactions with the Google Gemini LLM. The key Genkit flows, located in `src/ai/flows/`, are invoked by the client via Server Actions. These flows are simple wrappers that expose the AI capabilities to the Next.js application.

### Data Flow Example: Generating a Review

This is the most critical data flow in the application.

1.  **User Action**: The user clicks the "Generate Full Review" button in the `app/app/page.tsx` component.
2.  **Permission Check**: On the client, the `useUser` hook checks if the user's `creditsToday` count is less than their plan's `creditAllowance`. This check is performed against local state, which is kept in sync with Firestore in real-time.
3.  **Server Action Invocation**: If permitted, the client calls the `reviewAndImprove` Server Action, passing the resume text and job description. This function is directly imported from `src/ai/flows/review-and-improve.ts`.
4.  **AI Flow Triggered**: The Server Action, running on the server, invokes the `reviewAndImproveFlow` Genkit flow with the provided inputs.
5.  **Gemini API Call**: The Genkit flow constructs a detailed prompt and sends the data to the Google Gemini model.
6.  **Structured Response**: Gemini returns a structured JSON object containing the full analysis and the enhanced resume text, as defined by the flow's output schema (`ReviewAndImproveOutput`).
7.  **Database Update**: Upon receiving a successful response from the AI, the client-side logic calls the `decrementCredits` function from the `useUser` hook. This function updates the user's `creditsToday` counter in their Firestore document atomically.
8.  **UI Update**: The structured response is returned to the client. The client's state is updated, which causes the `ResultsPanel` component to render with the new data.

---

## 5. Project Structure

```
.
├── public/           # Static assets (images, fonts, print.css)
├── src/
│   ├── ai/           # All Genkit-related code
│   │   └── flows/    # Genkit flows exposed as Server Actions
│   ├── app/          # Next.js App Router pages and layouts
│   ├── components/   # Reusable React components
│   ├── hooks/        # Custom React hooks (useUser.ts, usePaystack.ts)
│   ├── lib/          # Utility functions and libraries
│   │   └── firebase/ # Firebase configuration and client SDK
└── ...               # Config files (next.config.ts, tailwind.config.ts, etc.)
```

---

## 6. AI Credit System

To balance operational costs with user value, CareerCraft AI uses a credit-based system. Instead of tracking confusing "token counts," each major AI-powered action consumes a set number of "AI Credits." This provides users with a clear and simple way to understand their usage.

The cost for each feature is determined by three factors:
1.  **Actual Cost**: The complexity of the request sent to the AI model (Google Gemini) and the size of the response directly impact the real-world cost. More complex tasks use more "tokens" and are therefore more expensive.
2.  **User Value**: Features that provide more comprehensive analysis or save the user more time are considered more valuable.
3.  **Simplicity**: The credit system abstracts away the technical details of token counts into a simple, easy-to-understand metric.

### Credit Cost Breakdown:

#### 1. Full Resume Review: **10 Credits** (Highest Cost)
-   **Relevant Flow**: `src/ai/flows/review-and-improve.ts`
-   **Reasoning**: This is the most complex and costly operation in the application.
    -   **Massive Input**: The request includes the user's entire resume and potentially a long job description.
    -   **Complex Instructions**: The prompt instructs the AI to perform multiple, distinct analyses in one go (overall score, content analysis, job match, ATS score, etc.) and to completely rewrite the resume.
    -   **Large, Structured Output**: The AI must generate a large, multi-level JSON object and the full enhanced resume text. This requires significant processing and results in a high token count.

#### 2. AI Cover Letter Generation: **5 Credits** (Medium Cost)
-   **Relevant Flow**: `src/ai/flows/generate-cover-letter.ts`
-   **Reasoning**: This feature provides high value but is less computationally intensive than the full review.
    -   **Large Input**: It also uses the full resume and job description as context.
    -   **Creative Task**: The AI performs a creative writing task to synthesize information into a compelling letter, which is a demanding job for the model.
    -   **Medium Output**: The output is a single block of text (the letter), which is smaller and less complex than the JSON from the full review.

#### 3. AI Suggestions (in Guided Builder): **1 Credit** (Lowest Cost)
-   **Relevant Flow**: `src/ai/flows/generate-resume-section.ts`
-   **Reasoning**: This is a "micro-transaction" designed for quick, focused assistance.
    -   **Small Input**: The request only sends the section type (e.g., "Work History"), the user's industry, and a small amount of context (like a job title). It does *not* send the whole resume.
    -   **Simple Task**: The AI's job is highly focused: generate a small list of example bullet points.
    -   **Tiny Output**: The response is a very small JSON object containing just a few strings.

---

## 7. Getting Started & Firebase Setup

This guide will walk you through setting up a Firebase project and connecting it to your CareerCraft AI application.

### Step 1: Prerequisites

*   Node.js and npm installed on your machine.
*   A Google Account.
*   A Google AI API key with the Gemini API enabled. You can get one from [Google AI Studio](https://aistudio.google.com/app/apikey).
*   A [Paystack Account](https://paystack.com/) (for payment processing).

### Step 2: Create Your Firebase Project

1.  Go to the [Firebase Console](https://console.firebase.google.com/).
2.  Click **"Add project"** and follow the on-screen instructions to create a new project. You can disable Google Analytics for this demo if you wish.

### Step 3: Enable Firebase Services

You need to enable Authentication and Firestore.

1.  **Enable Authentication**:
    *   In the Firebase Console, go to the **Authentication** section (under "Build").
    *   Click **"Get started"**.
    *   On the "Sign-in method" tab, select **Google** from the list of providers.
    *   Enable the Google provider and provide a project support email.
    *   Click **Save**.
    *   **CRITICAL:** Now, go to the **Settings** tab within Authentication. Under **Authorized domains**, click **Add domain** and enter `localhost`. This is required to allow sign-in when running the app locally.

2.  **Enable Firestore Database**:
    *   Go to the **Firestore Database** section (under "Build").
    *   Click **"Create database"**.
    *   Choose to start in **Production mode**.
    *   Select a location for your database (e.g., `us-central`).
    *   Click **Enable**.

### Step 4: Configure Environment Variables

This is how your application gets the keys to connect to Firebase, Google AI, and Paystack.

1.  In the root of this project, find the `.env` file.
2.  **Get Your Google AI Key**:
    *   In your `.env` file, find the line `GOOGLE_API_KEY=` and paste your key after the equals sign.
3.  **Get Your Paystack Public Key**:
    *   Log in to your [Paystack Dashboard](https://dashboard.paystack.com/).
    *   Go to **Settings > API Keys & Webhooks**.
    *   Copy your **Public Key** (it starts with `pk_test_` or `pk_live_`).
    *   In your `.env` file, find `NEXT_PUBLIC_PAYSTACK_PUBLIC_KEY=` and paste your key.

4.  **Get Your Firebase Configuration**:
    *   In the Firebase Console, go to your **Project Settings** (click the gear icon ⚙️ next to "Project Overview").
    *   In the "General" tab, scroll down to the "Your apps" section.
    *   Click the **Web** icon (`</>`) to create a new web app.
    *   Give your app a nickname (e.g., "CareerCraft Web") and click **"Register app"**.
    *   Firebase will show you your configuration object. Copy these values into your `.env` file, matching them with the `NEXT_PUBLIC_FIREBASE_*` variables.

Your completed `.env` file should look something like this:
```
# Paystack Public Key
NEXT_PUBLIC_PAYSTACK_PUBLIC_KEY=your_paystack_public_key

# Google AI Key
GOOGLE_API_KEY=your_google_api_key_here

# Firebase Config
NEXT_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=your_measurement_id # Optional
```

### Step 5: Set Up Firestore Security Rules

For the application to work correctly, you must set up security rules for your Firestore database. These rules ensure that users can only read and write their own data.

1.  In the Firebase Console, go to the **Firestore Database** section.
2.  Click on the **"Rules"** tab.
3.  Replace the existing rules with the following:
    ```
    rules_version = '2';
    service cloud.firestore {
      match /databases/{database}/documents {
        // Users can read and write to their own document in the 'users' collection.
        match /users/{userId} {
          allow read, update, delete: if request.auth.uid == userId;
          allow create: if request.auth.uid != null;
        }
      }
    }
    ```
4.  Click **"Publish"**.

### Step 6: Server-Side Paystack Integration (Required)

The client-side UI for Paystack is complete, but for a secure, production-ready system, you must implement a backend webhook handler.

**Why is this necessary?** The client-side `onSuccess` callback from Paystack is not secure. A malicious user could trigger it without actually paying. The only secure way to confirm a payment is to listen for a webhook event directly from Paystack's servers.

**What you need to do:**
1.  **Create a Webhook Endpoint**: You need to create a secure API endpoint in your application. The best way to do this in a Next.js app is with an API Route. Create a file at `src/app/api/paystack/webhook/route.ts`.
2.  **Verify the Webhook**: This endpoint must verify that incoming requests are genuinely from Paystack.
    *   Get your **Paystack Secret Key** from your dashboard. Store it securely as an environment variable (e.g., `PAYSTACK_SECRET_KEY`). **Do not** expose this in your client-side code.
    *   In your API route, use Node.js's `crypto` module to create an HMAC SHA512 hash of the request body using your secret key.
    *   Compare this hash to the `x-paystack-signature` header in the incoming request. If they don't match, the request is fraudulent and should be ignored.
3.  **Update Firestore**: Once a `charge.success` event is verified, your endpoint should securely update the user's `planId` in their Firestore document (`users/{userId}`). You can get the user's email from the webhook payload (`event.data.customer.email`) to find the correct user document.

This server-side logic is critical for the payment system to function correctly and securely.

### Step 7: Run the Application

Now you're ready to run the app!

*   In one terminal, install the dependencies:
    ```bash
    npm install
    ```
*   Then start the Next.js dev server:
    ```bash
    npm run dev
    ```
*   In a second terminal, start the Genkit dev server:
    ```bash
    npm run genkit:watch
    ```
*   Open your browser and navigate to `http://localhost:9002`. You should see the application and be able to sign in with Google.
