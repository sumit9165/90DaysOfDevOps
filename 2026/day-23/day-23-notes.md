# Day 23 – Git Branching & Working with GitHub
----
## Task 1: Understanding Branches
1. What is a branch in Git?
- A branch is a separate version of your project.
- When you create a branch, you are saying: “I want to work on something new without touching the original version.”

2. Why do we use branches instead of committing everything to main?
- To develop features or fixes in isolation
- To avoid breaking stable code in main
- To allow parallel work by multiple developers
- To review and test changes before merging
- main should usually stay stable and production-ready.
- If you commit everything to main: You might break working code.
 Other developers cannot rely on it being stable.
 You mix unfinished work with production code.
- Instead:
- Create a branch
- Work safely
- Test
- Merge into main when ready
- This keeps main clean and stable.

3. What is HEAD in Git?
- HEAD tells Git: "This is where I am right now."
- It points to the branch or commit you are currently working on.

4. What happens to your files when you switch branches?
- When you change branches: Git changes your files
- It makes your folder look exactly like that branch

## Very Simple Summary

- Branch = separate version of your project

- We use branches to keep main safe

- HEAD = where you are now

- Switching branches changes your project files

---------------

## Task 2: Branching Commands — Hands-On








