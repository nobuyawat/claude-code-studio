# Changelog

## [2026-04-15] — v2.0.2 Desktop Chat Delegation (Verified)

### Added
- Version display at `/book-run` startup (v2.0.2 identifier)
- Phase 7 identification text (`📌 Desktop Chat Delegation Mode`)
- Self-diagnostic section for version mismatch detection
- Prompt generation rules based on real-world verification
- Practical "copy-paste ready" prompt examples in workaround-guide.md
- Troubleshooting section for Desktop chat delegation

### Changed
- book-run Phase 7: Upgraded prompt generation with trigger-text requirement
- workaround-guide.md: Added verified prompt format and success criteria

### Verified
- KDP form auto-fill via Desktop chat delegation: SUCCESS (2026-04-15)
- Title, subtitle, furigana, romaji, author fields all populated correctly
- Delegation architecture confirmed as production-ready

---

## [2026-04-14] — Domain Restriction Bug Workaround

### Added
- `docs/workaround-guide.md` — Claude in Chrome domain restriction bug workaround guide
- Known Issues section in README.md
- Domain restriction fallback in bootstrap-workspace SKILL.md
- Desktop chat delegation for book-run Phase 7 (KDP registration)

### Changed
- book-run Phase 7: Switched from direct Claude in Chrome operation to Desktop chat delegation
- bootstrap-workspace Phase 3: Added fallback for blocked domains with auto-generated Desktop chat prompts

### Context
Claude in Chrome v1.0.66+ introduced a domain restriction bug that blocks navigation to non-Google domains from Claude Code (bridge).
Anthropic has acknowledged this as a P1 regression bug (Issue #43255).
v1.0.68 partially mitigates but does not fully resolve the issue.

All local operations (code generation, file processing, EPUB generation) are unaffected.
Browser operations are delegated to Claude Desktop chat as a workaround.
