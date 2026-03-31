---
name: spec-to-scripts
description: >
  A 3-step pipeline for building complete apps with near-zero errors: (1) deep interview to understand the app, (2) generate an exhaustive technical specification document, (3) produce verbatim copy-paste Claude Code session scripts broken into modules. Use this skill whenever the user wants to build an app, create a software project, plan a development project, vibe code something, or turn an idea into working software. Also trigger when someone mentions "app blueprint", "build plan", "development spec", "Claude Code scripts", "module scripts", or says things like "I want to build an app", "help me plan my app", "I have an app idea", "generate build instructions", or "create a technical spec". Also trigger when the user wants to fix, debug, update, or extend an existing app â "fix my app", "add a feature", "rebuild this part", "something's not working". This skill removes the ceiling of AI vibe coding by giving Claude Code perfectly structured, module-by-module instructions.
---

# Spec-to-Scripts: The App Building Pipeline

You are guiding the user through a proven 3-step process that consistently produces working apps with near-zero errors. This process works because it separates *thinking* from *building* â the spec does the thinking, the scripts do the building, and Claude Code just follows clear instructions.

The three steps are:
1. **Interview** â Understand the app deeply before writing anything
2. **Technical Specification** â A comprehensive document covering architecture, data models, UI, edge cases, and acceptance criteria
3. **Verbatim Claude Code Scripts** â Module-by-module, copy-paste-ready instructions that a developer (or the user in Warp/terminal) feeds directly to Claude Code

Why this works better than just prompting Claude Code directly: Claude Code is brilliant at *executing* well-scoped tasks but struggles when it has to *invent* the architecture, make design decisions, AND write code all at once. By front-loading all the decisions into the spec, each Claude Code session gets a focused, bounded task with clear success criteria. That's the difference between "build me a patient management system" (which produces chaos) and "build this exact data model with these exact fields, this exact UI with these exact colors, and stop when you hit these acceptance criteria" (which produces working software).

---

## Step 1: The Interview

Before writing a single line of spec, understand what the user is building. This interview is the foundation â skip it or rush it and the whole pipeline suffers.

Ask these questions conversationally (not as a rigid form). Adapt based on what the user already told you. If they uploaded documents, reference files, or described the app earlier in the conversation, pull from that context first and only ask what's missing.

### Core Questions

**The Vision**
- What does this app do in one sentence?
- Who uses it? (Be specific: "chiropractors on iPads" is better than "healthcare professionals")
- What's the single most important thing it does? (This becomes Module 1 after setup)
- What existing tools/workflows does it replace?

**Technical Foundation**
- What tech stack? (If they don't know, recommend based on their use case. For web apps: React/Next.js + Supabase is a solid default. For mobile: Flutter + Firebase. For simple tools: Python + FastAPI.)
- Any existing accounts or infrastructure? (Firebase project, AWS account, domain name, API keys)
- Where will it run? (Phone, tablet, desktop browser, all of the above)
- Any third-party APIs or services needed? (Payment processing, AI/LLM, email, SMS)

**Design & UX**
- What should it *feel* like? (Get specific: "Apple-quality, premium, clean white" is actionable. "Modern and nice" is not.)
- Any reference apps or websites they admire?
- Primary interaction mode? (Mouse/keyboard, touch/tablet, voice-first, hybrid)
- Brand colors, fonts, or design tokens? (If none, propose a tasteful default palette)

**Data & Logic**
- What are the main "things" in the system? (Patients, orders, tasks, messages â these become data models)
- What are the key workflows? Walk through a typical user session start to finish
- What are the business rules? (Pricing logic, permission levels, state-specific regulations, etc.)
- What happens when things go wrong? (Offline behavior, validation errors, edge cases)

**Scope**
- What's V1? (The minimum that's actually useful â not a toy demo, but not everything either)
- What's the roadmap beyond V1? (Capture this at a high level for the spec's "Future Versions" section)

### Interview Tips

- If the user is vague on design, push them. "Clean and modern" means nothing to Claude Code. Exact hex colors, font names, pixel sizes, and border radii mean everything.
- If they describe a complex app, help them identify natural module boundaries. Each module should be buildable in one Claude Code session and testable on its own.
- Listen for implicit requirements. If they say "it runs on iPads in a clinic," that implies touch-friendly UI, offline resilience, and possibly voice input.
- Don't let scope creep into V1. If they mention 20 features, help them pick the 6 that make V1 useful and defer the rest to V1.1/V2.

---

## Step 2: The Technical Specification

Generate a comprehensive technical specification document. This is NOT a casual summary â it's the complete blueprint that the Claude Code scripts will be derived from. Every decision that would otherwise be made ad-hoc during coding gets made here instead.

Save this as a `.md` file (or `.docx` using the docx skill if the user prefers a formatted document).

### Spec Structure

Follow this structure. Every section matters.

```
# [App Name]
## [Tagline â one line describing what it does]

### Technical Specification for Claude Code Development

Version X.0 Â· [Date]
[Organization/Person name]

---

## Executive Vision
2-3 paragraphs. What this app does, why it exists, what makes it different.
Written in a way that gives Claude Code the "why" behind every decision.## Design Philosophy
- What design standard is this held to? (e.g., "Apple-quality", "Material Design 3", "Stripe-level polish")
- Visual design principles: exact colors (hex codes), fonts (with weights), spacing scale, border radii, shadow values
- UX principles: the non-negotiable interaction rules (e.g., "never make the user repeat themselves", "every tap moves you forward")

## Interface Layout
- Describe every screen/zone with exact dimensions, colors, and behavior
- For single-screen apps: describe each zone (top bar, main area, input area)
- For multi-screen apps: describe navigation structure and each screen's layout

## System Architecture Overview
- Tech stack with specific versions where it matters
- How components connect (frontend â API â database â external services)
- Infrastructure: hosting, auth, database, storage, CDN
- Security model: auth flow, data access rules, API key management

## Data Flow
- Walk through 2-4 primary user workflows step by step
- "When the user does X, here's exactly what happens: step 1, step 2..."
- Include the unhappy paths: what happens when disambiguation is needed, when data is missing, when the network drops

## Build Phases Overview
- List all versions (V1.0, V1.1, V2.0...)
- Clearly state: "This spec covers V1.0 in full detail. Later versions are outlined."
- V1 should be broken into modules. Typically 4-8 modules for a substantial app.

## V1.0 Modules (one section per module)

For EACH module, include ALL of the following:

### Module N: [Name]

**Goal**: One sentence â what this module adds to the app.

**Claude Code Session Brief**: A paragraph describing what Claude Code needs to know about the existing project state and what it's building in this session.

**Data Models**: Every model this module introduces or modifies.
- Field name (type, constraints, default values)
- Include relationships between models
- Include toJson/fromJson or equivalent serialization methods

**Services/API**: Every service, API endpoint, or Cloud Function this module needs.
- Function signatures with parameter types and return types
- Business logic described in detail
- For AI-powered features: include the COMPLETE system prompt, not a summary

**UI Components**: Every visual element this module adds.
- Exact layout (dimensions, spacing, alignment)
- Exact colors and typography for every element
- Exact behavior (what happens on tap, swipe, hover)
- Animation details (duration, easing, what animates)

**Edge Cases**: A "What If?" section covering every way this module could go wrong.
- What if the data matches multiple records?
- What if required data is missing?
- What if the network drops?
- What if the user does something unexpected?

**Acceptance Criteria**: A checklist of 4-8 specific, testable statements.
- "Saying 'X' produces Y" not "search works"
- "Card displays with 16px border radius and subtle shadow" not "cards look nice"
- Include at least one cross-module integration test if applicable

## Future Versions (Roadmap)
- V1.1, V2.0, etc. at a high level
- Feature bullets, not full specs
- Enough detail that the user can later trigger this skill again to spec out V1.1

## Working with Claude Code: Best Practices
- Git workflow (commit after each module, how to rollback)
- Troubleshooting tips specific to the tech stack
- Session management tips (new session per module, always start with "read the project first")

## Cost Estimate (optional)
- API costs, hosting costs, third-party service costs if applicable
```

### Spec Writing Principles

**Be exhaustive on data models.** Every field, every type, every constraint. If Claude Code has to invent a field name or guess a type, that's a spec failure. Include optional fields explicitly marked as optional with their default values.

**Be pixel-precise on UI.** Hex colors, not color names. "Inter 600, 16px, #1D1D1F" not "bold dark text." Include exact border radii, shadow values, spacing in pixels. Claude Code can execute "16px border radius, box-shadow: 0 2px 10px rgba(0,0,0,0.05)" perfectly. It cannot execute "clean card with subtle shadow" consistently.

**Write complete AI system prompts.** If the app uses an LLM, write the FULL system prompt in the spec, not a description of what it should do. Include the expected response format (usually JSON with specific fields). Include examples of how it should handle ambiguous input. This is often the most critical part of the spec â a vague AI prompt produces vague AI behavior.

**Define module boundaries carefully.** Each module should:
- Be buildable in a single Claude Code session (roughly 30-90 minutes of Claude Code work)
- Have a clear "done" state that's testable
- Build on previous modules but not require future ones
- Have its own git commit message ready

**Include the negative space.** Tell Claude Code what NOT to do. "Don't build any screens yet â just get the project created." "No separate patient list page â everything through chat." "Do NOT use default Flutter blue." The things Claude Code should avoid are just as important as what it should build.

---

## Step 3: Verbatim Claude Code Scripts

This is where the magic happens. Transform the technical spec into a series of copy-paste scripts that the user feeds directly into Claude Code, one module at a time.

Save this as a `.md` file.

### Script Structure

```markdown
# [App Name] â Verbatim Claude Code Session Scripts

Copy and paste each section into a fresh Claude Code session.
Build them in order. Do NOT skip ahead.
After each module, test it, then commit with git before starting the next one.

---

## Before You Start: One-Time Setup

Paste this into your very first Claude Code session:

\```
[Complete setup instructions: create project, install dependencies,
configure services, initialize git. End with a clear stop point.]

Stop here. Don't build any screens yet. Just get the project created,
dependencies installed, [service] connected, and git initialized.
\```

**After this succeeds, say:**

\```
Commit this with the message "Project setup and [service] configuration complete"
\```

---

## Module 1: [Name]

**Start a new Claude Code session. Paste this:**

\```
I'm building [App Name], a [description] at ~/[project-dir].
[Service/framework] is already configured.
Read the project structure first before writing any code.

[Complete module instructions...]
\```

**After this succeeds and you've verified it works, say:**

\```
Commit this with the message "Module 1 complete: [description]"
\```

---

[Repeat for each module...]

---

## After All Modules: Smoke Test

**New Claude Code session. Paste this:**

\```
I'm building [App Name] at ~/[project-dir]. All [N] modules are complete.
Read the entire project.
I need you to help me do a smoke test of the full workflow:

1. Check that the app builds and deploys without errors
2. [Specific build/deploy commands]
3. Walk me through testing this exact scenario:
   a. [Step-by-step test scenario covering the full user journey]

Tell me if anything is broken and fix it.
\```
```

### Script Writing Rules

These rules are what make the scripts work reliably. Each one exists because the alternative produces errors.

**Always start with context.** Every module script begins with: "I'm building [App], a [what it is] at [path]. Read the project structure first before writing any code." Claude Code needs to understand what exists before it adds to it. Without this, it will overwrite files, duplicate logic, or create incompatible code.

**One module = one Claude Code session.** Never combine modules. Claude Code's context window is precious â a fresh session for each module means maximum focus. Tell the user to start a new session explicitly.

**Be verbatim, not descriptive.** The script IS the prompt. Write exactly what the user will paste. Don't write "tell Claude Code to create a patient model with the following fields" â write the actual prompt with the actual fields listed out. The user should never have to translate or interpret.

**Include every detail from the spec.** The script should contain ALL the information Claude Code needs for that module â data models with field types, UI specs with exact colors, API function signatures, business logic, edge case handling. If something is in the spec, it goes in the script. Claude Code won't have access to the spec document, so the script must be self-contained.

**Tell Claude Code what already exists.** After the setup module, every script should mention what's already been built: "it has a working chat interface with voice input and Firestore message persistence." This prevents Claude Code from rebuilding things that already exist.

**Tell Claude Code when to stop.** End setup and foundational modules with explicit stop points: "Stop here. Don't build any screens yet." Without this, Claude Code will eagerly continue building things you haven't specified yet.

**Include the git commit.** After every module, provide the exact commit message. This creates clean rollback points and trains the user to commit before moving on.

**Add smoke tests after groups of modules.** After every 4-6 modules (or at version boundaries), include a dedicated smoke test script. This test script walks through a realistic end-to-end scenario and asks Claude Code to fix anything that's broken. This catches integration issues early.

**Write the deploy/test commands.** Don't assume the user knows how to build or deploy. Include the exact commands: `flutter build web`, `npm run build`, `firebase deploy`, etc.

**Make design requirements non-negotiable.** Put design specs under a header like "DESIGN REQUIREMENTS (non-negotiable):" and list them with exact values. Claude Code respects this framing and won't substitute defaults.

**For AI features, include complete system prompts.** When a module involves an LLM (like a Cloud Function calling Claude API), include the ENTIRE system prompt in the script as a quoted block. Include the expected JSON response format. Include examples of how it should handle edge cases. Vague AI prompts are the #1 source of bugs in AI-powered features.

**Structure complex modules with numbered sections.** Use clear numbered sections within each script: "1. DATA MODEL... 2. SERVICE LAYER... 3. UI COMPONENT... 4. CONNECT TO EXISTING..." This gives Claude Code a natural execution order.

---

## Delivering the Output

After completing all three steps, present the user with:

1. **The Technical Specification** â saved as a file (`.md` or `.docx`)
2. **The Verbatim Scripts** â saved as a `.md` file
3. **A quick-start summary** â a brief message explaining:
   - How many modules there are
   - Estimated time per module (rough guess based on complexity)
   - The order to build them
   - Reminder: new Claude Code session per module, commit after each, test before moving on

If you used the docx skill for the spec, mention that both files are ready. The scripts file is always markdown since it's designed for copy-paste into a terminal.

---

## Fixing, Debugging, or Extending an Existing App

The same 3-step pipeline works for fixes and extensions â you just adapt each step slightly.

### Step 1 (Interview) becomes: Understand the Problem

Instead of asking "what are you building?", ask:
- What's broken or what do you want to change?
- What does the app do currently? (tech stack, architecture, what's already built)
- Where does the code live? (repo path, hosting, services)
- What have you already tried?
- Is this a bug fix, a new feature, or a rebuild of an existing feature?

If the user has an existing tech spec or scripts from a previous build, ask them to share those â they're gold for understanding the current architecture.

### Step 2 (Spec) becomes: A Targeted Fix Spec

Don't rewrite the entire app spec. Write a focused spec that covers ONLY what's changing:
- **What exists now**: Brief summary of current architecture relevant to the fix
- **What's wrong / what's missing**: Clear description of the problem or desired change
- **What the fix looks like**: Data model changes, UI changes, API changes â same level of detail as a new build spec, but scoped to the affected modules only
- **Acceptance criteria**: How do we know the fix worked?
- **What NOT to touch**: Explicitly list parts of the app that should remain unchanged. This prevents Claude Code from "helpfully" refactoring things that aren't broken.

### Step 3 (Scripts) becomes: Targeted Fix Scripts

Generate scripts the same way, but scoped:
- Might be just 1-2 modules instead of 6-8
- Each script still starts with "Read the project first" and describes what exists
- Each script still ends with a commit message
- Still include a smoke test that verifies the fix AND confirms nothing else broke

The key insight: even a "quick fix" benefits from the spec step. Writing down what you're changing and what you're NOT changing before touching code prevents the cascade of "I fixed one thing and broke three others."

---

## Important Reminders

- The interview is not optional. Even if the user says "just build it," push back gently and ask the key questions. A 5-minute interview saves hours of rebuilding.
- The spec is the source of truth. Every detail in the scripts should trace back to the spec. If you're writing a script and realize the spec is missing something, go back and add it to the spec first.
- Module boundaries matter. If a module feels too big (more than ~150 lines of script), split it. If it feels too small (fewer than ~30 lines), merge it with an adjacent module.
- The user might not be technical. Write scripts that are complete and self-contained. Don't assume they know what a "Cloud Function" is â if the script creates one, it should create the complete thing, not reference external setup steps.
- This process works for ANY tech stack. The examples in your head might be Flutter/Firebase, but the same structure applies to React/Supabase, Python/FastAPI, Swift/CloudKit, or anything else. Adapt the tech-specific details but keep the structure identical.
