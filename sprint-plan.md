# Sprint Plan: Paradox Coaching Transcript Analysis Web App

This document outlines a sprint-based plan for the development of the Paradox Coaching Transcript Analysis Web App. It expands on the high-level phases defined in the project scope.

## Phase 1: Foundation & Authentication (Approx. 3-4 weeks)

**Goal:** Establish the project's foundational structure, integrate user authentication, set up the database, and configure basic deployment.

**Sprint 1.1: Project Setup & Initial Backend (1 week) - COMPLETED**
*   **Tasks:**
    *   [x] Initialize Next.js frontend project.
    *   [x] Set up basic file structure for frontend components and pages.
    *   [x] Initialize Node.js backend project for Vercel Functions.
    *   [x] Define basic API endpoint structure (e.g., `/api/auth`, `/api/transcripts`).
    *   [x] Set up linting, formatting, and pre-commit hooks for both frontend and backend.
    *   [x] Create initial `README.md` with project setup instructions.
*   **Deliverables:**
    *   [x] Git repository with initial frontend and backend code structure.
    *   [x] Basic "Hello World" Next.js app running locally.
    *   [x] Basic Vercel Function endpoint testable locally.

**Sprint 1.2: Authentication Integration & UI Foundation (1-2 weeks) - COMPLETED**
*   **Tasks:**
    *   [x] Sign up for Clerk and configure a new application. (Manual Step - Assumed Done)
    *   [x] **ShadCN/UI Setup:**
        *   [x] Initialize ShadCN/UI in the project.
        *   [x] Configure `tailwind.config.js` and `globals.css` as per ShadCN/UI documentation.
        *   [x] Add a sample ShadCN/UI component (e.g., Button) to test the setup.
    *   [x] **Clerk Integration:**
        *   [x] Install Clerk Next.js SDK.
        *   [x] Set up Clerk environment variables (`.env.local`). (Assumed Done by User)
        *   [x] Wrap root layout with `<ClerkProvider>`.
        *   [x] Create Clerk sign-in (`/sign-in`) and sign-up (`/sign-up`) pages.
        *   [x] Create Clerk middleware (`middleware.ts`) to protect routes.
        *   [x] Integrate Clerk frontend components (e.g., `UserButton`, `SignInButton`, `SignOutButton`) into the UI (potentially using ShadCN components for styling).
    *   [x] Implement protected routes in Next.js (verify with middleware).
    *   [x] Set up Clerk backend SDK/middleware in Vercel Functions to verify authenticated requests (basic setup, more in-depth when APIs are built).
    *   [x] Test authentication flow thoroughly (sign up, sign in, sign out, protected route access). (User Confirmed)
*   **Deliverables:**
    *   [x] ShadCN/UI initialized and configured.
    *   [x] Functional teacher login, logout, and user profile display using Clerk.
    *   [x] Frontend routes protected by authentication.
    *   [x] Basic backend mechanism to identify authenticated users (via Clerk middleware).

**Sprint 1.3: Database & Basic Deployment (1 week)**
*   **Tasks:**
    *   [x] Choose and set up a PostgreSQL database provider (e.g., Vercel Postgres, Supabase, Neon). (User Confirmed: Neon)
    *   [x] Define initial database schema (Prisma: `Analysis` model).
        *   [x] `Users` table (if storing user-specific info beyond Clerk, otherwise Clerk handles user profiles. At minimum, a way to link analyses to Clerk User IDs). (Decided: Link via `teacher_id` in `Analysis` table)
        *   [x] `Analyses` table (columns for `id`, `teacher_id`, `transcript_content`, `analysis_json`, `created_at`, `updated_at`).
    *   [x] Write initial database migration scripts (if using a migration tool). (Prisma `migrate dev`)
    *   [x] Connect the Vercel Functions backend to the PostgreSQL database. (Prisma Client setup)
    *   [x] Implement a basic API endpoint to test database connectivity (e.g., read/write a dummy record). (`/api/db-test` - Assumed Successful by User)
    *   [ ] Set up basic Vercel deployment for both frontend and backend.
    *   [ ] Configure environment variables for Clerk, Database in Vercel. (OpenAI API key later)
*   **Deliverables:**
    *   [x] Provisioned PostgreSQL database with initial schema.
    *   [x] Backend API connected to the database.
    *   [ ] Basic version of the application deployed to Vercel with working authentication and database connection.

## Phase 2: Core Analysis & Saving (Approx. 4-5 weeks)

**Goal:** Implement transcript submission, integrate AI analysis, and enable saving of transcripts and results to the database.

**Sprint 2.1: Transcript Input - Text Area (1 week)**
*   **Tasks:**
    *   Design and implement the UI for transcript submission via a text area on the frontend.
    *   Develop frontend logic to capture text input.
    *   Create a backend API endpoint (`/api/submit-transcript-text`) to receive the text transcript from an authenticated teacher.
    *   Basic validation for transcript input (e.g., not empty).
*   **Deliverables:**
    *   Functional text area for transcript input.
    *   Backend endpoint that receives text transcripts.

**Sprint 2.2: Transcript Input - File Upload (1 week)**
*   **Tasks:**
    *   Implement UI for `.txt` and `.docx` file upload on the frontend.
    *   Develop frontend logic to handle file selection and prepare it for upload.
    *   Create a backend API endpoint (`/api/submit-transcript-file`) to receive file uploads.
    *   Implement backend logic to parse `.txt` files.
    *   Research and implement backend logic to parse `.docx` files (e.g., using a library like `mammoth.js`).
    *   Add validation for file types and size.
*   **Deliverables:**
    *   Functional file upload for `.txt` and `.docx` files.
    *   Backend logic to extract text content from uploaded files.

**Sprint 2.3: OpenAI API Integration & Initial Analysis (1-2 weeks)**
*   **Tasks:**
    *   Securely store and manage OpenAI API key.
    *   Develop a backend service/module to interact with the OpenAI API (`gpt-4o`).
    *   Refine and finalize the AI prompt (as provided in project scope) for transcript analysis.
    *   Implement logic to send the transcript (from text or file) to the OpenAI API and receive the structured JSON response.
    *   Implement initial error handling for OpenAI API requests (e.g., API errors, rate limits, timeouts).
    *   Create a test endpoint or utility to send a sample transcript and view the raw JSON output from OpenAI.
*   **Deliverables:**
    *   Backend module capable of sending transcripts to OpenAI and receiving analysis.
    *   Successful retrieval of structured JSON analysis from OpenAI for sample transcripts.

**Sprint 2.4: Saving Analysis to Database (1 week)**
*   **Tasks:**
    *   Extend the backend API endpoints (or create a new one like `/api/analyze-and-save`) to:
        *   Receive the transcript.
        *   Call the OpenAI API for analysis.
        *   Parse the OpenAI JSON response.
        *   Save the original transcript and the AI-generated analysis JSON to the `Analyses` table in the database, linking it to the authenticated teacher's ID (from Clerk).
    *   Implement robust error handling for database operations.
*   **Deliverables:**
    *   Submitted transcripts and their corresponding AI analyses are saved to the database.
    *   Data is correctly linked to the authenticated teacher.

## Phase 3: Display & Retrieval (Approx. 3-4 weeks)

**Goal:** Develop the frontend to display AI evaluations, implement Pass/Fail logic, and allow teachers to view their saved analyses.

**Sprint 3.1: Structured Display of AI Evaluation (1-2 weeks)**
*   **Tasks:**
    *   Design and implement frontend components to display the structured AI evaluation (scores, observations, strengths, areas for improvement, actionable suggestions) based on the JSON received from the backend.
    *   Ensure clear and organized presentation of feedback, including transcript evidence.
    *   Develop UI for the prominent "Pass" or "Fail" visual indicator and its reasoning.
    *   Frontend logic to interpret the `pass_exam` boolean and reasoning from the AI analysis.
    *   Implement loading states and error messages for the analysis display.
*   **Deliverables:**
    *   Frontend UI that clearly presents the detailed AI analysis results.
    *   Visual Pass/Fail assessment displayed to the teacher.

**Sprint 3.2: Saved Analyses Dashboard/List (1 week)**
*   **Tasks:**
    *   Create a backend API endpoint (`/api/analyses`) to retrieve a list of saved analyses for the currently authenticated teacher.
    *   Design and implement a dashboard or list view page on the frontend.
    *   Display key information for each saved analysis (e.g., date, a snippet of the transcript or title if added, Pass/Fail status).
    *   Implement pagination or infinite scrolling if the list can become long.
*   **Deliverables:**
    *   Backend endpoint to fetch saved analyses for a teacher.
    *   Frontend dashboard displaying a list of the teacher's saved analyses.

**Sprint 3.3: Viewing Detailed Saved Analysis (1 week)**
*   **Tasks:**
    *   Create a backend API endpoint (`/api/analyses/[analysisId]`) to retrieve a specific saved analysis (transcript and AI JSON) by its ID, ensuring the requesting teacher owns it.
    *   Implement frontend routing for viewing a single saved analysis (e.g., `/analyses/[analysisId]`).
    *   Reuse or adapt the AI evaluation display components (from Sprint 3.1) to show the details of the selected saved analysis.
    *   Implement functionality to display the original submitted transcript alongside or linked from the detailed analysis view.
*   **Deliverables:**
    *   Backend endpoint to fetch a specific saved analysis.
    *   Frontend page to view the full details of a selected saved analysis, including the original transcript.

## Phase 4: Output & Polish (Approx. 2-3 weeks)

**Goal:** Implement PDF report generation, improve UI/UX, and add robust error handling.

**Sprint 4.1: PDF Report Generation (1-2 weeks)**
*   **Tasks:**
    *   Choose and set up Puppeteer (or an alternative PDF library) on the backend (Vercel Functions environment).
    *   Design the layout and content for the PDF report, summarizing a saved analysis.
    *   Create a backend API endpoint (`/api/analyses/[analysisId]/download-pdf`) that:
        *   Retrieves the saved analysis data.
        *   Generates a PDF using Puppeteer with the data.
        *   Returns the PDF as a downloadable file.
    *   Add a "Download PDF" button on the detailed saved analysis view on the frontend.
*   **Deliverables:**
    *   Functional PDF generation for saved analyses.
    *   Teachers can download a PDF report of their analysis.

**Sprint 4.2: UI/UX Refinement & Responsiveness (1 week)**
*   **Tasks:**
    *   Review and refine the overall UI/UX for consistency and ease of use.
    *   Ensure the application is responsive and usable on various devices (desktop, tablet, mobile).
    *   Add loading indicators, toasts/notifications for actions (e.g., saving analysis, errors).
    *   Improve overall styling and visual appeal.
*   **Deliverables:**
    *   Polished and responsive user interface.
    *   Enhanced user experience with clear feedback mechanisms.

**Sprint 4.3: Robust Error Handling & Testing (Ongoing, focus this week)**
*   **Tasks:**
    *   Systematically review and enhance error handling across the application:
        *   Frontend: User-friendly messages for API errors, validation errors, network issues.
        *   Backend: Comprehensive error logging, appropriate HTTP status codes, graceful failure for external service issues (OpenAI, DB).
    *   Conduct thorough testing of all key features and user flows.
    *   Address any identified bugs or issues.
*   **Deliverables:**
    *   Robust error handling implemented throughout the application.
    *   A more stable and reliable application.

## Phase 5: Final Deployment & Handover (Approx. 1 week)

**Goal:** Finalize deployment, prepare documentation, and hand over the application.

**Sprint 5.1: Final Deployment & Documentation (1 week)**
*   **Tasks:**
    *   Perform final testing on the Vercel production environment.
    *   Ensure all environment variables are correctly configured for production.
    *   Optimize application performance (e.g., code splitting, image optimization).
    *   Prepare teacher user guide/instructions for using the app.
    *   Prepare technical documentation (e.g., setup, deployment, API overview).
    *   Clean up codebase, remove unused code, and ensure consistent formatting.
*   **Deliverables:**
    *   Fully deployed and tested application on Vercel.
    *   Teacher user guide.
    *   Technical documentation.
    *   Clean and well-documented codebase ready for handover.

This sprint plan provides a more detailed breakdown. Durations are estimates and can be adjusted based on development velocity and complexity encountered. Regular review and adaptation of the plan will be necessary. 