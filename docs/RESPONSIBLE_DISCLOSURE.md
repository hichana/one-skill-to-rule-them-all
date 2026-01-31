# Responsible Disclosure Policy

## Purpose

OSTRTA is a security tool designed to detect malicious patterns in SKILL.md files. As with any security tool, it's possible to discover techniques that bypass OSTRTA's detection mechanisms.

This policy establishes guidelines for responsibly reporting such discoveries to give the OSTRTA team time to fix vulnerabilities before they become publicly known.

## Scope

This policy applies to:

1. **Detection Bypasses** - Techniques that evade OSTRTA's analysis
2. **Obfuscation Methods** - Novel encoding/hiding techniques OSTRTA doesn't recognize
3. **False Negatives** - Malicious patterns OSTRTA fails to detect
4. **Security Vulnerabilities in OSTRTA** - Bugs that could be exploited

## Reporting Process

### Private Disclosure

If you discover a security issue or bypass technique:

1. **DO NOT publish the bypass immediately**
2. **Report privately** via GitHub Security Advisories:
   - https://github.com/[your-org]/one-skill-to-rule-them-all/security/advisories

3. **Include in your report**:
   - **Description**: Clear explanation of the bypass/vulnerability
   - **Proof of Concept (PoC)**: Minimal example demonstrating the issue
   - **Impact**: What malicious behavior could go undetected
   - **Suggested Fix**: If you have ideas on how to fix it (optional)
   - **Your Attribution Preference**: How you'd like to be credited (or remain anonymous)

### What to Expect

1. **Acknowledgment**: We'll acknowledge your report within 48 hours
2. **Assessment**: We'll assess severity and impact (typically 1-2 weeks)
3. **Fix Timeline**:
   - **Critical issues** (active exploits in the wild): 7-14 days
   - **High severity**: 30 days
   - **Medium/Low severity**: 60-90 days
4. **Public Disclosure**: Coordinated disclosure after fix is released
5. **Credit**: You'll be credited in release notes (unless you prefer anonymity)

## Responsible Disclosure Timeline

We request that security researchers:

- **Give us 30-90 days** to fix issues before public disclosure
- **Coordinate disclosure timing** with the OSTRTA team
- **Provide advance notice** (at least 7 days) before publishing details

## What We Commit To

The OSTRTA team commits to:

- **Respond promptly** to security reports
- **Keep you informed** of progress on fixes
- **Give credit** for responsible disclosure (unless you prefer anonymity)
- **Not take legal action** against researchers who follow this policy
- **Prioritize fixes** based on severity and impact

## Out of Scope

The following are **NOT** covered by this policy:

- **Theoretical bypasses** without practical demonstration
- **Social engineering** attacks (e.g., tricking users into ignoring warnings)
- **Zero-day exploits** in dependencies (report to upstream maintainers)
- **Intentional limitations** documented in DISCLAIMER.md (OSTRTA makes no guarantee of detecting all malicious content)

## Reporting Malicious Skills

If you discover a malicious skill in the wild (on ClawdHub, GitHub, etc.):

1. **Report to the platform** (ClawdHub, GitHub, etc.) first
2. **Optionally share with OSTRTA** as a test case to improve detection
3. **Include attribution** to the skill author if possible

### Evidence-Based Reporting

When reporting malicious skills:

- **Provide evidence**: Specific malicious patterns or behaviors
- **Avoid accusations of intent**: Describe what the code does, not what the author "intended"
- **Consider false positives**: Could this be a legitimate use case?

## Bug Bounty Program

**Status**: Not currently available

We may implement a bug bounty program in the future. Check this document for updates.

## Examples of Responsible Disclosure

### Good Example ✅

**Researcher discovers novel Unicode obfuscation:**

1. Emails security@ostrta: "I found a Unicode normalization bypass using [technique]. Here's a PoC: [minimal example]"
2. Gives OSTRTA 60 days to fix
3. Coordinates public disclosure after fix is released
4. Gets credited in release notes

### Bad Example ❌

**Researcher discovers bypass and immediately publishes:**

1. Tweets: "OSTRTA is useless! Here's how to bypass it: [detailed technique]"
2. No advance notice to OSTRTA team
3. Attackers immediately start using the technique
4. OSTRTA users are left vulnerable

## Legal Safe Harbor

OSTRTA operates under the MIT License and commits to:

- **Not pursue legal action** against researchers who:
  - Follow this responsible disclosure policy
  - Act in good faith
  - Don't violate laws (e.g., unauthorized access to systems)
  - Don't harm OSTRTA users or the project

## Questions?

If you have questions about this policy, contact us via:
- GitHub Issues: https://github.com/[your-org]/one-skill-to-rule-them-all/issues

## Policy Updates

This policy may be updated over time. Check this document for the latest version.

**Last updated**: 2026-01-31

---

**Thank you for helping make OSTRTA more secure through responsible disclosure!** 🛡️
