# Phase 1 — Fundamentals & Setup (Weeks 1–2)

## Objective
Prepare a bulletproof development environment and sharpen core skills you already have as a .NET developer: advanced C#, modern JavaScript (ES2023+), and professional Git/GitHub workflows.

## Outcomes
- Advanced C# patterns and async programming mastered.
- Local dev environment configured for frontend and backend work.
- Confident Git workflow usage (feature branches, PRs, rebasing).
- Small, demonstrable exercises (console app + JS utilities).

## Week-by-week Plan
Week 1 — Advanced C#
- Day 1: Delegates, events, lambdas, expression-bodied members.
- Day 2: LINQ advanced queries and performance considerations.
- Day 3: Asynchronous programming: Task, async/await, ValueTask.
- Day 4: Newer C# features: records, pattern matching, init-only properties.
- Day 5: Memory management, Span<T>, pooling, and performance tips.

Week 2 — Git & JavaScript
- Day 6: Git branches, rebasing vs merging, resolving conflicts.
- Day 7: GitHub PR reviews, CODEOWNERS, branch protection rules.
- Day 8-10: JavaScript ES2023 basics: modules, promises, fetch API.
- Day 11-12: DOM basics, event delegation, browser devtools.
- Day 13-14: Mini-project polishing and write README.

## Mini Projects / Exercises
- Async File Processor (C# console app): read/process many files using async streams.
- Git Contribution: fork a small OSS repo, open a PR using a feature branch.
- JS Utility Library: small module with tests (e.g., date formatting, debounce).

## Acceptance Checklist
- [ ] VS Code / Visual Studio configured with recommended extensions.
- [ ] .NET 8 SDK installed and `dotnet` CLI tested.
- [ ] Git Flow or GitHub Flow practiced in at least one PR.
- [ ] Completed C# console app with unit tests.

## Resources (Focused)
- Microsoft C# guide (official): https://learn.microsoft.com/dotnet/csharp/
- Async/Await patterns: https://learn.microsoft.com/dotnet/csharp/programming-guide/concepts/async/
- LINQ documentation & examples: https://learn.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/
- .NET download & CLI: https://dotnet.microsoft.com/en-us/download / https://learn.microsoft.com/dotnet/core/tools/
- Pro Git book: https://git-scm.com/book/en/v2
- GitHub Learning Lab: https://lab.github.com/
- Modern JS: https://javascript.info/ and MDN: https://developer.mozilla.org/docs/Web/JavaScript

---

## Interview Topics & Most-Asked Questions (Phase 1)

Focus interview preparation on the bold topics below. Read the linked resources, implement small demos, and prepare short explanations with code examples.

1. Async/Await & Task vs ValueTask
   - Q: Explain async/await in C#. What is a Task and how does synchronization context affect it?
   - Learn: https://learn.microsoft.com/dotnet/csharp/programming-guide/concepts/async/
   - Deep read: Performance improvements & ValueTask: https://devblogs.microsoft.com/dotnet/performance-improvements-in-value-task/

2. LINQ & Deferred Execution
   - Q: What is deferred execution in LINQ? Differences between IQueryable and IEnumerable.
   - Learn: https://learn.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/linq-overview

3. .NET Garbage Collector & Memory Management
   - Q: How does the .NET GC work? What are generations and LOH?
   - Learn: https://learn.microsoft.com/dotnet/standard/garbage-collection/

4. Delegates, Events, and Design Choices
   - Q: Explain delegates and events in C#. When to prefer events vs callbacks?
   - Learn: https://learn.microsoft.com/dotnet/csharp/programming-guide/delegates-and-events/

5. Git Workflows, Rebase vs Merge
   - Q: Walk through resolving a complex merge conflict; when to rebase vs merge?
   - Learn: https://www.atlassian.com/git/tutorials/comparing-workflows / https://git-scm.com/book/en/v2

6. JavaScript: Event Loop, Closures, Modules
   - Q: Explain the event loop and difference between microtasks and macrotasks.
   - Learn: https://developer.mozilla.org/docs/Web/JavaScript/EventLoop

Practice approach:
- For each question, prepare a short code example and one-paragraph explanation. Convert each topic into a GitHub issue with checklist: Read → Implement demo → Write explanation → Add tests.