# CareerCraft AI - Pre-Launch Strategy

This document outlines a complete, step-by-step pre-launch plan to maximize early adoption, engagement, and feedback before paid subscriptions are introduced.

---

## 1. Temporary Pricing & Offer (CRITICAL)

**Purpose:** To remove all barriers to entry, encouraging maximum sign-ups and usage. This frames early users as "Early Access Members" who are part of an exclusive group, making them feel valued rather than simply using a free product.

**Step-by-Step Instructions:**
1.  **Disable Paid Plans:** The `useUser` hook (`src/hooks/useUser.ts`) has been modified to temporarily hide the "Starter" and "Pro" plans.
2.  **Create "Early Access" Plan:** A temporary "Early Access" Plan has been created in `useUser.ts`. This plan has a generous `creditAllowance: 100` to grant extensive access to all AI features while protecting against abuse. All new and existing users are automatically enrolled in this plan.
3.  **Update UI:** The pricing page (`src/components/pricing-page.tsx`) has been updated to only show the "Early Access" Plan, clearly communicating the free early access offer.

**Messaging:**
-   **Headline:** "Get Free, Extensive Access to Your AI Resume Assistant"
-   **Sub-headline:** "For a limited time, become an early access member and get a generous daily allowance of AI credits to use all CareerCraft features for free. Help us shape the future of resume building."

---

## 2. Landing Page Optimization (CRITICAL)

**Purpose:** To convert visitors into early adopters by clearly communicating the value proposition of the limited-time offer.

**Step-by-Step Instructions:**
1.  **Update Hero Section:** The main landing page (`src/app/page.tsx`) hero section has been updated with the new messaging above. The main Call to Action (CTA) has been changed from "Get Started for Free" to "Get Your Free Account" or "Claim Your Free Early Access."
2.  **Add "Why Early Access?" Section:** A new section has been added to the landing page explaining the benefits of joining as an early access member: a generous credit allowance, a direct line to the development team, and the opportunity to provide feedback.
3.  **Reinforce the CTA:** All CTAs on the page now lead directly to the sign-up/login flow. Since Firebase Authentication captures the user's email, a separate waitlist form is not needed.

**Visuals:**
-   Use an icon like a gift box or a "VIP badge" to visually represent the special offer.

---

## 3. Social Media Teasing & Promotion (IMPORTANT)

**Purpose:** To generate awareness and drive traffic to the landing page from relevant professional networks.

**Step-by-Step Instructions:**
1.  **Choose Platforms:** Focus on **LinkedIn** (for professional audience) and **Twitter/X** (for tech/developer audience).
2.  **Create Visuals:** Prepare screenshots and short screen recordings of the app in action. Key visuals should include:
    *   The AI analysis score (e.g., a "before" and "after" score).
    *   The keyword gap analysis.
    *   The AI Cover Letter generator in action.
3.  **Schedule Posts:** Plan a series of posts leading up to and during the pre-launch period.

**Example Posts:**

-   **Post 1: The Problem (LinkedIn/Twitter)**
    -   **Copy:** "Did you know that over 75% of resumes are rejected by an ATS before a human ever sees them? It's not because the candidates are unqualified—it's because their resumes aren't optimized. We're building something to fix that. #ResumeTips #JobSearch #CareerTech #ATS"
    -   **Visual:** A stark infographic showing the 75% rejection statistic.

-   **Post 2: The Solution Teaser (LinkedIn/Twitter)**
    -   **Copy:** "What if you could know your resume's "Job Match Score" *before* you apply? Here's a sneak peek of the AI-powered analysis inside CareerCraft AI. We're getting ready to let the first users in for free. #AI #ResumeBuilder #CareerCraftAI"
    -   **Visual:** A short screen recording showing the Job Match score and keyword analysis.

-   **Post 3: The Launch Announcement (LinkedIn/Twitter)**
    -   **Copy:** "It's time. We're opening up CareerCraft AI for free early access! Get instant AI analysis, ATS optimization, and tailored cover letters—all for free. Your feedback will help shape our future. Claim your spot! [Link to your app] #ProductLaunch #SaaS #AI #FreeTool"
    -   **Visual:** A polished hero image of the app dashboard.

-   **Post 4: Feature Spotlight (LinkedIn/Twitter)**
    -   **Copy:** "Writing cover letters is painful. So we had our AI do it for you. Based on your new resume and the job description, CareerCraft generates a tailored draft in seconds. Available now for all our early access members. #CoverLetter #AIWriter #JobApplication"
    -   **Visual:** A video showing the cover letter generation feature.

---

## 4. Analytics Setup (CRITICAL)

**Purpose:** To understand user behavior, identify popular features, and find areas for improvement. This data is critical for making informed product decisions.

**Step-by-Step Instructions:**
1.  **Use a Web Analytics Tool:** Integrate a tool like Google Analytics, Vercel Analytics, or Plausible.
2.  **Prioritize Key Events to Track:**
    *   **User Sign-up:** The most important initial metric. How many visitors are converting to users?
    *   **Full Review Generation:** Track every time the `handleFullReview` function is successfully called in `src/app/app/page.tsx`. This is your core feature engagement metric.
    *   **Cover Letter Generation:** Track clicks on the `handleGenerateCoverLetter` button. This measures engagement with a key premium feature.
    *   **Guided Builder Usage:** Track how many users start the guided builder vs. pasting text. This tells you which input method is preferred.
    *   **Template Export:** Track clicks on the `handlePrint` function. This is a key success metric—it means a user created a document they intend to use.

**Why Each Metric Matters:**
-   **Sign-up Rate:** Tells you if your landing page is effective.
-   **Feature Engagement (Review, Cover Letter):** Shows which AI features provide the most value. If one is used far more than another, it influences future development and pricing.
-   **Export Rate:** Indicates that users are completing the entire workflow and finding the final product valuable enough to use in a real job application.

---

## 5. Referral & Sharing Incentives (OPTIONAL, BUT RECOMMENDED)

**Purpose:** To leverage your initial user base to drive word-of-mouth growth.

**Step-by-Step Instructions:**
1.  **Keep it Simple:** Since paid plans are disabled, the incentive cannot be a discount. Instead, focus on status and early access.
2.  **Implement the Incentive:**
    *   After a user successfully exports a resume or receives a great review score, show them a pop-up or a dedicated section in the UI.
    *   **Messaging:** "Love your new resume? Share CareerCraft AI with 3 friends! As a thank you, we'll give you a permanent "Early Access" badge on your profile and a 50% discount for life when we do introduce paid plans."
    *   **Mechanism:** Use a simple "Share" button that copies a unique referral link or opens a pre-populated email.

**Expected Impact:**
-   This turns your most engaged users into advocates. The promise of a future discount is a powerful, low-cost incentive that can significantly boost early-stage growth.

---

## 6. Community Building & Feedback (CRITICAL)

**Purpose:** To create a direct feedback loop with your most engaged users, making them feel like true insiders and providing you with invaluable insights.

**Step-by-Step Instructions:**
1.  **Create a Feedback Form:** Set up a simple Google Form to collect structured feedback. Ask targeted questions like:
    *   What's the #1 thing you wish CareerCraft could do?
    *   What was the most confusing part of the experience?
    *   How valuable was the AI analysis? (Scale of 1-5)
    *   Did you encounter any bugs?
2.  **Promote the Feedback Link In-App:**
    *   The application now has a "Leave Feedback" button in the main header (`src/components/app-header.tsx`).
    *   **ACTION REQUIRED:** You must replace the placeholder URL in the header with your actual Google Form link.
3.  **Engage Actively:**
    *   **Purpose:** To show users you are listening and value their input.
    *   **Tactics:** Post regular updates on what you're building based on feedback. Acknowledge and thank every user who provides feedback. Share success stories from the community in a blog post or on social media.

    