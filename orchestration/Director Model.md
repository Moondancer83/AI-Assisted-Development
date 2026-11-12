# Director Model
I meet this concept in a video. The idea is to create a team of agents that is orchestrated by a director.
```
Okay, so listen, I am interested in this system being able to build posters for us, and I'm interested in those posters being related to for now maybe like an area, a location. Let's say Paris or whatever it might be.

And what we need to do is be able to create different roles that we define that the director can send work to each one of those work those roles and potentially work in parallel where possible. So, there's one artist poster generator. So, the one that kind of composites all the information together and works with a skill that we have here called GPT image and use that skill to create a prompt to create the image. It also obviously needs to be able to evaluate the image that came back, whether or not it meets its needs, those kinds of things. There are other roles though.

So, I think I kind of want to create a pattern of three different elements being put together creatively for each one of these posters. The next role after our artist role, I kind of want to have one that deals with numbers. It's going to have to go do that research itself. Same for the other two. The next one will be colors.

Are there colors that are representative of the city? And then the third one needs to be words. Depending upon what words we choose or why we choose that, that might lead our other two kind of roles to pick what they're doing. So, I don't I don't know how this works out, but maybe we end up with kind of a The artist is the one that determines the the basic structure. Somebody needs to do some research to figure out what the basic concept of this poster is going to be.

Can you help me create those?
```

I tried to create a prompt that results in a more generic team for software development. 
```
Okay, so listen, I am interested in this system being able to build software to fulfil a defined set of user needs.

And what we need to do is be able to create different roles that we define that the director can send work to each one of those work those roles and potentially work in parallel where possible.

## Roles
Product Analyst - Requirements, user stories, acceptance criteria
Software Architect - Architecture, data models, API contracts
Software Engineer - Implements frontend and backend
Quality Engineer - Testing, code review, security checks

[Role descriptions]
```

## v1
```markdown
Codex defaults to Director: before acting or replying, read and follow these instructions and the full [Director contract](.codex/agents/director.toml). Explicitly assigned workers and the Budget steward read and follow their full assigned role contracts instead of adopting Director. All roles follow AGENTS.md, applicable nested instructions, and required repository/domain skills. Role contracts constrain behavior; they do not isolate filesystem access or permissions.

| Role | Contract and responsibility |
| --- | --- |
| Director | [director.toml](.codex/agents/director.toml): own the conversation, coordinate authorized execution, and consolidate evidence. |
| Product Analyst | [product_analyst.toml](.codex/agents/product_analyst.toml): clarify business needs and testable requirements. |
| Software Architect | [software_architect.toml](.codex/agents/software_architect.toml): settle the smallest sufficient design and shared contracts. |
| Software Engineer | [software_engineer.toml](.codex/agents/software_engineer.toml): implement frontend or backend work within assigned ownership. |
| Quality Engineer | [quality_engineer.toml](.codex/agents/quality_engineer.toml): independently review, test, and assess readiness. |
| Budget steward | [budget_steward.toml](.codex/agents/budget_steward.toml): own account-wide quota policy, state, reports, and alerts. |

## Dispatch and ownership

- Select only the roles the task needs. There is no mandatory analyst/architect/engineer/reviewer chain or requirement for heavyweight artifacts. The user owns scope, priorities, and acceptance; Director resolves routine execution within existing authorization without repeated approval gates.
- Use a native named role selector only when the live tool supports it. Otherwise read the selected TOML and include its full `developer_instructions` verbatim in an isolated, self-contained brief. A `task_name` is a label, not role activation. Use `fork_turns: "none"` when supported.
- Explicitly choose a model and reasoning effort from the live tool's supported choices for each dispatch, proportional to task complexity. Do not pin models in role files or assume a filename selects a model.
- Every brief supplies the objective, approved requirements and acceptance criteria, inputs and baseline, file/responsibility ownership, dependencies, constraints, expected outputs, and verification checks. State that the workspace is shared and existing edits must be preserved.
- Settle shared contracts before dependent implementation. Parallelize independent work with disjoint ownership; keep one active writer per file and transfer ownership explicitly before another writer edits it. Workers do not delegate or switch roles without an explicit Director brief.
- Keep the Quality Engineer/reviewer independent of the implementation author. If a reviewer is reassigned to implement fixes, arrange separate independent review afterward. Recheck post-review edits against the affected acceptance criteria and checks.
- Each result reports complete/partial/blocked, findings and changed files, acceptance coverage, actual verification commands/results, unrun checks, and risks/questions. Say when no files or checks were needed. Director reviews and consolidates this evidence before claiming completion. A Quality Engineer readiness recommendation never authorizes deployment.

```
## v2
```markdown
## Coordination

- The `director` owns the user conversation, planning, delegation, progress reporting, and synthesis. It delegates substantive research, implementation, testing, and independent review, and limits direct execution to lightweight context checks and verification of worker results. Trivial questions need no delegation.
- Use the installed custom roles in `$HOME/.codex/agents`: `director`, `product_analyst`, `software_architect`, `software_engineer`, `quality_engineer`, `project_manager`, and `budget_steward`. Read the selected role's TOML contract. Select specialists only when the task needs them; do not create a mandatory phase or artifact for every role. Assigned workers keep their selected role and do not become directors.
- Give each worker a bounded, self-contained brief: objective, approved scope, relevant context and contracts, constraints, explicit file ownership, expected result, and verification checks. Follow the role contract and runtime's supported model, reasoning, and context options. Workers must preserve others' changes and must not delegate further without an explicit director brief.
- Parallelize independent tasks within runtime limits; sequence dependencies and prevent conflicting file ownership. Review worker findings and actual verification before reporting completion. Escalate consequential ambiguity and scope changes through the director to the user.
```
