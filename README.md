#User Portal:
https://sentinelsc.schoolpool.ca/
#Admin Portal:
https://sentinelsc.schoolpool.ca/admin

# Test Manual:
https://github.com/JakeC021/schoolcarpool/wiki#3-parent-portal

# Test feedback:
https://shorturl.at/fA4ts

---

# SchoolPool — Test Team Briefing

> Pre-launch manual test · April 2026

Thank you for helping us test SchoolPool before it goes live. This document gives you everything you need to understand the system, navigate both portals, and submit useful feedback.
## Table of contents

- [What is SchoolPool?](#what-is-schoolpool)
- [Design goals](#design-goals)
- [Parent portal](#parent-portal)
  - [Registration](#1-registration)
  - [Dashboard](#2-dashboard)
  - [Posting a ride request (passenger)](#3-posting-a-ride-request-passenger)
  - [The board (driver)](#4-the-board-driver)
  - [Trip lifecycle](#5-trip-lifecycle)
  - [Account standing](#6-account-standing-no-show-ladder)
  - [Profile](#7-profile)
  - [Points and leaderboard](#8-points-and-leaderboard)
- [Admin portal](#admin-portal)
- [Test environment](#test-environment)
- [Flows to prioritise](#flows-to-prioritise)
- [How to submit feedback](#how-to-submit-feedback)

---

## What is SchoolPool?

SchoolPool is a **non-profit school carpooling platform** that connects families at the same school who share overlapping commutes. The goal is to reduce car trips, ease traffic around schools, and build community — not to make money. There are no payments in the app; participation is incentivised through a points system.

The system has **two separate portals**:

| Portal | Audience | Device |
|--------|----------|--------|
| **Parent portal** | Families (passengers and drivers) | Mobile (430 px) |
| **Admin portal** | School staff | Desktop |

---

## Design goals

| Goal | What it means in practice |
|------|---------------------------|
| **Safe for children** | Full street addresses are hidden until a booking is confirmed. Drivers carry a visible ID code to verify identity at pickup. |
| **Low friction** | Parents can post a ride request in under a minute. Drivers browse a board and confirm with one tap. |
| **School-scoped** | Registration requires an email address on the school's domain. Parents cannot join a school they don't belong to. |
| **Accountable** | No-shows are tracked and escalate automatically (warning → suspension → ban). Every admin action is logged. |
| **Admin-controlled** | School staff can manage user standing, edit email templates and agreements, and view a full audit trail — without developer involvement. |

---

## Parent portal

The parent portal is designed for **mobile browsers**. Use a phone or a narrow browser window (≤430 px) for the most accurate experience.

### 1. Registration

New parents register in four steps:

1. **Agreements** — accept the platform's driver and passenger agreements
2. **Personal details** — name, email (must match school domain), password
3. **Children** — add each child's name and year group
4. **Address** — save a home address (used as default pickup location)


---

### 2. Dashboard

The home screen after login. Shows:

- A greeting and the school name
- Action buttons: **Post a request** (passenger) and **Browse board** (driver)
- Community stats: rides shared, CO2 saved (assumes petrol/gas cars), and a leaderboard
- Account status banners if the user has been warned or suspended

---

### 3. Posting a ride request (passenger)

A parent who needs a ride posts a request:

1. Choose **morning** (to school) or **afternoon** (from school)
2. Pick a date and preferred pickup time
3. Select a child and number of seats
4. Confirm the pickup address

The full street address is stored but **not shown to drivers** until a booking is confirmed — only the postal code centroid appears on the board. Requests expire automatically if unclaimed.

> **Note:** Requests cannot be posted after a nightly cutoff hour (configurable by admin). If the gate is active you will see a message.

---

### 4. The board (driver)

A parent who wants to drive browses open requests. They see:

- Direction, date, time, seats needed
- Approximate pickup area (postal code — no street address)

The driver filters by direction (morning/afternoon) and confirms a request. Confirmation:
- Creates a booking
- Reveals the full pickup address to the driver
- Sends confirmation emails to both parties

---

### 5. Trip lifecycle

Once a booking is confirmed:

```
Confirmed → In progress → Completed
```

| Action | Who | When |
|--------|-----|------|
| **Begin trip** | Driver | After picking up the passenger |
| **Complete trip** | Driver | After dropping off the passenger |
| **Rate driver** (1–5 stars) | Passenger | After completion |
| **Report no-show** | Driver | If passenger is not at pickup |
| **Report an issue** | Either | After completion |

---

### 6. Account standing — no-show ladder

No-shows escalate automatically based on frequency:

| Threshold | Consequence |
|-----------|-------------|
| 2 no-shows in a month, or 2 consecutive | **Warned** |
| 3 no-shows in a month | **Suspended** (7 days) |
| 2nd suspension | **Suspended** (30 days) |
| 3rd+ suspension | **Banned** |

Each status has a dedicated screen explaining the consequence and what the user should do next. Emails are sent at every status change.

---

### 7. Profile

Parents manage:

- Personal information (name, email, phone)
- Children (name, year group)
- Saved addresses
- Vehicles (**required** before a parent can drive — make, model, colour, plate, seats)
- Driver ID code (a short alphanumeric code shown to passengers at pickup for identity verification)

---

### 8. Points and leaderboard

- Drivers earn **10 points** per completed trip
- Passengers earn **5 points** per completed trip
- A community leaderboard shows the top contributors at the school

---

## Admin portal

The admin portal is **desktop-only** and used exclusively by school staff. Staff accounts are created by the school admin and cannot be self-registered.

### Role permissions

| Role | Access |
|------|--------|
| **Admin** | Full access to everything |
| **Moderator** | Dashboard, Users, Audit log |
| **Analyst** | Dashboard only |

### Sections

| Section | What staff can do |
|---------|-------------------|
| **Dashboard** | Live stats (users, rides, bookings, no-shows) and a recent activity feed |
| **Users** | View all parent accounts; warn, suspend, ban, or reinstate users |
| **Audit log** | Full searchable log of every action by parents and staff — filter by actor, date, or action type |
| **Email templates** | Edit the content of system emails (welcome, password reset, booking confirmation, no-show notice, account status change) |
| **Agreements** | Edit the driver and passenger agreements parents accept at registration |
| **System settings** | Set school name, email domain, and the nightly request cutoff hour |
| **Team** | Create and manage staff accounts; assign roles; remove staff |

---

## Test environment

| Item | Value |
|------|-------|
| Parent portal | `https://sentinelsc.schoolpool.ca/` |
| Admin portal | `https://sentinelsc.schoolpool.ca/admin` |
| School email domain | `@edu.sd45.bc.ca` |
| Emails | Real emails are sent — check your inbox during the test |

> You can register multiple parent accounts using different email addresses at the same domain (e.g. `driver1@outlook.com`, `passenger1@google.com`).

---

## Flows to prioritise

Work through these in order if possible. Each builds on the last.

1. **Register** a new parent account with an email address
2. **Post a ride request** as a passenger (morning, next available date)
3. **Register a second account**, add a vehicle, then **browse the board** and **confirm the booking**
4. **Start the trip** as the driver → **complete the trip**
5. **Rate the driver** as the passenger
6. **Check My trips** on both accounts — verify trip history is correct
7. **Admin:** log in, find both test users, change one user's status → verify the email arrives
8. **Admin:** edit an email template, trigger it, verify the edited content arrives in the inbox
9. **Try edge cases:**
   - Register with a non-school email (should be rejected)
   - Cancel a pending request
   - Cancel a confirmed booking as the passenger
   - Report a no-show as the driver
   - Log in with wrong password (check error message)

---

## How to submit feedback

Use the feedback form (link shared separately) to log each finding as you go. **One submission per issue** — this makes triage much easier.

For each bug or observation, please include:

- The screen or feature where it occurred
- What you did (steps to reproduce)
- What you expected to happen
- What actually happened
- Your device and browser (e.g. iPhone 14 / Safari, Windows 11 / Chrome)

Screenshots are very helpful — attach them in a follow-up email if the form doesn't accept uploads.

---

*Your feedback directly shapes whether this app is ready for real families. Thank you.*
