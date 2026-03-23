# PR #21 Review: Improve complex mechanical/illustrative diagram quality

## Overall Assessment: Approve with suggestions

This is a solid, well-structured improvement to mechanical and illustrative diagram generation. Low-risk since it's prompt/skill-only (no runtime code changes). The 4 complementary skill files form a coherent system that addresses clearly diagnosed problems.

---

## Strengths

1. **Well-diagnosed problem**: The issue proposal correctly identifies root causes for poor diagram quality — no planning step before generation, only 9 lines of illustrative guidance, no shape construction methodology, and disconnected interactive controls.

2. **Comprehensive, complementary solutions**: The new files form a logical pipeline:
   - `svg-diagram-skill.txt` expansion → planning protocol + composition rules
   - `master-agent-playbook.txt` → progressive rendering architecture for interactivity
   - `mechanical-illustration-skill.txt` → deep technical construction guide with worked examples
   - `svg-shape-library.txt` → reusable SVG shape fragments as starting points

3. **Practical, not theoretical**: Includes working SVG code, two complete worked compositions (car drivetrain, airfoil), concrete measurements (viewBox dimensions, font sizes, spacing rules), and a centralized state-and-render pattern.

4. **Good architecture**: The centralized `state → updateState → render()` pattern in `master-agent-playbook.txt` solves a real problem with interactive diagrams where controls and visuals get out of sync.

5. **Consistent**: Agent and MCP skill directories are kept in sync (verified — only diff is the expected `__init__.py`).

---

## Suggestions

### 1. System prompt token budget (Medium priority)
This PR adds ~847 lines of new skill content loaded via `load_all_skills()` glob. As skills accumulate, this could push against context limits — especially with longer conversations. The issue proposal itself notes "Consider including only 6-8 most common shapes initially," but the full `svg-shape-library.txt` (317 lines) is included. Consider trimming less common shapes or implementing lazy/conditional loading.

### 2. Skill duplication between `agent/` and `mcp/` (Medium priority)
All 4 skill files are exactly mirrored between `apps/agent/skills/` and `apps/mcp/skills/`. Consider a shared `skills/` directory at the repo root (or symlinks) to prevent silent drift if one copy is updated without the other. Currently there's no mechanism to enforce they stay in sync.

### 3. Issue proposal as committed file (Low priority)
`.github/ISSUE_PROPOSAL_mechanical_diagrams.md` (152 lines) is a planning/strategy document with no runtime purpose. This would be more conventional and discoverable as a GitHub Issue, keeping the repo focused on operational files.

### 4. Domain coverage (Low priority)
The shape library and worked examples focus almost exclusively on vehicles and mechanical systems. A brief section on non-mechanical illustrative diagrams (biology, architecture, abstract concepts) — even just noting that the same composition principles apply — would broaden the guidance.

### 5. Validation (Low priority)
No before/after comparisons showing improved output quality. Consider adding test prompts and sample outputs (even just screenshots in the PR description) to demonstrate the improvement.

---

## Minor Notes

- The 6-step diagram planning protocol in `svg-diagram-skill.txt` is an excellent addition — this alone should prevent many common failures (unrecognizable shapes, spatially inaccurate placement, disconnected controls).
- The progressive rendering architecture with `viz-{component}-{property}` ID convention is well-designed and provides a clear, repeatable pattern.
- The worked references (car drivetrain composition, airfoil/airplane composition) in `mechanical-illustration-skill.txt` are thorough and serve as excellent templates.

---

## Files Reviewed

| File | Status | Lines Changed |
|------|--------|---------------|
| `.github/ISSUE_PROPOSAL_mechanical_diagrams.md` | New | +152 |
| `apps/agent/skills/master-agent-playbook.txt` | New | +100 |
| `apps/agent/skills/mechanical-illustration-skill.txt` | New | +350 |
| `apps/agent/skills/svg-diagram-skill.txt` | Modified | +80/-3 |
| `apps/agent/skills/svg-shape-library.txt` | New | +317 |
| `apps/mcp/skills/master-agent-playbook.txt` | New (mirror) | +100 |
| `apps/mcp/skills/mechanical-illustration-skill.txt` | New (mirror) | +350 |
| `apps/mcp/skills/svg-diagram-skill.txt` | Modified (mirror) | +80/-3 |
| `apps/mcp/skills/svg-shape-library.txt` | New (mirror) | +317 |
