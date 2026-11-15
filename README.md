# Omran Academy

An Arabic-first online learning platform for teachers, with a full teacher console, protected online exams, live sessions, chat, payments and reports. Built with Flutter and Supabase, and shipped to the web, iOS and Android from a single codebase.

**Live demo:** https://omran-academy-eg.web.app/

![Omran Academy on web and mobile](docs/screenshots/showcase/01-hero.png)

## Table of contents

- [Overview](#overview)
- [The problem it solves](#the-problem-it-solves)
- [Screenshots](#screenshots)
- [Features](#features)
- [Exam integrity](#exam-integrity)
- [Architecture](#architecture)
- [Tech stack](#tech-stack)
- [Backend](#backend)
- [Security model](#security-model)
- [Push notifications](#push-notifications)
- [Engineering challenges and fixes](#engineering-challenges-and-fixes)
- [Responsive design](#responsive-design)
- [Project structure](#project-structure)
- [Contact](#contact)
- [License](#license)

## Overview

Omran Academy lets a teacher run a complete online academy under their own name.

- **Students** browse courses, register and pay, follow lessons, take exams, attend live sessions, chat with the teacher, track their own progress and download certificates.
- **The teacher** manages everything from the same app: sections, grades and curricula, courses with units and lessons, exams, live sessions, students, enrollment requests, activation codes, coupons, payments, announcements and reports.

The interface is Arabic with full right-to-left support and adapts from phones to large desktop screens.

I built it over more than 70 days of focused work. I did not want to ship something quick. I wanted a product I could hand to a real client, so I spent that time thinking through what a teacher and their students actually run into, and solving those problems one by one.

This repository is a showcase and documentation of how the system is built. The application source code is private.

## The problem it solves

The biggest problem was registration. Many students simply could not finish signing up or paying. The platform gives them four independent ways in, so nobody is blocked:

1. **Pay and enter.** The student registers, pays online, and is enrolled in the course immediately. The enrollment appears on the teacher's side right away.
2. **Activation code.** The student messages the teacher. The teacher taps **Create code and send**, and the student enters the code (`XXXX-XXXX-XXXX`) to unlock the course. The teacher can also activate the student directly.
3. **Teacher approval.** A request lands in the teacher's **Awaiting your decision** list. One tap on **Approve and activate** enrolls the student. The teacher can also reject it or open a chat with the student first.
4. **Teacher creates the account.** The teacher adds the student from inside the platform with a name, email, phone, grade and password, optionally enrolls them in a course, and sends the details to the student.

Online payment can be switched on or off by the teacher. When it is off, the checkout screen explains this and points the student to the code and approval routes instead of leaving them stuck.

![Payment, activation codes and approvals](docs/screenshots/showcase/03-payment-and-codes.png)

## Screenshots

### Student onboarding

![Student onboarding](docs/screenshots/showcase/02-student-onboarding.png)

### Teacher dashboard on mobile

![Teacher dashboard](docs/screenshots/showcase/04-teacher-dashboard.png)

### Create and manage

![Create and manage](docs/screenshots/showcase/05-teacher-create-and-manage.png)

<details>
<summary>All screens individually</summary>

| | | |
|:-:|:-:|:-:|
| <img src="docs/screenshots/individual/student-landing.png" width="220"/><br/>Landing page | <img src="docs/screenshots/individual/student-register-details.png" width="220"/><br/>Registration: details | <img src="docs/screenshots/individual/student-register-account.png" width="220"/><br/>Registration: account |
| <img src="docs/screenshots/individual/student-welcome-owl.png" width="220"/><br/>Welcome guide | <img src="docs/screenshots/individual/student-payment-notice.png" width="220"/><br/>Payment notice | <img src="docs/screenshots/individual/student-payment-summary.png" width="220"/><br/>Summary and activation code |
| <img src="docs/screenshots/individual/teacher-courses.png" width="220"/><br/>Courses | <img src="docs/screenshots/individual/teacher-exams.png" width="220"/><br/>Exams | <img src="docs/screenshots/individual/teacher-students-list.png" width="220"/><br/>Students and progress |
| <img src="docs/screenshots/individual/teacher-enroll-requests.png" width="220"/><br/>Enrollment requests | <img src="docs/screenshots/individual/teacher-add-student.png" width="220"/><br/>Add a student | <img src="docs/screenshots/individual/teacher-new-course.png" width="220"/><br/>New course |
| <img src="docs/screenshots/individual/teacher-more-menu.png" width="220"/><br/>More (1) | <img src="docs/screenshots/individual/teacher-more-menu-2.png" width="220"/><br/>More (2) | |

</details>

## Features

### Student experience

**Account**
- Sign up with email and a one-time code, or with Google
- Three-step registration (details, account, payment) with a progress indicator
- International phone field with country codes and live password-rule checks
- A short welcome guide after the first sign-in
- Interrupted registration resumes with the previously chosen course

**Discovery**
- Course catalog browsed by section
- Search that matches every typed word anywhere in the course name, with a search history
- Course cards showing price, discount, lesson count and rating

**Learning**
- Home screen with overall progress and a **Continue where you left off** shortcut
- Lessons as video, PDF or live session, with attachments
- Personal notes on each lesson
- Study time tracking
- Course ratings with comments
- Certificates generated as PDF

**Exams**
- One question at a time with a countdown timer
- Multiple choice, true or false, and essay questions
- Instant result and, when the teacher allows it, the correct answers

**Chat and notifications**
- Chat with the teacher using text, images and voice messages
- Delivery and read status
- Push notifications and an in-app notification center
- Option to hide last seen

### Teacher console

**Dashboard**
- Student count, published courses and average score at a glance
- Quick actions for new course, sessions, announcements, messages, activity log, exam monitoring, reports and students
- A **Needs your attention** area for pending items

**Content**
- Sections, grades and curricula managed by the teacher
- Courses with title, grade, curriculum, price, discount percentage, cover image and description
- Units and lessons, each lesson can be hidden or shown
- Draft and published states

**Exams**
- Duration, number of attempts, pass percentage, shuffled questions, deadline, late submission, answer review
- Maximum number of times a student may leave the exam screen
- Secure mode, cover image, and three question types
- Live monitoring, per-exam reports, essay grading and review of flagged results

**Live sessions**
- Scheduled time and duration, with files attached to the session
- Periodic "are you still with us?" checks at an interval the teacher sets
- Raised hands, muting, attendance, and time spent in the session per student
- A report for every session

**Students**
- Add, edit and remove students, and follow each student's progress
- Private teacher notes per student
- Grade change requests with approve or reject
- Filters for students, codes, subscribers and history

**Payments and enrollment**
- Payment records with refunds and refund review
- Coupons, as a percentage or a fixed amount, with usage limit, expiry and an optional course scope
- Activation codes and enrollment requests

**Reports and administration**
- Overview, students, exams and courses reports
- Activity log showing who studied, took exams or signed in
- Export to Excel and PDF
- Platform settings: name, branding, sections and grades, card images and titles
- Announcements to all students

**Chat**
- Text, images and voice messages, with reactions
- Edit and delete messages, pin and mute conversations
- Delivery and read status, and last seen

## Exam integrity

Exams are protected in layers rather than by a single mechanism.

- **Clipboard shortcuts disabled.** Copy, paste, cut and select-all are blocked, including the Insert-key variants.
- **Bulk paste blocked.** A text input formatter rejects any single edit that adds a large block of text, so pasting from the keyboard suggestions bar fails too. Normal typing and short suggestions still work.
- **Watermark.** The student's name and email are tiled across the exam page without blocking touch, so any captured image can be traced back.
- **Exam lock.** On the web a dedicated script locks the page during the exam. On Android the native layer protects the screen.
- **Exit counter.** The teacher sets how many times a student may leave the exam. Exits and time spent away are recorded.
- **Live monitoring.** The teacher sees who is inside an exam right now and who has not finished.
- **Flagged results.** Suspicious attempts are collected for review.
- **Server-side logic.** Starting, aborting and scoring an exam, and the exam keys, are handled by database functions. The student's device is not the source of truth.
- **Shuffling, attempts and deadlines** reduce answer sharing.

No web application can fully prevent a photo of a screen taken by another device. The goal is to make cheating hard, traceable and visible to the teacher.

## Architecture

The project follows Clean Architecture, organized feature-first. Each feature has its own `data`, `domain` and `presentation` layers, with shared code kept in `core` and `shared`.

```mermaid
flowchart LR
    P["Presentation<br/>pages and cubits"] --> D["Domain<br/>entities, repository contracts, use cases"]
    DA["Data<br/>models, data sources, repository implementations"] --> D
    DA --> S[("Supabase<br/>Auth, Postgres, RPC, Realtime, Storage, Edge Functions")]
    P -.-> SH["Shared<br/>layout, widgets"]
    P -.-> C["Core<br/>config, errors, offline queue, push, platform"]
    DA -.-> C
```

The dependency rule is `presentation -> domain <- data`. The domain layer is plain Dart with no Flutter or Supabase imports, and the UI never talks to Supabase directly.

### How a request flows

```mermaid
sequenceDiagram
    participant UI as Page
    participant CU as Cubit
    participant UC as Use case
    participant RP as Repository
    participant DS as Remote data source
    participant SB as Supabase
    UI->>CU: user action
    CU->>UC: call
    UC->>RP: repository contract
    RP->>DS: fetch or save
    DS->>SB: from() / rpc() / functions.invoke()
    SB-->>DS: rows or error
    DS-->>RP: model
    RP-->>UC: entity or failure
    UC-->>CU: result
    CU-->>UI: new state
```

### Enrollment flow

```mermaid
flowchart TD
    A[Student opens a course] --> B{Chooses a route}
    B -->|Pays online| C[Payment confirmed]
    B -->|Has a code| D[Enters activation code]
    B -->|Contacts the teacher| E[Request appears for the teacher]
    E --> F{Teacher decides}
    F -->|Approve and activate| G
    F -->|Create code and send| D
    F -->|Reject| H[Request closed]
    I[Teacher adds the student] --> G
    C --> G[Student enrolled]
    D --> G
```

### Design decisions

- **Cubit** for state management, with immutable states and models.
- **Dependency injection** with repositories as lazy singletons and cubits created per screen.
- **go_router** with guards for authentication and role (student or teacher).
- Features do not import each other. They communicate through the domain layer or shared code.
- **Use cases** are used for the operations that carry real logic (payment, starting and scoring an exam), not for every call.
- **Unified error handling.** Exceptions become typed failures in the repository, then a clear Arabic message for the user.
- **Optimistic updates.** The screen updates immediately, and if saving fails it returns to the correct state automatically.
- **Conditional imports** give each platform its own implementation of web-only features such as the exam lock and the meeting tab.
- **Realtime streams.** Database change events are exposed as streams from the data source and closed together with their cubit.

## Tech stack

| Layer | Technology |
|---|---|
| App framework | Flutter and Dart (Material 3), RTL Arabic |
| State management | Cubit (flutter_bloc) |
| Dependency injection | get_it |
| Models | freezed and json_serializable |
| Navigation | go_router |
| Backend | Supabase: Auth, Postgres, RPC, Realtime, Storage, Edge Functions |
| Payments | Paymob, through Edge Functions |
| Push notifications | Firebase Cloud Messaging |
| Authentication | Email with one-time code, Google Sign-In |
| Documents | pdf, printing, excel, file_picker, share_plus |
| Voice | record, audioplayers |
| Fonts | google_fonts |
| Local storage | shared_preferences |

## Backend

All server logic lives in Supabase.

**Main tables**

| Area | Tables |
|---|---|
| Users | `profiles`, `device_tokens` |
| Structure | `sections`, `grade_levels`, `curricula`, `grade_requests` |
| Courses | `courses`, `units`, `lessons`, `lesson_content`, `lesson_progress`, `lesson_notes`, `study_time`, `ratings` |
| Exams | `exams`, `exam_questions`, `exam_keys`, `exam_results`, `exam_missing_reviews` |
| Assignments | `assignments` |
| Sessions | `sessions`, `session_links` |
| Chat | `messages`, `message_reactions`, `chat_pins`, `chat_mutes`, `chat_prefs` |
| Money and access | `payments`, `coupons`, `activation_codes`, `activation_log`, `enrollments`, `course_requests` |
| Communication | `announcements`, `announcement_seen`, `notifications` |
| Operations | `activity_log`, `teacher_notes`, `platform_settings`, `faqs` |

**Storage buckets:** `media` for course, exam and profile images, and `chat-media` for chat attachments.

**Database functions (RPC)** are used for the sensitive or heavy operations, including `start_exam`, `abort_exam`, `exam_key`, `exams_page`, `exams_in_progress`, `exams_unfinished`, `exam_question_counts`, `review_flagged_results`, `join_session`, `session_report`, `approve_request`, `reject_request`, `revoke_code`, `chat_overview`, `mark_delivered`, `mark_chat_seen`, `edit_message`, `delete_message`, `touch_presence`, `get_last_seen`, `set_hide_last_seen`, `set_my_avatar`, `students_never_messaged` and `activity_extras`.

**Edge Functions**

| Function | Purpose |
|---|---|
| `create-payment` | Creates a Paymob payment for a course and method |
| `refund-payment` | Refunds a payment and optionally removes course access |
| `manage-student` | Creates, updates and deletes student accounts on the teacher's behalf |

## Security model

- Row Level Security isolates each user's data in the database.
- Sensitive operations run on the server: payments, refunds, student account management, and exam keys and scoring.
- The client carries only the public Supabase key and the Firebase client options. Service credentials and payment secrets live in server-side function secrets.
- Course prices are calculated by the same rule on the client and on the server, so the amount displayed matches the amount charged.
- Passwords require at least eight characters with lowercase, uppercase and a digit.
- Keys, environment files and signing files are excluded from version control.

## Push notifications

Notifications use Firebase Cloud Messaging. Device tokens are registered when a user signs in and removed when they sign out. Tapping a notification opens the screen it refers to, and falls back to the notification center when there is no specific destination. While the app is open, FCM does not draw a system notification, so the app shows an in-app banner instead. The notification center loads in pages and supports marking as read and deleting.

## Engineering challenges and fixes

These are real problems I hit while building the platform, and how I solved them.

| Problem | Fix |
|---|---|
| Students could not finish registration or payment | Added four independent enrollment routes (online payment, activation code, teacher approval, teacher-created account) and the ability to resume an interrupted registration |
| Cheating in online exams | Layered protection: clipboard shortcuts disabled, bulk paste blocked, watermark, exam lock, exit counter, live monitoring and server-side scoring |
| Pasting through keyboard suggestions bypassed the shortcut block | Added an input formatter that rejects any single edit adding more than a small number of characters |
| Activity data was lost when the connection dropped | Built a persistent offline queue: bounded size, seven-day expiry, retry with increasing delays, and different handling for permanent and temporary errors |
| Items added while the queue was syncing could be overwritten | The queue re-reads its stored list before every write, so nothing enqueued during a send is lost |
| Browsers blocked the payment and meeting tabs | Pages are opened directly from the user's tap. For meetings, a blank tab opens at the moment of the tap and is redirected after the join is recorded |
| A failed payment closed the page and left the student confused | The failure now shows inside the page with a retry. Enrollment is granted by the payment itself, so a failed follow-up step cannot undo it |
| Loading every exam with all its questions was slow | The server returns question counts, and the questions load only when needed. Unloaded questions are never overwritten with empty data |
| Messages stayed unread and the sender never saw a read mark | Fixed the read and delivery marking so both sides stay consistent |
| Duplicate messages appeared when realtime and local updates overlapped | Added de-duplication when merging realtime events with local state |
| Push messages did not appear while the app was open | Added an in-app banner for foreground notifications |
| A late and suspicious attempt was counted twice in reports | A flagged attempt now counts once, in the flagged card only |
| Course search was too rigid and the search history filled up with partial entries | Search matches every typed word anywhere in the course name, and incremental typing replaces the previous history entry instead of adding a new one |
| Displayed price could differ from the charged price | The client uses the same rounding and minimum-price rule as the server function |
| A slow or unavailable backend could block app start | Startup does not wait on non-essential loading, and failures show a retry screen and a health banner |
| Backend errors appeared as raw technical messages | All errors are mapped to short Arabic messages that also say what to do next |

## Responsive design

The same screens adapt to three size classes (compact under 600 px, medium under 1024 px, and expanded). Grids compute their column count from the available width, spacing grows with the screen, and the navigation moves from a bottom bar on phones to a top bar on larger screens. The app has been used on desktop browsers and iPhone Safari.

## Project structure

```
lib/
├── main.dart
├── app/
│   ├── di/                 Dependency injection
│   ├── router/             go_router, auth and role guards
│   └── shell/              Navigation shell
├── core/
│   ├── config/             Environment and Firebase options
│   ├── constants/          Table, RPC, bucket and function names
│   ├── error/              Failures and error mapping
│   ├── offline/            Persistent activity queue
│   ├── push/               FCM service and notification routing
│   ├── platform/           Web and mobile implementations
│   ├── export/             Excel, PDF and text export
│   └── utils/              Validators, formatters, price rules
├── shared/
│   ├── theme/
│   ├── layout/
│   ├── widgets/
│   └── cubits/
└── features/
    ├── auth/               profile/            structure/
    ├── catalog/            course_content/     exams/
    ├── assignments/        sessions/           chat/
    ├── payments/           students/           announcements/
    ├── notifications/      activity/           reports/
    ├── certificates/       home/               settings/
```

Each feature contains `data/`, `domain/` and `presentation/`. As an example, `exams` has data sources and models for exams, questions and results, a repository contract with use cases such as start, submit and abort, and presentation code for taking, editing, monitoring and reviewing exams.

## Contact

If you run an academy or teach online and want a platform with your own name and look, get in touch.

- **Ahmed Osama El-shakoushy**
- LinkedIn: https://www.linkedin.com/in/ahmed-osama-816069399/
- Phone: 01068397464
- Live demo: https://omran-academy-eg.web.app/

## License

Copyright (c) 2026 Ahmed Osama El-shakoushy. All rights reserved.

This repository is published for showcase and documentation purposes. The application source code is private and may not be copied, modified or redistributed without written permission.
