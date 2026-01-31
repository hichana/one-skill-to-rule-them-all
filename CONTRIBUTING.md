# Contributing to OSTRTA

Thank you for your interest in improving OSTRTA! This project welcomes contributions from security researchers, developers, and users.

---

## How to Contribute

### 1. Test with Real Skills

The most valuable contribution is testing OSTRTA against real-world SKILL.md files:

- Find false positives (benign skills flagged as malicious)
- Find false negatives (malicious skills that pass as safe)
- Test edge cases and novel attack techniques
- Report your findings via GitHub Issues

### 2. Improve Threat Patterns

If you discover a new attack vector or obfuscation technique:

1. Document the pattern in an issue
2. Provide example malicious and benign skills
3. Suggest detection logic
4. We'll add it to SKILL.md

### 3. Add Test Fixtures

Contribute example skills to `tests/fixtures/`:

- `malicious_skills/` - Attack patterns OSTRTA should catch
- `benign_skills/` - Safe skills that shouldn't be flagged
- `obfuscated_skills/` - Encoding tricks

Each fixture should include comments explaining what it tests.

### 4. Improve Documentation

Help make OSTRTA easier to use:

- Fix typos or unclear explanations
- Add examples or tutorials
- Create video walkthroughs
- Write blog posts about OSTRTA

### 5. Build on Top of OSTRTA

Create tools that extend OSTRTA:

- CI/CD integration scripts
- Browser extensions
- IDE plugins
- API wrappers

Share your projects with the community!

---

## Making Changes

### For Documentation

1. Fork the repository
2. Edit the relevant markdown file:
   - `SKILL.md` - Threat patterns and detection logic
   - `README.md` - Main project documentation
   - `USING_OSTRTA.md` - Usage guide
   - `docs/*.md` - Technical documentation
3. Test your changes (read through, check formatting)
4. Submit a pull request

### For Threat Patterns

To add a new threat pattern to SKILL.md:

1. **Identify the category** (or propose a new one)
2. **Document the pattern:**
   - Description of the threat
   - Indicators to detect
   - Example malicious code
   - Example benign code (to prevent false positives)
3. **Add test fixture** in `tests/fixtures/malicious_skills/`
4. **Update SKILL.md** with the pattern
5. **Test manually** - Load SKILL.md in Claude and analyze your test fixture
6. **Submit PR** with:
   - Updated SKILL.md
   - New test fixture
   - Description of what the pattern catches

### For Test Fixtures

1. Create a realistic SKILL.md file demonstrating an attack
2. Add clear comments explaining what should be detected
3. Name the file descriptively (e.g., `homoglyph_credential_theft.md`)
4. Place in the appropriate directory:
   - `tests/fixtures/malicious_skills/` - Should be flagged
   - `tests/fixtures/benign_skills/` - Should pass
   - `tests/fixtures/obfuscated_skills/` - Tests encoding detection
5. Submit PR with the fixture

---

## Pull Request Guidelines

### Good PR Checklist

- [ ] Clear title explaining what the PR does
- [ ] Description of the problem being solved
- [ ] Changes are focused (one logical change per PR)
- [ ] Documentation updated if needed
- [ ] Test fixtures added if new patterns introduced
- [ ] No unrelated changes (formatting, refactoring)

### PR Review Process

1. Submit your PR
2. Maintainers will review within 1-2 weeks
3. Address any feedback
4. Once approved, we'll merge!

---

## Reporting Security Issues

### Found a Bypass?

If you discover a way to evade OSTRTA's detection:

1. **Do NOT publish the bypass publicly**
2. Report privately via [GitHub Security Advisories](https://github.com/[repo]/security/advisories)
3. Include:
   - Description of the technique
   - Example malicious skill that evades detection
   - Suggested fix (if you have one)
4. Allow 30-90 days for a fix before public disclosure

We'll credit you in the release notes and SECURITY.md.

### Responsible Disclosure

We're committed to improving OSTRTA's security. We appreciate researchers who:

- Report privately first
- Give us time to fix before publishing
- Help us understand the impact
- Work with us to verify the fix

---

## Code of Conduct

### Be Respectful

- Treat all contributors with respect
- Welcome newcomers
- Focus on the idea, not the person
- Assume good intent

### Be Constructive

- Provide clear, actionable feedback
- Explain the "why" behind suggestions
- Offer help, not just criticism
- Celebrate good work

### Be Professional

- No harassment or discrimination
- No spam or promotional content
- No off-topic discussions
- Keep it focused on improving OSTRTA

---

## Questions?

- **GitHub Issues:** For bugs, feature requests, false positives/negatives
- **Discussions:** For questions, ideas, show-and-tell

---

## Philosophy

OSTRTA follows the "do one thing well" principle. When contributing, ask:

1. **Does this improve threat detection?** ✅
2. **Does this improve usability?** ✅
3. **Does this maintain simplicity?** ✅

If yes to all three, it's a good contribution!

**See [COMPARISON.md](COMPARISON.md) for the full design philosophy and UNIX principles.**

---

**Thank you for helping make the OpenClaw ecosystem safer!**

---

**License:** By contributing, you agree your contributions will be MIT licensed.
