t t # CareerCraft AI: Your AI-Powered Resume Assistant

<p align="center">
  <img src="https://firebasestorage.googleapis.com/v0/b/studiodev-33920.appspot.com/o/launch-assets%2Fbg_klw77o33.png?alt=media&token=c19c5c7d-8e42-4f33-b91c-b5f7b8813a1e" alt="CareerCraft AI Dashboard Preview" data-ai-hint="app dashboard resume" width="800"/>
</p>

<p align="center">
  <em>Craft the resume that lands the interview. CareerCraft AI is a full-stack SaaS application that leverages generative AI to provide a comprehensive suite of tools for resume analysis, enhancement, and optimization.</em>
</p>

<p align="center">
  <a href="./docs/progress-history.md"><img src="https://img.shields.io/badge/Project_Status-Actively_Developed-brightgreen" alt="Project Status"></a>
  <a href="https://github.com/your-username/careercraft-ai/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License"></a>
</p>

---

## 🚀 Motivation

The job application process can feel like a black hole. Talented candidates are often overlooked not because they lack skills, but because their resumes fail to communicate their value effectively or pass through automated screening systems (ATS). I built CareerCraft AI to solve this problem by providing job seekers with an intelligent assistant to create a truly compelling, professional, and optimized resume.

## ✨ Key Features

-   **Multi-Format Resume Parsing**: Upload `.pdf`, `.docx`, or `.txt` files, or simply paste text. All parsing is done securely in the browser.
-   **Guided Resume Builder**: Don't have a resume? Create one from scratch with a step-by-step wizard and get AI-powered suggestions for every section.
-   **AI-Powered Comprehensive Review**: Get a holistic analysis of your resume, including an overall quality score, key recommendations, and content analysis for grammar, tone, and impact.
-   **Job-Specific Optimization**: Paste a job description URL to get a "Job Match Score," identify keyword gaps, and receive suggestions to tailor your resume for a specific role.
-   **ATS Readiness Optimization**: Understand how an Applicant Tracking System sees your resume and get an ATS compatibility score with actionable advice.
-   **AI Cover Letter Generation**: Generate a tailored cover letter draft in seconds based on your enhanced resume and the target job description.
-   **Professional Template Library & PDF Export**: Choose from a library of professionally designed, ATS-friendly templates and export your final resume as a polished PDF.

## 🛠️ Tech Stack & Architecture

This project is built on a modern, serverless architecture designed for scalability and rapid development.

<p align="center">
  <img src="https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white" alt="Next.js">
  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript">
  <img src="https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white" alt="Tailwind CSS">
  <img src="https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black" alt="Firebase">
  <img src="https://img.shields.io/badge/Google_Gemini-4285F4?style=for-the-badge&logo=google&logoColor=white" alt="Google Gemini">
</p>

-   **Framework**: [Next.js](https://nextjs.org/) (with App Router)
-   **Language**: [TypeScript](https://www.typescriptlang.org/)
-   **Generative AI**: [Google Gemini](https://deepmind.google.com/technologies/gemini/) via [Genkit](https://firebase.google.com/docs/genkit)
-   **Backend & Hosting**: [Firebase](https://firebase.google.com/) (Authentication, Firestore, App Hosting)
-   **UI**: [React](https://react.dev/), [ShadCN UI](https://ui.shadcn.com/), [Tailwind CSS](https://tailwindcss.com/)

➡️ **[See the detailed Architecture and Tech Stack Documentation](./docs/architecture.md)**

## 🚀 Quick Start Guide

Get the application running on your local machine in just a few steps.

1.  **Clone the Repository**
    ```bash
    git clone https://github.com/your-username/careercraft-ai.git
    cd careercraft-ai
    ```

2.  **Install Dependencies**
    ```bash
    npm install
    ```

3.  **Set Up Environment Variables**
    -   Follow the detailed setup guide to get your Firebase and Google AI keys.
    -   Copy the values into a new `.env` file at the root of the project.

4.  **Run the Development Servers**
    -   In one terminal, start the Next.js app:
        ```bash
        npm run dev
        ```
    -   In a second terminal, start the Genkit AI flows:
        ```bash
        npm run genkit:watch
        ```

5.  **Open the App**
    -   Navigate to `http://localhost:3000` in your browser.

➡️ **[View the Full Setup and Deployment Guide](./docs/setup.md)**

## 📚 Full Documentation

-   **[Architecture Overview](./docs/architecture.md)**
-   **[Tech Stack](./docs/stack.md)**
-   **[Features Guide](./docs/features.md)**
-   **[Setup & Deployment](./docs/setup.md)**
-   **[Future Roadmap](./docs/future-roadmap.md)**
-   **[FAQ & Troubleshooting](./docs/faq.md)**

## 📈 Project Progress & History

This project is actively developed. You can view a timeline of the work and key milestones, which serves as proof of my consistent contribution and the project's evolution.

-   **[Changelog](./docs/changelog.md)**: A versioned log of new features and fixes.
-   **[Progress History](./docs/progress-history.md)**: A human-readable development timeline.

## ✍️ Blog Post: How I Built This

I've written a detailed, Medium-ready article that breaks down how I built this entire application from scratch using a modern, AI-first tech stack.

➡️ **[Read the full story: "How I Built an AI-Powered Resume Assistant with Firebase and Genkit"](./docs/blog-firebase-studio.md)**

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the issues page.

➡️ **[Read our Contribution Guidelines](./docs/contributing.md)**

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

## 📬 Contact

-   **GitHub**: [@your-username](https://github.com/your-username)
-   **LinkedIn**: [Your Name](https://www.linkedin.com/in/your-profile/)
-   **Portfolio**: [your-portfolio.com](https://your-portfolio.com)
-   **Email**: [your-email@example.com](mailto:your-email@example.com)
