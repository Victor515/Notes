# Designing Good Data Architecture

1. some key areas we’ll return to throughout the book: **flexible and reversible decisions**, change management, and **evaluation of trade-offs**
2. *Magical thinking* culminates in poor engineering.
3. **Data architecture** is the design of systems to support the evolving data needs of an enterprise, achieved by flexible and reversible decisions reached through a careful evaluation of trade-offs
4. In short, operational architecture describes *what needs to be done*, and technical architecture details *how it will happen*
5. **Agility** is the foundation for good data architecture; it acknowledges that the world is fluid. Good data architecture is flexible and easily maintainable.



## Principles of Good Data Architecture:

Principle 1: Choose Common Components Wisely

- On the one hand, you need to focus on needs across the data engineering lifecycle and teams, utilize common components that will be useful for individual projects, and simultaneously facilitate interoperation and collaboration. On the other hand, architects should avoid decisions that will hamper the productivity of engineers working on domain-specific problems by forcing them into one-size-fits-all technology solutions.

Principle 2: Plan for Failure

Principle 3: Architect for Scalability

- An elastic system can scale dynamically in response to load, ideally in an automated fashion

Principle 4: Architecture Is Leadership

- As a data engineer, you should practice architecture leadership and seek mentorship from architects. Eventually, you may well occupy the architect role yourself.

Principle 5: Always Be Architecting

*Principle 6: Build Loosely Coupled Systems*

- When the architecture of the system is designed to enable teams to test, deploy, and change systems without dependencies on other teams, teams require little communica‐ tion to get work done. In other words, **both the architecture and the teams are loosely coupled**.
- Putting data and services behind APIs enabled the loose coupling and eventually resulted in AWS as we know it now

Principle 7: Make Reversible Decisions

- One of an architect’s most important tasks is to remove archite ture by finding ways to eliminate irreversibility in software designs -- Martin Fowler
- Aim for two-way doors whenever possible.

Principle 8: Prioritize Security

- Data engineers as security engineers

Principle 9: Embrace FinOps



## Major Architecture Concepts

1. A **domain** is the real-world subject area for which you’re architecting. A **service** is a set of functionality whose goal is to accomplish a task.
2. Multitier architecture: The notion is to separate data from the application, and application from the presentation
3. We don’t suggest an entire refactor but instead break out services.