# How I Built an AI-Powered Resume Assistant with Firebase and Genkit

*A look under the hood of CareerCraft AI, a full-stack SaaS application that went from idea to deployment using a modern, AI-first tech stack.*

---

<img src="https://firebasestorage.googleapis.com/v0/b/studiodev-33920.appspot.com/o/launch-assets%2Fbg_klw77o33.png?alt=media&token=c19c5c7d-8e42-4f33-b91c-b5f7b8813a1e" alt="CareerCraft AI Dashboard Preview" data-ai-hint="app dashboard resume" />

The job application process can feel like a black hole. You spend hours crafting the perfect resume, tailoring it to a job description, and sending it off, only to be met with silence. As a developer, I knew there had to be a smarter way. What if you could get an expert opinion *before* you hit "submit"? What if you could use AI to not just check your spelling, but to give you a genuine competitive edge?

That's the idea behind **CareerCraft AI**, the AI-powered resume assistant I built. It’s a full-stack web application designed to help job seekers optimize their resumes, match them to specific jobs, and even generate tailored cover letters.

In this post, I want to pull back the curtain and show you exactly how I built it, the technologies I chose, and why this stack is so powerful for building modern, AI-native applications.

## The Goal: A Seamless, Intelligent User Experience

I had a few core principles for this project:

1.  **Fast & Modern UI**: The interface had to be clean, intuitive, and responsive.
2.  **Serverless & Scalable**: I didn't want to manage servers. The infrastructure should scale automatically with user demand.
3.  **AI at the Core**: The AI features couldn't be just a gimmick; they had to be deeply integrated and provide real value.
4.  **Secure & Private**: Users are uploading sensitive resume data. Their privacy is paramount.

## The Tech Stack: My Toolkit for Building AI-First Apps

Choosing the right tools is half the battle. Here’s the stack I used for CareerCraft AI and why each piece was critical.

#### **Framework: Next.js (with the App Router)**

For the frontend and application structure, Next.js was a no-brainer. The App Router, in particular, is a game-changer. Using Server Components by default allowed me to keep the client-side JavaScript bundle lean, leading to faster initial page loads. The ability to use Server Actions also dramatically simplified my client-server communication, letting me call secure backend functions as if they were local.

#### **UI: Tailwind CSS & ShadCN UI**

I'm a huge fan of utility-first CSS. Tailwind CSS gives me the power to build custom, polished designs without writing a single line of custom CSS.

To accelerate development, I used [ShadCN UI](https://ui.shadcn.com/). It's not a typical component library. Instead, it gives you beautifully designed, accessible components that you copy and paste directly into your project. This means you have full ownership and control over the code, which was perfect for tweaking components to match my exact design needs.

#### **Backend & Database: Firebase (Authentication & Firestore)**

Firebase is the backbone of this application. It’s a serverless platform that handles the most complex parts of building a web app with ease.

*   **Firebase Authentication**: I implemented a secure, passwordless sign-in flow using the Google provider in under an hour. Firebase handles all the complexities of session management, tokens, and security out of the box.
*   **Firestore**: This is the NoSQL database where all user data is stored. I created a `users` collection to hold profiles, subscription plans, and AI credit usage. The real-time nature of Firestore means that when a user's data is updated on the backend (like a plan change), the UI can reflect that change instantly without needing a page refresh.

```javascript
// A simplified look at the useUser hook connecting to Firestore
// src/hooks/useUser.ts

const userRef = doc(db, 'users', user.uid);
unsubscribeFirestore = onSnapshot(userRef, (userSnap) => {
    // ... update React state with user data
});
```

#### **AI Orchestration: Genkit**

This is the heart of the AI functionality. [Genkit](https://firebase.google.com/docs/genkit) is an open-source framework from Google that makes it incredibly simple to build, test, and monitor AI-powered features.

Instead of making direct, messy calls to an AI model from my application code, I defined self-contained **Genkit flows**. For example, the `reviewAndImprove` flow is a single function that takes the user's resume and a job description, sends a carefully crafted prompt to the AI, and returns a perfectly structured JSON object with the full analysis.

This decouples the AI logic from the application logic, making the entire system cleaner and easier to maintain.

#### **The Brains: Google's Gemini Model**

All of Genkit’s flows in this app are powered by Google's **Gemini** model. Its key strengths for this project were:

*   **Structured Output**: I can instruct Gemini to return its response in a specific JSON schema using Zod. This is incredibly powerful. Instead of getting back a blob of text that I have to parse, I get a predictable object with fields like `overallScore`, `keywordGaps`, and `enhancedResumeText`.

```typescript
// A snippet from a Genkit flow defining the output schema
// src/ai/flows/review-and-improve.ts

const ReviewAndImproveOutputSchema = z.object({
  overallScore: z.number().int().min(0).max(100),
  keyRecommendations: z.array(z.string()),
  // ... more fields
});
```

*   **Strong Instruction Following**: For a tool like this, the AI needs to follow complex instructions precisely, like avoiding bias and rewriting content in a certain style. Gemini excels at this.

## How It All Works: The AI Credit System

One of the biggest challenges with AI features is managing cost. Every call to the Gemini API costs money based on the number of tokens processed. To prevent abuse and build a sustainable business model, I implemented a simple credit system.

1.  **Centralized Costs**: I created a single `AI_CREDIT_COSTS` object in my code that defines how much each feature costs (e.g., a full review is 10 credits, a cover letter is 5).
2.  **Firestore Tracking**: Each user's document in Firestore has a `creditsToday` field and a `lastUsageDate` field.
3.  **Daily Reset**: When a user performs their first action of the day, the app checks if `lastUsageDate` is yesterday. If it is, it resets `creditsToday` to 0.
4.  **Client-Side Checks & Server-Side Decrements**: Before a user can click "Generate," the app checks if they have enough credits. If they do, the AI flow is called. Only after a *successful* response from the AI does the client tell Firestore to atomically increment the `creditsToday` count. This ensures users are only charged for what they use.

## Pre-Launch Strategy: The "Early Access" Program

Before launching paid plans, my goal is to build a community and gather feedback. I implemented a simple "Early Access" plan that gives all new users a generous daily allowance of 100 AI credits for free.

This is managed entirely in the `useUser` hook, which is the central hub for user data on the client. During the pre-launch phase, it simply overrides any other plan and assigns the "Early Access" benefits to every single user. This makes early users feel valued and encourages them to explore the app's full potential.

## Final Thoughts

Building CareerCraft AI has been an incredibly rewarding experience. The modern full-stack toolkit of Next.js, Firebase, and Genkit makes it possible for a single developer to build and deploy sophisticated, AI-powered applications that once would have required a large team.

If you're looking to build your own AI-native application, I can't recommend this stack highly enough.

---

### What's Next?

-   ➡️ **Check out the full source code on GitHub**: [github.com/your-username/careercraft-ai](https://github.com/your-username/careercraft-ai)
-   👍 **Follow me on Medium** for more articles on building with modern web technologies.
-   🤝 **Connect with me on LinkedIn**: [linkedin.com/in/your-profile](https://linkedin.com/in/your-profile)
-   🌐 **Explore my full portfolio**: [your-portfolio.com](https://your-portfolio.com)
