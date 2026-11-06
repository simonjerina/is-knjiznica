# Project Proposal — Knjigomat

**Knjigomat** is a library management system for a small public, school or faculty library. Members search the catalog, borrow and reserve books, and manage their loans from the web or a phone. Librarians manage the collection and the circulation desk.

## Problem
Many smaller libraries (school, faculty, municipal branch) still run circulation on spreadsheets, paper cards or outdated desktop software. As a result:

- **Members** can't easily check whether a book is actually on the shelf, don't know when a reserved book becomes available, and forget due dates, which leads to late fees and frustration.
- **Librarians** spend a lot of time on routine work: checking availability, processing renewals by hand, chasing overdue books and working out fees.
- **The library** has no clear overview of what is borrowed, overdue or in demand, so buying decisions are guesswork.

Knjigomat puts the catalog, loans, a fair reservation queue and automatic due-date and fee handling into a single system. Members can use it from a phone right at the shelf.

## Users and roles
| Role | Description | What they can do |
|------|-------------|------------------|
| Member | Registered library user (student, resident…) | Search the catalog, see availability, reserve books, view and renew own loans, see own fees, use the mobile app and the AI assistant |
| Librarian | Library staff at the circulation desk | Manage books and physical copies, check copies out and in, manage reservations, waive or settle fees, view overdue reports |
| Admin | System administrator | Manage user accounts and roles, configure loan rules (loan length, max loans, max renewals, daily fee) |

Authorization is enforced on the server by role **and** ownership: a member can only see and change their own loans, reservations and fees.

## User stories

Format: *As a <role>, I want <goal>, so that <benefit>.* Priority: **M**ust / **S**hould / **C**ould / **W**on't.

| # | Story | Priority | Milestone | Issue |
|---|-------|----------|-----------|-------|
| 1 | As a **member**, I want to search the catalog by title, author, ISBN or genre and see how many copies are available, so that I know whether I can borrow a book right now. | M | midterm | #… |
| 2 | As a **librarian**, I want to add, edit and remove books and their physical copies, so that the catalog matches what is on the shelves. | M | midterm | #… |
| 3 | As a **librarian**, I want to check a copy out to a member and check it back in, so that every loan is recorded with a due date. | M | midterm | #… |
| 4 | As a **member**, I want to see my current loans and their due dates, so that I return books on time. | M | midterm | #… |
| 5 | As a **member**, I want to reserve a book that is currently unavailable, so that I get it as soon as a copy is returned. | M | midterm | #… |
| 6 | As a **member**, I want to renew a loan online, so that I can keep a book longer without visiting the library. | S | midterm | #… |
| 7 | As a **librarian**, I want to see a list of overdue loans with the fees owed, so that I can follow up with members. | S | midterm | #… |
| 8 | As an **admin**, I want to manage user accounts and roles and set loan rules, so that the library can adapt the system to its policies. | S | midterm | #… |
| 9 | As a **member**, I want to scan a book's ISBN barcode with my phone, so that I instantly see its availability and can reserve it. | M | final | #… |
| 10 | As a **member**, I want to describe what I'm looking for in plain language (e.g. "something about graphs for beginners"), so that I find relevant books without knowing exact titles. | M | final | #… |
| 11 | As a **member**, I want an AI assistant that can find a book, reserve it and renew my loans after I confirm, so that I can handle my library tasks in one conversation. | M | final | #… |
| 12 | As a **member**, I want to get a notification when my reserved book is ready or a loan is about to expire, so that I don't miss pickups or due dates. | C | final | #… |
| 13 | As a **member**, I want to pay fees online, so that I don't have to pay at the desk. | W | — | — |

### Acceptance criteria

**Story 1 — catalog search**
- **Given** the catalog contains "Introduction to Algorithms" with 3 copies, 1 of them on loan
- **When** a member searches for "algorithms"
- **Then** the book appears in the results showing *2 of 3 available*

**Story 3 — check-out and check-in**
- **Given** a member with fewer than the maximum allowed loans and no unpaid fees above the limit
- **When** a librarian checks out an available copy to them
- **Then** a loan is created with a due date of *today + loan length*, and the copy is marked as on loan
- **And when** the copy is checked in after the due date, **then** a late fee of *days late × daily fee* is recorded on the member's account

**Story 5 — reservation queue**
- **Given** all copies of a book are on loan
- **When** a member reserves it
- **Then** they are added to the end of the book's reservation queue and see their position
- **And when** a copy is returned, **then** it is held for the first member in the queue for 3 days, and nobody else can borrow it during that time

**Story 6 — renewal**
- **Given** a member has a loan that has been renewed fewer than the maximum number of times
- **When** they renew it and **no other member is waiting** in the reservation queue for that book
- **Then** the due date is extended by one loan period
- **Otherwise** the renewal is rejected with a clear reason (limit reached / book reserved by someone else / loan overdue)

**Story 11 — AI agent**
- **Given** a logged-in member asks: "Find me an available book about databases, reserve it and extend my current loans"
- **When** the agent has looked up the books and loans through the API
- **Then** it shows the planned actions (which book to reserve, which loans to renew) and performs **nothing** until the member confirms
- **And** every action runs with the member's own permissions, so a renewal the rules forbid is refused and explained

## Mobile app use case
The phone is used **at the shelf and on the go**:

- **Scan an ISBN barcode** with the camera to instantly see availability, the shelf location and the reservation queue, and to reserve the book with one tap.
- **"My loans"** on the go: due dates, one-tap renewals and the status of reservations.
- **Notifications** (could-have) when a reserved book is ready for pickup or a loan is about to expire.

These things only make sense on a phone: you're standing in front of the shelf or away from your computer when a book is due.

## AI features
- **LLM feature: natural-language catalog search.** The member types a free-text request ("an easy intro to machine learning in Slovenian"). The LLM turns it into a structured filter (topic/genre, keywords, language, level) that runs against our search endpoint. Results are ranked and get a one-line "why this book" explanation.
- **Agent task:** *"Find an available book on a topic I describe, reserve it, and renew my loans that are about to expire — after I confirm."* The agent uses our API (search catalog → check availability → list my loans → reserve → renew). Write actions require explicit confirmation from the user, enforced in code, and every call uses the member's own token.
