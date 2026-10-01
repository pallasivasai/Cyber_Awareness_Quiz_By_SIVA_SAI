# Cyber Awareness Quiz

**Cyber Awareness Quiz** is a full-stack cybersecurity learning and assessment web application built with React, TypeScript, Vite, Tailwind CSS, and Supabase.

Authenticated users choose a difficulty, complete a randomized 10-question cybersecurity quiz in fullscreen mode, track completion time, save the result, view a leaderboard, and generate a verifiable certificate when the score is at least **75%**.

## Quiz flow

```text
Login / Signup
      |
      v
Choose difficulty
Easy / Medium / Hard
      |
      v
Query difficulty-specific questions
      |
      v
Shuffle + select 10
      |
      v
Fullscreen quiz + timer
      |
      v
Result + saved score
      |
      +------> Leaderboard
      |
      +------> 75%+ certificate
                    |
                    v
             Verification page
```

## Main features

### Authentication

Supabase Auth is used for user access, and the quiz home page loads the authenticated user's profile/username.

### Difficulty modes

The UI provides:

- **Easy** — beginner friendly
- **Medium** — moderate challenge
- **Hard** — expert level

The selected difficulty is used when loading questions and is stored with certificate data.

### Randomized ten-question quiz

The application reads `quiz_questions` filtered by the selected difficulty, shuffles the returned set in the browser, and uses the first **10** questions.

Each question provides four choices and a stored correct answer.

### Fullscreen mode

Fullscreen is required during the quiz.

The application listens for `fullscreenchange`, shows a warning when fullscreen is exited, and intercepts common keys such as Escape and F11 while the quiz is active.

### Timer and score

The quiz records elapsed time from the start of the attempt and counts correct answers.

The result page displays:

- grade
- correct answers / total
- percentage
- time taken
- difficulty

Scores are written to the `quiz_scores` table.

### Certificate generation

A certificate button appears when the result is **75% or higher**.

The current flow:

1. generate a unique certificate ID
2. save it to the `certificates` table
3. open the certificate view
4. invoke the `send-certificate-email` function
5. allow browser download as PNG
6. allow printing

Scores of **70% or higher** also trigger the result-page confetti effect.

### Certificate verification

The certificate includes a verification route:

```text
/verify/<certificateId>
```

`VerifyCertificate.tsx` looks up the exact certificate ID in Supabase and displays the stored recipient, score, question count, difficulty, and issue date.

### Leaderboard

The leaderboard loads up to **20** saved scores.

Ordering is:

1. higher score first
2. lower completion time first when scores are equal

Certificate holders can have their certificate opened from the leaderboard.

## Database

The project uses Supabase/PostgreSQL tables including:

- `profiles`
- `quiz_questions`
- `quiz_scores`
- `certificates`

The repository migrations include Row Level Security configuration and certificate verification policies.

## Key files

| File | Responsibility |
|---|---|
| `src/components/quiz/QuizHome.tsx` | Difficulty selection, quiz rules and leaderboard entry |
| `src/components/quiz/QuizScreen.tsx` | Question loading, fullscreen, timer and answer flow |
| `src/components/quiz/QuizResult.tsx` | Score persistence, grading and certificate flow |
| `src/components/quiz/Leaderboard.tsx` | Leaderboard and certificate access |
| `src/components/quiz/Certificate.tsx` | Certificate rendering, PNG download and print |
| `src/pages/VerifyCertificate.tsx` | Certificate verification |
| `src/hooks/useAuth.tsx` | Authentication state |
| `src/integrations/supabase/client.ts` | Supabase client |
| `supabase/functions/send-certificate-email/index.ts` | Certificate email function |
| `supabase/migrations/` | Database schema and security policies |

## Technology stack

- React
- TypeScript
- Vite
- Tailwind CSS
- shadcn/Radix UI
- Supabase Auth
- Supabase PostgreSQL
- Supabase Row Level Security
- Supabase Edge Functions
- React Router
- `canvas-confetti`
- `html2canvas`

## Run locally

```bash
git clone https://github.com/pallasivasai/Cyber_Awareness_Quiz_By_SIVA_SAI.git
cd Cyber_Awareness_Quiz_By_SIVA_SAI
npm install
npm run dev
```

The Supabase client/environment configuration used by the project must also be available for database and edge-function features.

## Current behavior notes

- Exactly 10 questions are used for each quiz attempt after difficulty filtering and shuffling.
- Fullscreen is required during the active quiz.
- Certificate eligibility is 75% or higher.
- Certificate images are generated in the browser with `html2canvas`.
- Verification is performed by exact certificate ID lookup.
- Leaderboard ordering uses score first and completion time second.

## Links

- [GitHub Repository](https://github.com/pallasivasai/Cyber_Awareness_Quiz_By_SIVA_SAI)
