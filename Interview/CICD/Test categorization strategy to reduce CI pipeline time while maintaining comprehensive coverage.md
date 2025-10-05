
<img width="1243" height="582" alt="image" src="https://github.com/user-attachments/assets/eaa50fff-4ffc-494d-b2b6-b01f786b019f" />


Developer
   │
   │  Commit changes ("many changes")
   ▼
Bug Branch (bug-213)
   │
   │  CI runs "fast" tests (unit tests, linting)
   ▼
Pull Request (PR)
   │
   │  Code Review + "medium" tests (integration / feature tests)
   ▼
Reviewer Approved?
   │
   ├─ No → Back to Developer for changes
   │
   └─ Yes → Run "slow" tests (full regression, E2E, deployment verification)
           │
           ▼
        Slow tests pass?
           │
           ├─ No → Back to Developer for fixes
           │
           └─ Yes → Merge into Main
                       │
                       ▼
                   Main Branch Updated & Stable

