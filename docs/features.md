# Features & Functionality Guide

CareerCraft AI is designed to be a comprehensive, step-by-step assistant for creating a job-winning resume. Below is a breakdown of the core features and workflows.

## 1. Resume Input & Creation

Users have three primary ways to get their resume content into the application:

-   **File Upload**: Users can upload existing resumes in `.pdf`, `.docx`, or `.txt` formats. For privacy, all file parsing happens directly in the browser—the file itself is never sent to a server.
-   **Paste Text**: Users can copy and paste their resume content from any source into a large text editor.
-   **Guided Resume Builder**: For users starting from scratch, the Guided Builder provides a step-by-step form to create a resume section by section (Basics, Work History, Education, etc.).
    -   **AI Suggestions**: Within the builder, users can spend a small number of AI credits to get AI-generated suggestions and tips for any section they are working on, helping to overcome writer's block.

## 2. Job Description Analysis

To maximize the effectiveness of the AI review, users can (optionally) provide a job description for the role they are targeting.

-   **URL Extraction**: Users can paste a URL to a job posting (e.g., on LinkedIn). The application uses an AI-powered flow (`extractJobDescriptionFromUrl`) to scrape the page, remove noise, and extract the core job description text.
-   **Manual Paste**: Users can also paste the job description text directly.

## 3. AI-Powered Comprehensive Review

This is the core feature of the application, powered by the `reviewAndImprove` Genkit flow. When a user requests a full review, the AI performs a multi-faceted analysis and returns a rich, structured response.

### Key Analysis Areas:

-   **AI-Enhanced Resume Text**: The AI completely rewrites the user's resume content to be more impactful, using strong action verbs and quantifying achievements. This new version becomes the basis for all further analysis and export.
-   **Overall Score (0-100)**: A single, holistic score representing the quality of the *newly enhanced* resume.
-   **Key Recommendations**: A concise list of the top 3-5 most important improvements the AI made.
-   **Content Analysis**:
    -   **Clarity & Tone**: Evaluates the professionalism and consistency of the new text.
    -   **Grammar & Spelling**: Confirms the absence of errors.
    -   **Impact & Action Verbs**: Provides feedback on the use of powerful language.
-   **Job Match Analysis (if job description is provided)**:
    -   **Match Score (0-100)**: A score indicating how well the enhanced resume matches the job description.
    -   **Keyword Gaps**: A list of important keywords from the job description that are missing from the resume.
    -   **Matched Keywords**: A list of keywords present in both documents.
-   **ATS Analysis**:
    -   **ATS Score (0-100)**: A score indicating the Applicant Tracking System (ATS) compatibility of the enhanced resume.
    -   **ATS Suggestions**: Explanations of changes made to improve the resume's machine-readability (e.g., standard date formats, removal of complex formatting).

## 4. Interactive Results Panel

The AI's comprehensive review is presented in a rich, tabbed interface, allowing the user to explore the feedback in detail.

-   **Overview**: Shows the overall score and key recommendations.
-   **Enhanced Resume**: Displays the full, rewritten resume text in an editable `textarea`, allowing for final manual tweaks.
-   **Job Match**: Visualizes the match score, keyword gaps, and matched keywords.
-   **Cover Letter**: A dedicated tab for the AI Cover Letter Generator.
-   **Content & ATS**: Tabs to review the detailed feedback on content quality and ATS compatibility.

## 5. AI Cover Letter Generator

If a user has provided a job description, they can use the `generateCoverLetter` flow to create a tailored cover letter.

-   The AI uses the **enhanced resume** and the target job description as context.
-   Users can select a desired tone (e.g., Formal, Creative, Enthusiastic).
-   The generated letter is displayed in an editable text area for review and personalization.

## 6. Template Library & PDF Export

The final step is to apply the enhanced resume content to a professional template.

-   **Template Browser**: Users can browse a filterable library of professionally designed resume templates. Categories include Structural, Career Stage, Profession-Based, and Format-Based (e.g., "ATS-Friendly", "Markdown").
-   **Live Preview**: Selecting a template opens a full-size, live preview in a dialog, showing the user's content formatted in the chosen design.
-   **PDF Export**: From the preview dialog, users can export their final resume as a high-quality, print-ready PDF with a single click.

## 7. User Account & Credit System

-   **Authentication**: Secure sign-in is handled by Firebase Authentication (Google provider).
-   **Subscription Plans**: The app is built with a flexible plan structure (`Free`, `Starter`, `Pro`). During the pre-launch phase, all users are enrolled in a generous "Early Access" plan.
-   **AI Credits**: Major AI actions consume credits. This is managed via the `useUser` hook and Firestore, with costs defined centrally. This system provides a simple way for users to understand their usage and protects the application from abuse.
