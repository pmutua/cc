# Setup and Deployment Guide

This guide will walk you through setting up a Firebase project and connecting it to your CareerCraft AI application so you can run it locally.

## 1. Prerequisites

-   **Node.js**: Version 18.x or higher.
-   **npm**: Should be included with your Node.js installation.
-   **Google Account**: Required to create a Firebase project.
-   **Google AI API Key**: You need an API key with the Gemini API enabled. You can get one from [Google AI Studio](https://aistudio.google.com/app/apikey).
-   **Paystack Account** (Optional for local testing): You'll need a Paystack account to test the full payment flow. You can get your test keys from the [Paystack Dashboard](https://dashboard.paystack.com/).

---

## 2. Firebase Project Setup

### Step 1: Create Your Firebase Project

1.  Go to the [Firebase Console](https://console.firebase.google.com/).
2.  Click **"Add project"** and follow the on-screen instructions to create a new project. You can disable Google Analytics for this demo if you wish.

### Step 2: Enable Firebase Services

You need to enable Authentication and Firestore for the application to work.

1.  **Enable Authentication**:
    -   In the Firebase Console, go to the **Authentication** section (under "Build").
    -   Click **"Get started"**.
    -   On the "Sign-in method" tab, select **Google** from the list of providers.
    -   Enable the Google provider and provide a project support email. Click **Save**.
    -   **CRITICAL:** Go to the **Settings** tab within Authentication. Under **Authorized domains**, click **Add domain** and enter `localhost`. This is required for local development.

2.  **Enable Firestore Database**:
    -   Go to the **Firestore Database** section (under "Build").
    -   Click **"Create database"**.
    -   Choose to start in **Production mode**.
    -   Select a location for your database (e.g., `us-central`).
    -   Click **Enable**.

### Step 3: Set Up Firestore Security Rules

For the application to work correctly, you must set up security rules that allow users to read and write their own data.

1.  In the Firebase Console, go to the **Firestore Database** section and click the **"Rules"** tab.
2.  Replace the existing rules with the following:
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
3.  Click **"Publish"**.

---

## 3. Local Development

### Step 1: Clone the Repository

```bash
git clone https://github.com/your-username/careercraft-ai.git
cd careercraft-ai
```

### Step 2: Install Dependencies

```bash
npm install
```

### Step 3: Configure Environment Variables

This is how your application gets the keys to connect to Firebase, Google AI, and Paystack.

1.  Create a new file named `.env` in the root of the project.
2.  Add the following variables to your `.env` file, replacing the placeholder values with your actual keys.

    ```
    # Paystack Public Key (for client-side)
    NEXT_PUBLIC_PAYSTACK_PUBLIC_KEY=pk_test_xxxxxxxxxx

    # Paystack Secret Key (for server-side webhook verification)
    PAYSTACK_SECRET_KEY=sk_test_xxxxxxxxxx

    # Google AI Key
    GOOGLE_API_KEY=your_google_ai_key_here

    # Firebase Config
    NEXT_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
    NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
    NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
    NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
    NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
    NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id
    ```

    To get your Firebase configuration values, go to your **Project Settings** in the Firebase Console, and in the "General" tab, scroll down to "Your apps" and click the Web icon (`</>`) to find your config object.

### Step 4: Run the Development Servers

1.  In one terminal, start the Next.js development server:
    ```bash
    npm run dev
    ```

2.  In a second terminal, start the Genkit development server (which runs the AI flows):
    ```bash
    npm run genkit:watch
    ```

3.  Open your browser and navigate to `http://localhost:3000`. You should see the application and be able to sign in with Google.

---

## 4. Deployment

This application is configured for one-click deployment to **Firebase App Hosting**.

The `apphosting.yaml` file in the root of the project contains the configuration for the App Hosting backend.

### Deployment Workflow

1.  **Prerequisites**:
    -   Install the Firebase CLI: `npm install -g firebase-tools`
    -   Log in to Firebase: `firebase login`

2.  **Initialize App Hosting**:
    -   Run `firebase init apphosting` in your project directory.
    -   Follow the prompts to connect to your Firebase project.

3.  **Deploy**:
    -   Run the following command to build and deploy your application:
        ```bash
        firebase apphosting:backends:deploy
        ```
    -   The CLI will provide you with the URL of your live application.
