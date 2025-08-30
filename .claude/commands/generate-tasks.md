# Rule: Generating a Task List from a PRD
The user provides the location for PRD Sub-Directory under @tasks/ - it contains two files - a high level brief and a prd document.
PRD Sub-Directory : $ARGUMENTS
## Goal

To guide an AI assistant in creating a detailed, step-by-step task list in Markdown format based on an existing Product Requirements Document (PRD) and the high level brief document under the tasks/ folder uder the sub-directory @ARGUMENTS . The task list should guide a developer through implementation.

## Output

- **Format:** Markdown (`.md`)
- **Location:** `/tasks/$ARGUMENTS`
- **Filename:** `tasks-[prd-file-name].md` (e.g., `tasks-prd-user-profile-editing.md`)
## Process

1.  **Receive PRD Reference and high level breif :** The user points the AI to a specific sub-directory under tasks/ directory with the name $ARGUMENTS - which contains two files 1. PRD 2.High level Brief
2.  **Analyze PRD:** The AI reads and analyzes the functional requirements, user stories, and other sections of the specified PRD.
3.  **Phase 1: Generate Parent Tasks:** Based on the PRD and high level brief analysis, create the file and Create a task list grouped by **Lean Startup–style MVPs**. For each MVP, define **vertical user stories** starting with a **walking skeleton**, then iteratively add layers to build out the feature in phased slices. Generate **high-level tasks** for each MVP in a strict **Outside-In TDD** sequence from *Growing Object-Oriented Software, Guided by Tests* — start with an end-to-end acceptance test, drive the outermost interface, and descend through collaborators with focused unit tests, faking or extracting implementations as you progress. Each slice must deliver **visible, shippable value** to the user.

**Example:**
For a **music playlist generator app**:

* **MVP 1: Walking Skeleton**

  * Write an acceptance test for “User can input a song query and get a placeholder playlist.”
  * Implement a minimal front-end input field connected to the backend API.
  * Mock backend response with static playlist data.
  * Validate integration with a test simulating the full flow.

* **MVP 2: Basic Playlist Generation**

  * Write an acceptance test for “Playlist is generated dynamically using an external music API.”
  * Add backend logic to fetch real tracks from the API.
  * Implement basic validation and error handling.
  * Update front-end to display real results.

* **MVP 3: Personalization Layer**

  * Write an acceptance test for “User playlists adapt based on saved preferences.”
  * Implement user profile storage.
  * Create recommendation logic based on user history.
  * Refactor tests to validate multiple user profiles and states.

. Present these tasks to the user in the specified format (without sub-tasks yet). Inform the user: "I have generated the high-level tasks based on the PRD. Ready to generate the sub-tasks? Respond with 'Go' to proceed."
1.  **Wait for Confirmation:** Pause and wait for the user to respond with "Go".
2.  **Phase 2: Generate Sub-Tasks:** Once the user confirms, break down each parent task of each MVP into smaller, actionable sub-tasks necessary to complete the parent task. The sub-tasks should be designed to follow Strict Test Driven Development - outside in TDD should be followed when appropriate.  Ensure sub-tasks logically follow from the parent task and cover the implementation details implied by the PRD.
3.  **Identify Relevant Files:** Based on the tasks and PRD, identify potential files that will need to be created or modified. List these under the `Relevant Files` section, including corresponding test files if applicable.
4.  **Compatible With Outside-in TDD** - when generating tasks and sub-task bear in mind that Outside-in TDD will be used to execute the tasks.
5.  **Generate Final Output:** Combine the parent tasks, sub-tasks, relevant files, and notes into the final Markdown structure.
6.  **Save Task List:** Save the generated document in the tasks/@ARGUMENTS  directory with the filename `tasks-[prd-file-name].md`, where `[prd-file-name]` matches the base name of the input PRD file (e.g., if the input was `prd-user-profile-editing.md`, the output is `tasks-prd-user-profile-editing.md`).

## Output Format

The generated task list _must_ follow this structure:

```markdown
## Relevant Files

- `path/to/potential/file1.ts` - Brief description of why this file is relevant (e.g., Contains the main component for this feature).
- `path/to/file1.test.ts` - Unit tests for `file1.ts`.
- `path/to/another/file.tsx` - Brief description (e.g., API route handler for data submission).
- `path/to/another/file.test.tsx` - Unit tests for `another/file.tsx`.
- `lib/utils/helpers.ts` - Brief description (e.g., Utility functions needed for calculations).
- `lib/utils/helpers.test.ts` - Unit tests for `helpers.ts`.

### Notes

- Unit tests should typically be placed alongside the code files they are testing (e.g., `MyComponent.tsx` and `MyComponent.test.tsx` in the same directory).
- Use `npx jest [optional/path/to/test/file]` to run tests. Running without a path executes all tests found by the Jest configuration.

## Tasks

- [ ] 1.0 Parent Task Title
  - [ ] 1.1 [Sub-task description 1.1]
  - [ ] 1.2 [Sub-task description 1.2]
- [ ] 2.0 Parent Task Title
  - [ ] 2.1 [Sub-task description 2.1]
- [ ] 3.0 Parent Task Title (may not require sub-tasks if purely structural or configuration)
```

## Interaction Model

The process explicitly requires a pause after generating parent tasks to get user confirmation ("Go") before proceeding to generate the detailed sub-tasks. This ensures the high-level plan aligns with user expectations before diving into details.

## Target Audience

Assume the primary reader of the task list is a **junior developer** who will implement the feature.
