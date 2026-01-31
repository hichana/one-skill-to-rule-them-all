# Why SKILL.md Only?

**OSTRTA is a single markdown file. No Python. No installation. Just Claude.**

This document explains the design philosophy behind keeping OSTRTA as a pure SKILL.md implementation.

---

## The Original Vision

OSTRTA started with a simple idea:

> **"One file to rule them all"** - A single SKILL.md that could audit other SKILL.md files for security issues.

During early prototyping, we built a Python CLI tool to validate the threat detection patterns and understand the problem space. That research was invaluable—it helped us discover:

- 8 prompt injection patterns
- 12 sensitive file paths
- 7 obfuscation techniques
- Whitelisted domains
- False positive prevention strategies

But the Python tool was always meant to be **research scaffolding**, not the final product.

---

## Why Not Python?

We could have shipped both a SKILL.md and a Python tool, but that would violate the core principle: **elegant simplicity**.

### The Python Problem

A Python CLI tool introduces:
- ❌ Installation friction (`pip install`, dependencies)
- ❌ Maintenance burden (code, tests, dependencies, breaking changes)
- ❌ Accessibility barrier (developers only)
- ❌ Rigidity (only detects known patterns)
- ❌ Complexity (users must choose between two tools)

### The SKILL.md Advantage

A single markdown file provides:
- ✅ **Zero installation** - Just load the file
- ✅ **Maximum accessibility** - Anyone with Claude can use it
- ✅ **Adaptive intelligence** - Claude can reason about novel attacks
- ✅ **Conversational UX** - Ask follow-up questions, get explanations
- ✅ **Trivial maintenance** - Edit markdown, no code changes
- ✅ **Educational** - Users learn about threats while analyzing
- ✅ **Elegant simplicity** - One file, one purpose

---

## What About Automation?

**"But what about CI/CD integration? Batch analysis? Deterministic results?"**

These are valid use cases, but they serve a different audience (security engineers automating at scale) than OSTRTA's primary mission (helping everyday users audit skills before installation).

### For Users: SKILL.md is Perfect

99% of users are:
- Installing 1-10 skills manually
- Want to understand *why* a skill is flagged
- Benefit from conversational explanations
- Don't need sub-second analysis

For these users, SKILL.md is the ideal solution.

### For Automation: Build Your Own

The SKILL.md file **is** the comprehensive specification. If you need automation:

1. Use the patterns documented in SKILL.md
2. Build a simple script that:
   - Runs regex patterns against skill content
   - Decodes Base64/Unicode obfuscation
   - Generates JSON output

**Example (100 lines of Python):**
```python
import re, base64

# All patterns are documented in SKILL.md
PATTERNS = {
    'prompt_injection': r'IMPORTANT\s*:\s*(?:You|Ignore|Disregard)',
    'data_exfil': r'curl.*\$\(cat.*\.(aws|ssh)',
    # ... copy patterns from SKILL.md
}

def analyze(content):
    findings = []
    for category, pattern in PATTERNS.items():
        if re.search(pattern, content):
            findings.append(category)
    return findings
```

This is **intentionally** simple. OSTRTA provides the knowledge (threat patterns), you provide the automation wrapper.

---

## Design Philosophy: UNIX Principles for AI Skills

OSTRTA follows classic UNIX design philosophy:

1. **Do one thing well** - Audit SKILL.md files for security issues
2. **Make it simple** - One markdown file, no dependencies
3. **Make it readable** - Markdown is human-readable and LLM-friendly
4. **Avoid feature creep** - No CI/CD, no API, no dashboard—just analysis

This is the same philosophy that made `grep`, `sed`, and `awk` timeless. They do one thing, simply and well.

---

## What We Learned from the Python Prototype

Building the Python tool taught us:

### ✅ What Works

- Recursive Base64 decoding (up to 10 layers)
- Unicode normalization (NFKC)
- Zero-width character detection (U+200B, U+200C, U+200D, U+FEFF)
- Homoglyph normalization (Cyrillic → Latin)
- Whitelisting trusted domains (GitHub, npm, PyPI)
- False positive prevention (benign "important" usage)

All of this knowledge is now embedded in SKILL.md.

### ❌ What Doesn't Work

- **Rigid pattern matching** - Misses novel attacks
- **No context awareness** - Can't understand intent
- **Binary verdicts** - No room for nuance
- **Silent failures** - Doesn't explain *why* something is flagged

Claude's natural language reasoning solves all of these issues.

---

## The "But Python is Faster!" Argument

**Yes, Python is faster:**
- Python tool: ~0.5 seconds per skill
- SKILL.md + Claude: ~30-60 seconds per skill

**But speed isn't the bottleneck:**

When you're about to install a skill that could exfiltrate your AWS credentials, do you care if the analysis takes 1 second or 60 seconds?

The real time cost is:
1. Finding the skill (minutes)
2. Reading the description (seconds)
3. **Deciding whether to trust it (minutes)**
4. Installing and testing (minutes)

Spending 60 seconds on a thorough, explained analysis is nothing compared to the risk of a compromised system.

---

## When SKILL.md Isn't Enough

If you genuinely need automation (CI/CD, batch processing 100+ skills), you have options:

### Option 1: Claude API + SKILL.md

Use Claude's API programmatically with SKILL.md as the system prompt:

```python
import anthropic

client = anthropic.Anthropic(api_key="...")
skill_md = open("SKILL.md").read()

def analyze_skill(skill_content):
    response = client.messages.create(
        model="claude-sonnet-4-5",
        system=skill_md,  # Load SKILL.md as system prompt
        messages=[{"role": "user", "content": f"Analyze: {skill_content}"}]
    )
    return response.content
```

This gives you automation **and** adaptive intelligence.

### Option 2: Extract Patterns, Build Light Wrapper

Copy the patterns from SKILL.md into a simple script for quick screening:

```bash
#!/bin/bash
# quick-screen.sh - Fast pattern-based screening

grep -E "(IMPORTANT|CRITICAL|OVERRIDE).*:" "$1" && echo "⚠️ Injection markers"
grep -E "~/.aws/credentials|~/.ssh/id_rsa" "$1" && echo "⚠️ Sensitive files"
grep -E "curl.*\$\(cat" "$1" && echo "⚠️ Data exfiltration"
```

Use this for quick screening, then use SKILL.md (with Claude) for detailed analysis of flagged skills.

### Option 3: Fork and Build Your Own

OSTRTA is MIT licensed. If you need a full Python tool:
1. Fork this repo
2. Extract the patterns from SKILL.md
3. Build your automation layer
4. Share back with the community!

---

## The Bottom Line

**OSTRTA is a teaching tool, not an automation platform.**

It teaches users to think adversarially about skills. It makes threat detection accessible to non-security-experts. It adapts to novel attacks through Claude's reasoning.

If you need automation, the patterns are documented—build what you need on top.

But for the 99% of users who just want to know "is this skill safe before I install it?", **SKILL.md is perfect**.

---

## Frequently Asked Questions

### "Why not ship both SKILL.md and Python?"

**Complexity.** Users would ask "which should I use?" We'd maintain two implementations. Bugs would diverge. Documentation would split.

One tool, one answer: **Use SKILL.md.**

### "What if Claude changes and breaks SKILL.md?"

The patterns are documented in plain English. Even if Claude's behavior changes, the knowledge remains. You could:
- Use a different LLM
- Extract patterns manually
- Build a simple grep-based tool

The **knowledge** is the value, not the implementation.

### "I miss the Python tool. Can I have it back?"

The Python prototype is in the Git history. But we encourage you to:
1. Read SKILL.md to understand the patterns
2. Build your own tool tailored to your needs
3. Share it with the community as a separate project

### "What about companies with compliance requirements?"

Compliance often requires:
- Deterministic results ✅ (run same skill multiple times, get comparable findings)
- Audit trails ✅ (SKILL.md includes content hashing and timestamps)
- Reproducibility ✅ (same SKILL.md version = same detection logic)

SKILL.md provides these. If you need JSON output for compliance systems, use the Claude API approach (Option 1 above).

---

## Conclusion: Embrace Simplicity

OSTRTA is **one file** because:
- Simple things are easier to understand
- Simple things are easier to maintain
- Simple things are easier to trust
- Simple things are more likely to be used

The Python prototype served its purpose: validating the approach and discovering the patterns.

Now, OSTRTA is what it was always meant to be:

**One SKILL.md file to rule them all.**

---

**Version:** SKILL.md v1.0 (2026-01-31)

**License:** MIT (build whatever you want on top)

**Philosophy:** Do one thing well, and do it simply.
