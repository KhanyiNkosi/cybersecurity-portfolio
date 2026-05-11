# Application Security Architecture: Family Task Manager

**Author:** Khanyisile Natashie Nkosi  
**Tech Stack:** React 18, Next.js 14, TypeScript, Supabase (PostgreSQL)  
**Security Focus:** Role-Based Access Control (RBAC), Row-Level Security (RLS), Least Privilege, Secure Authentication  

---

## Project Overview
**Family Task Manager** is a live, full-stack SaaS application designed to help families manage daily chores, track tasks, and distribute incentives. 

Because the application handles private family dynamics, user emails, and parental controls, securing user data was a primary design requirement. I architected the application from the ground up using the **Principle of Least Privilege**, ensuring that children cannot access parental controls, and families cannot view or modify other families' private data.

---

## Secure Architecture & Threat Modeling

To secure this application, I implemented defense-in-depth across three main layers:

```text
[ User Interface ]  ---> Enforces UI-level restrictions (Hiding Parent Admin dashboards from Child views)
        |
[ Authentication ]  ---> JWT-based secure sessions handled by Supabase Auth (MFA-ready)
        |
[ Database Layer ]  ---> PostgREST API secured by PostgreSQL Row-Level Security (RLS) policies (The ultimate fallback)
