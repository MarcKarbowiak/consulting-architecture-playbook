# Case Study: YogaFlow — Offline-First Studio Management App

## 1. Executive Summary

YogaFlow is an Android mobile application built with Cordova and SQLite to manage student attendance and credit tracking in an offline-first studio environment.

---

## 2. Business Context & Constraints

### Problem
Attendance and credits were previously tracked using paper cards and pen, leading to errors and lack of reporting visibility.

### Objectives
- Digitize attendance tracking
- Safely manage credit balances
- Enable student self-registration
- Keep system simple and cost-efficient

### Constraints
- No cloud infrastructure
- Limited IT support
- Single-location deployment

---

## 3. My Role & Scope

- Designed overall application architecture
- Implemented Android app using Cordova
- Designed relational SQLite schema
- Built transactional credit tracking logic
- Provided operational guidance for backup procedures

---

## 4. High-Level Architecture

- Cordova-based Android app
- SQLite local database
- Periodic manual backup process

---

## 5. Key Architectural Decisions & Trade-offs

- Offline-first design over cloud dependency
- Local SQLite for simplicity and reliability
- Minimal operational footprint to match business scale

---

## 6. Failure Modes & Mitigations

- Device failure → periodic backup
- Credit miscalculation → transactional updates
- Data corruption → structured schema + validation

---

## 7. Security & Operational Considerations

- Local data protection practices
- Role separation (admin vs student)
- Controlled user deactivation rather than deletion

---

## 8. What I Would Refine Today

- Introduce optional lightweight cloud sync
- Add automated encrypted backup option
- Improve observability around credit anomalies

---

## 9. Outcomes & Lessons

- Eliminated manual tracking errors
- Improved credit accuracy
- Increased operational efficiency

This project illustrates constraint-driven greenfield design: building only what the business needed, no more, no less.

