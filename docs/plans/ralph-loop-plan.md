# Ralph Loop: Autonomous Learning Plan for AI-Mol

## Vision

Build a robust, self-improving PyMOL integration where Claude becomes a capable partner in protein science. The end state is a repo someone can clone and immediately have a "superpowered claude-mol" for their scientific work.

## What We Want

- Good skills that capture learned patterns
- Good docs that help users bootstrap
- CLI tools that work reliably
- Well-organized repo
- Tests that verify capabilities
- An agent that can launch/close PyMOL without problems, recover from crashes, and see visual outputs effectively
- **Publication-quality figures** - not just "did it load" but "would this go in a paper"

## Quality Standard (Added 2026-01-28)

**Each curriculum task should produce PUBLICATION QUALITY output:**

1. Ray-traced rendering (not OpenGL snapshots)
2. Thoughtful camera angles showing biological relevance
3. Multiple views when appropriate (overview + detail)
4. Consistent, meaningful color schemes
5. Proper resolution (300 DPI, sufficient dimensions)

**The goal is not to check boxes, but to build genuine expertise in molecular visualization.**

---

## Phase 1: Foundation (Do This First)

**Goal:** Make PyMOL interaction bulletproof before expanding capabilities.

### 1.1 Session Lifecycle

Build/verify these capabilities:

- [ ] Launch PyMOL programmatically (handles uv env, system install, etc.)
- [ ] Detect when PyMOL dies or becomes unresponsive
- [ ] Recover automatically from crashes
- [ ] Clean shutdown without zombie processes
- [ ] Connect to already-running PyMOL instance

**Key files:** `pymol_session.py`, `pymol_connection.py`

**Test it:**
```bash
python -c "from pymol_session import PyMOLSession; s = PyMOLSession(); s.start(); print('healthy:', s.is_healthy()); s.stop()"
```

### 1.2 Visual Feedback Loop

Build/verify:

- [ ] Take snapshots of current PyMOL view
- [ ] Save to scratch directory with meaningful names
- [ ] View images to verify commands worked
- [ ] Auto-cleanup old scratch images

**Key files:** `pymol_view.py`, `scratch/` directory

**Test it:**
```python
from pymol_view import reset_and_view
path = reset_and_view("1ubq")
# Then use Read tool to view the image
```

### 1.3 Foundation Exit Criteria

Before moving to Phase 2, all of these must work:

- [ ] `PyMOLSession().start()` works reliably
- [ ] `session.is_healthy()` correctly detects live/dead PyMOL
- [ ] `session.recover()` brings back a crashed session
- [ ] `pymol_view()` produces viewable images
- [ ] Can execute a simple workflow: load structure → modify → snapshot → view

---

## Phase 2: Curriculum-Driven Learning

**Goal:** Systematically build capabilities by working through test cases.

### The Learning Loop

```
1. Check foundation is solid (can connect, take snapshots)
2. Pick next task from curriculum (start at Tier 1)
3. Attempt the task
4. Evaluate outcome:
   - SUCCESS: Note patterns, consider skill extraction
   - FAILURE: Diagnose → fix tooling OR note gap
5. Update progress tracker
6. If 3+ consecutive failures: return to Phase 1
7. Repeat
```

### Curriculum Sources

Two reference curricula in `docs/curricula/`:
- `compass_artifact_*.md` - 85 tasks across 4 tiers
- `gemini_curriculum.md` - 100 tasks across 5 tiers

### Tier Progression

| Tier | Tasks | Target Pass Rate | Focus |
|------|-------|------------------|-------|
| Basic | 1-20 | >90% | Core commands, representations, coloring |
| Intermediate | 21-45 | >80% | Comparisons, surfaces, measurements |
| Advanced | 46-70 | >60% | Domain-specific (cryo-EM, drug discovery) |
| Expert | 71-85+ | >40% | Hypothesis generation, multi-step workflows |

### Skill Extraction Triggers

Create a new skill when:
- Same problem solved 2-3 times
- Workflow has 3+ steps that always go together
- Discovered non-obvious PyMOL behavior
- Found a pattern that would help future sessions

### Progress Tracking

Update `docs/progress/curriculum-tracker.md` each session:
- Mark tasks attempted/passed/failed
- Note blockers and gaps
- List skills created

---

## Phase 3: Polish & Bootstrap

**Goal:** Make the repo clone-and-go.

### Target Experience

1. User clones repo
2. Runs `/pymol-setup`
3. Says "load 1ubq and show me"
4. It works

### Deliverables

- [ ] `QUICKSTART.md` - 5-minute bootstrap guide
- [ ] Skills cover common workflows
- [ ] Tests verify reliability
- [ ] Clean repo structure

---

## Self-Regulation Rules

### When to Step Back

- 3+ consecutive task failures → check foundation
- Same error repeating → stop and fix tooling
- Can't see visual output → fix feedback loop first

### When to Push Forward

- Foundation is solid
- Pass rate meeting tier targets
- Patterns emerging that could become skills

### What to Skip (For Now)

- Tasks requiring commercial PyMOL features (morph)
- Tasks requiring external plugins not installed (APBS)
- Tasks requiring data we don't have (trajectories, local files)

Note these as gaps, revisit later.

---

## Session Structure

Each autonomous session should:

### 1. Start
```markdown
## Session: YYYY-MM-DD-N

### Starting State
- Phase: [1/2/3]
- Last session ended at: [task/milestone]
- Known blockers: [any]
```

### 2. Work
- If Phase 1: Work on foundation items
- If Phase 2: Pick next curriculum task, attempt it
- If Phase 3: Work on polish items

### 3. End
```markdown
### Completed
- [what got done]

### Issues
- [problems encountered]

### Artifacts
- [files created/modified]

### Next Session
- [what to pick up]
```

Update `docs/progress/curriculum-tracker.md` with any task attempts.

---

## Key Principles

1. **Reliability over features** - A working basic system beats a broken advanced one
2. **Visual verification** - Always check that commands did what you expected
3. **Capture learnings** - Skills and docs emerge from work, not as separate projects
4. **Self-regulate** - Step back when stuck, push forward when solid
5. **Bootstrap mindset** - Everything should help someone else get started

---

## Getting Started

First session should:

1. Verify PyMOL can be launched: `python pymol_session.py start`
2. Verify connection works: `python pymol_session.py ping`
3. Verify visual feedback works: take a snapshot, view it
4. If all work → start Tier 1 Task 1
5. If any fail → fix that first

Good luck, future me.
