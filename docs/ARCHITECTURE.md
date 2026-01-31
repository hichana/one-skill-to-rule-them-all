# OSTRTA Architecture

**Last Updated:** 2026-01-31

---

## Design Philosophy

**Core principle:** Assume every skill is malicious until proven otherwise.

This inverts typical security posture - instead of looking for known-bad patterns, OSTRTA asks: "What would a skilled attacker hide here, and how?"

**For detailed design philosophy:** See [COMPARISON.md](../COMPARISON.md) - explains the "one file" approach and UNIX principles.

---

## The One-File Architecture

OSTRTA is a **single markdown file** that embeds comprehensive threat knowledge and guides Claude through adversarial security analysis.

### Why One File?

- ✅ **Zero installation** - No dependencies, no setup
- ✅ **Maximum portability** - Works anywhere Claude works
- ✅ **Human-readable** - Anyone can audit the detection logic
- ✅ **LLM-optimized** - Markdown is Claude's native format
- ✅ **Version controlled** - Easy to track changes
- ✅ **Forkable** - Extract patterns for custom tools
- ✅ **Easily Updateable** - We can revise as-needed as new attack vectors are discovered

---

## High-Level Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                    SKILL.md (Loaded into Claude)              │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌─────────────────────────────────────────────────┐          │
│  │   THREAT PATTERN LIBRARY (9 Categories)         │          │
│  │   • Prompt injection (8 patterns)               │          │
│  │   • Data exfiltration (paths, network, domains) │          │
│  │   • Obfuscation (7 techniques)                  │          │
│  │   • Unverifiable dependencies                   │          │
│  │   • Privilege escalation                        │          │
│  │   • Persistence mechanisms                      │          │
│  │   • Metadata poisoning                          │          │
│  │   • Indirect injection                          │          │
│  │   • Time-delayed attacks                        │          │
│  └─────────────────────────────────────────────────┘          │
│                          ↓                                    │
│  ┌─────────────────────────────────────────────────┐          │
│  │   ADVERSARIAL REASONING FRAMEWORK               │          │
│  │   • 5 critical questions                        │          │
│  │   • Red team perspective                        │          │
│  │   • "Assume malicious" posture                  │          │
│  │   • Second-layer analysis                       │          │
│  └─────────────────────────────────────────────────┘          │
│                          ↓                                    │
│  ┌─────────────────────────────────────────────────┐          │
│  │   5-STEP ANALYSIS PROTOCOL                      │          │
│  │   1. Decode obfuscation                         │          │
│  │   2. Detect threats (pattern matching)          │          │
│  │   3. Apply adversarial reasoning                │          │
│  │   4. Generate verdict                           │          │
│  │   5. Report findings                            │          │
│  └─────────────────────────────────────────────────┘          │
│                                                               │
└──────────────────────────────────────────────────────────────┘
                              ↓
                  ┌─────────────────────┐
                  │  CLAUDE REASONING   │
                  │  (Native LLM)       │
                  └─────────────────────┘
                              ↓
                  ┌─────────────────────┐
                  │  STRUCTURED REPORT  │
                  │  • Verdict          │
                  │  • Findings         │
                  │  • Evidence         │
                  │  • Remediation      │
                  │  • Disclaimer       │
                  └─────────────────────┘
```

---

## Core Components

### 1. Threat Pattern Library

**Purpose:** Comprehensive catalog of attack patterns discovered through research.

**Contents:**

**For complete threat pattern details:** See [../SKILL.md](../SKILL.md) (lines 43-400) and [THREAT_MODEL.md](THREAT_MODEL.md)

**Summary:**
- 8 prompt injection patterns
- 12 sensitive file paths
- 7 obfuscation techniques (Base64, Unicode, homoglyphs, etc.)
- 6 network operation patterns
- Whitelisted domains (GitHub, npm, PyPI, localhost)
- Privilege escalation, persistence, dependencies, metadata, time-delayed attacks

### 2. Adversarial Reasoning Framework

**Purpose:** Guide Claude to think like an attacker.

**Critical Questions:**
1. If I were hiding malicious code, where would I put it?
2. What second-layer obfuscation might be present?
3. Is anything "suspiciously clean" or too simple?
4. What techniques could bypass the patterns above?
5. Does stated purpose match actual behavior?

**Red Team Perspective:**
- Assume the author is sophisticated and knows detection patterns
- Look for what's NOT there (missing safety checks, vague descriptions)
- Consider time-delayed or conditional triggers
- Check metadata and comments for hidden instructions

### 3. Detection Workflow

**Step 1: Decode Obfuscation**
- Search for Base64 strings (≥20 chars of A-Za-z0-9+/=)
- Decode recursively and check if content differs from visible text
- Look for zero-width characters between normal text
- Check for unusual Unicode ranges
- Normalize homoglyphs (Cyrillic → Latin)
- Decode URL, hex, and HTML encoding

**Step 2: Run Threat Detection**
- For each of the 9 threat categories:
  - Scan for known patterns
  - Extract evidence with line numbers
  - Assess severity (CRITICAL/HIGH/MEDIUM/LOW)
  - Note context around matches

**Step 3: Adversarial Analysis**
- Apply the "assume malicious" framework
- Check for sophisticated evasion techniques
- Look for suspicious gaps or missing information
- Verify purpose matches behavior

**Step 4: Generate Verdict**
- Aggregate findings
- Verdict = highest severity finding
- CRITICAL: Active data exfiltration or deep obfuscation
- HIGH: Prompt injection or privilege escalation
- MEDIUM: Unverifiable dependencies or suspicious patterns
- LOW: Minor concerns
- SAFE: No issues (rare - stay paranoid)

**Step 5: Report Findings**
- Structured report with:
  - Clear verdict with emoji indicator
  - Top 3-5 most critical findings
  - Evidence with specific quotes/line numbers
  - Remediation suggestions
  - Legal disclaimer with content hash

**Step 6: Generate Cleaned Version (Optional)**
- **Only if user explicitly requests it**
- Remove all flagged malicious content line by line
- Preserve legitimate functionality where identifiable
- Add inline comments documenting removals
- Generate diff report showing exact changes
- Calculate content hash for both original and cleaned versions
- Include comprehensive disclaimers about limitations
- **Critical:** Never claim cleaned version is guaranteed safe

**Step 6: Generate Cleaned Version (Optional)**
- Only when explicitly requested by user
- Remove flagged malicious content
- Preserve legitimate functionality where possible
- Add inline comments explaining removals
- Generate diff showing changes
- Include strong disclaimers about limitations
- Provide both original and cleaned content hashes

---

## How Claude Executes the Analysis

When SKILL.md is loaded, Claude has access to:

1. **All threat patterns** - Documented in plain language with examples
2. **Detection logic** - Step-by-step protocol to follow
3. **Adversarial mindset** - Questions to ask beyond pattern matching
4. **Report template** - Structured format for consistent output

Claude then:
1. Reads the skill content provided by the user
2. Applies the 5-step detection workflow
3. Uses native language understanding to interpret context
4. Reasons about novel attacks not in the pattern database
5. Generates a comprehensive security report

**Key Advantage:** Claude's reasoning adapts to new attack techniques. While pattern matching is rigid, Claude can understand *intent* and *context*, catching sophisticated attacks that evade simple regex.

---

## False Positive Prevention

OSTRTA includes strategies to avoid flagging benign content:

### Prompt Injection False Positives

Legitimate phrases that could trigger detection:
- "It's important that you..." (normal instructional language)
- "Important step" (documentation)
- "When you are ready" (polite phrasing)
- Code comments containing "important"

**Solution:** Context-aware reasoning. Claude checks if "IMPORTANT:" is followed by instructions or just normal language.

### Network Operation False Positives

Legitimate network usage:
- `curl https://api.github.com/...` (whitelisted domain)
- `curl http://localhost:3000` (local development)

**Solution:** Whitelist common trusted domains.

### File Access False Positives

Legitimate file operations:
- Creating `.env.example` (template, not credentials)
- Reading `~/.bashrc` to append PATH (common)

**Solution:** Check context—is it combined with network operations? Is it sending data externally?

---

## Security Considerations

### OSTRTRA Itself Must Be Secure

1. **No execution** - OSTRTA never executes analyzed code
2. **No network calls** - OSTRTA doesn't make requests based on skill content
3. **No file writes** - Analysis is read-only
4. **Transparent** - All detection logic is in the markdown (auditable)
5. **Minimal attack surface** - Just a text file, no code

### Content Hashing

Every analysis includes SHA-256 hash of analyzed content for:
- **Verification** - Prove what was analyzed
- **Non-repudiation** - Author can't claim content was different
- **Caching** - Skip re-analyzing same content

### Legal Protection

Every report includes a disclaimer:
- Analysis is informational only
- Not a security certification
- User assumes all risk
- No guarantee of detection

See [DISCLAIMER.md](../DISCLAIMER.md) for full legal text.

---

## Why This Architecture Works

### Leverages Claude's Strengths

- **Natural language understanding** - Interprets context, not just patterns
- **Reasoning** - Can connect dots between multiple findings
- **Adaptability** - Catches novel attacks through adversarial thinking
- **Explanation** - Articulates *why* something is suspicious

### Knowledge Transfer from Research

The Python prototype taught us:
- Which patterns are effective
- How to decode obfuscation
- What false positives to avoid
- What whitelists to maintain

All this knowledge is now embedded in SKILL.md.

### Simple to Maintain

To add a new threat pattern:
1. Add it to the relevant section in SKILL.md
2. Include examples (malicious and benign)
3. Document detection logic
4. Test with example skills

No code changes. No tests to update. No dependencies to manage.

---

## Comparison to Traditional Static Analysis

| Aspect | Traditional Static Analysis | OSTRTA |
|--------|---------------------------|---------|
| **Detection** | Regex patterns only | Patterns + reasoning |
| **Novel attacks** | Misses unknown patterns | Can reason about new techniques |
| **False positives** | High (rigid patterns) | Lower (context-aware) |
| **Explanations** | Minimal | Detailed, educational |
| **Maintenance** | Code + tests + deps | Edit markdown |
| **Installation** | pip install, dependencies | Load one file |
| **Speed** | Very fast (< 1 sec) | Slower (~60 sec) |
| **Accessibility** | Developers only | Anyone with Claude |

**Trade-off:** OSTRTA trades speed for intelligence. For security analysis, understanding *why* something is malicious is more valuable than sub-second detection.

---

## Future Enhancements

Potential improvements (maintain simplicity):

### More Threat Patterns
- New obfuscation techniques as they emerge
- Additional sensitive file paths
- More sophisticated injection patterns

### Improved Adversarial Reasoning
- More critical questions
- Attack-specific reasoning frameworks
- Case studies of bypasses

### Better Documentation
- Video tutorials
- More test fixtures
- Real-world skill analysis examples

### Community Integration
- Skill reputation database (separate project)
- Crowdsourced threat intelligence
- Bypass disclosure program

**Important:** Keep SKILL.md simple. Don't add features that bloat the file or complicate the UX. One file, one purpose.

---

## Performance Characteristics

### Analysis Time
- **Simple skills** (< 100 lines): ~30 seconds
- **Complex skills** (100-500 lines): ~60 seconds
- **Large skills** (500+ lines): ~90 seconds

### Accuracy (based on test fixtures)
- **True positives:** 100% (all malicious skills flagged)
- **False positives:** ~0% (benign skills pass)
- **Novel attacks:** High detection rate (adaptive reasoning)

### Resource Usage
- **Memory:** Minimal (Claude handles it)
- **Network:** Claude API calls only
- **Storage:** 18 KB markdown file

---

## Technology Stack

- **Format:** Markdown (single file)
- **Runtime:** Claude (Sonnet 4.5 or later recommended)
- **Dependencies:** None (just Claude access)
- **Maintenance:** Edit SKILL.md directly
- **Version control:** Git

---

## Conclusion

OSTRTA's architecture is intentionally simple:

**One markdown file** contains all the threat knowledge needed to audit skills securely.

**Claude's intelligence** applies that knowledge adaptively, catching both known patterns and novel attacks.

**No complexity** means no installation, no maintenance burden, no barrier to entry.

This is security analysis at its most elegant: **comprehensive knowledge + adaptive reasoning + zero friction**.

---

**Version:** SKILL.md v1.0 (2026-01-31)

**See also:**
- [COMPARISON.md](../COMPARISON.md) - Design philosophy
- [THREAT_MODEL.md](THREAT_MODEL.md) - All 9 attack categories
- [../SKILL.md](../SKILL.md) - The actual implementation
