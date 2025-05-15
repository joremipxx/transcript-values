# Technology Stack

This document outlines the definitive technology stack for the Paradox Coaching Transcript Analysis Web App.

## Frontend

*   **Framework:** Next.js

## Backend

*   **Runtime:** Node.js
*   **Platform:** Vercel Functions
*   **Responsibilities:**
    *   Handles transcript submissions.
    *   Interacts with the OpenAI API.
    *   Manages data storage (saving and retrieving analyses).
    *   Integrates with Clerk webhooks or APIs to verify authenticated teacher requests.
    *   Triggers PDF generation.

## Database

*   **Type:** PostgreSQL (or similar relational database like MySQL)
*   **Recommendation:** A managed service (e.g., Vercel Postgres, Supabase, or Render Postgres).
*   **Purpose:** Stores coaching transcripts and AI analysis results persistently, linked to a teacher user ID provided by Clerk.

## Authentication

*   **Service:** Clerk
*   **Purpose:** Handles user registration (if needed later) and login flows, providing secure user management and allowing the backend to verify authenticated requests from teachers.

## AI Integration

*   **Service:** OpenAI API
*   **Model:** `gpt-4o`
*   **Purpose:** Sophisticated transcript analysis and evaluation.

## PDF Generation

*   **Library:** Puppeteer (or equivalent Node.js library/service)
*   **Execution:** Server-side, called from the backend to reliably generate PDF reports.

## Deployment

*   **Platform:** Vercel
*   **Components Hosted:** Frontend (Next.js), Backend (Node.js Vercel Functions), and potentially the Database.

## Input Handling

*   **Methods:**
    *   Direct Text Area input.
    *   File processing for `.txt` and `.docx` formats on the backend. 