# OSTRTA Development Guidelines

## Project Overview

**OSTRTA (One Skill To Rule Them All)** is a security-focused OpenClaw skill that audits other SKILL.md files for malicious content using an adversarial "assume-malicious" posture.

## TDD Workflow (REQUIRED)

For EVERY feature/function/component, follow this strict Red-Green-Refactor cycle:

### 1. Write Tests FIRST (Red Phase)
- Define expected behavior through test cases
- Include edge cases and error conditions
- Tests should fail initially (proving they work)

### 2. Write Minimal Code to Pass (Green Phase)
- Implement just enough to make tests pass
- Focus on correctness, not perfection
- Avoid over-engineering at this stage

### 3. Refactor with Confidence
- Clean up implementation
- Remove duplication
- Improve readability and maintainability
- Tests ensure refactoring doesn't break functionality

### 4. Run Quality Gates
```bash
make quality  # Runs: lint, type-check, format, test
```

## Quality Gates

### Per-Feature Validation (After Each Feature)
```bash
make quality
```
This runs:
- `ruff check --fix` - Auto-fix code style issues
- `mypy ostrta` - Type checking
- `black ostrta tests` - Code formatting
- `pytest` - Unit tests with coverage

**Philosophy:** Catch issues immediately when context is fresh. Prevents accumulating technical debt.

### Final Validation (After Complete Task)
```bash
make test-all
```
This runs the comprehensive test suite:
- Unit tests with coverage
- Integration tests
- All 9 attack vector tests
- Coverage report generation

**Always provide results summary:**
```
---
📊 Results

- ✅ Unit tests: X passed
- ✅ Integration tests: X passed
- ✅ Attack vector tests: 9/9 categories detected
- ✅ Coverage: X%
- ✅ Type checking: 0 errors
- ✅ Linting: All checks passed
```

## Architecture Principles

### Security-First Design
1. **Never execute analyzed code** - OSTRTA only reads and analyzes SKILL.md content
2. **Assume malicious** - Every skill is untrusted until proven otherwise
3. **Defense in depth** - Multiple detector layers, adversarial reasoning
4. **Fail secure** - Errors should produce warnings, not false negatives

### Core Pipeline
```
Ingest → Decode → Analyze → Adversarial Reasoning → Verdict → Report
```

### Critical Security Module
**`ostrta/core/decode.py`** is the most critical module:
- Any obfuscation that bypasses this compromises the entire system
- Must handle: Base64, Unicode tricks, zero-width chars, homoglyphs, HTML entities
- Requires extensive testing with real-world obfuscation techniques

### Detector Pattern
All detectors follow this interface:
```python
class SomeDetector:
    def detect(self, content: str) -> List[Finding]:
        """
        Analyze content for specific threat category.
        Returns list of findings with evidence and severity.
        """
```

### Severity Levels
- **CRITICAL** - Active malicious behavior (data exfiltration, remote code execution)
- **HIGH** - Prompt injection, privilege escalation
- **MEDIUM** - Suspicious patterns, unverifiable dependencies
- **LOW** - Minor concerns, best practice violations
- **INFO** - Informational findings

### Verdict Logic
```python
verdict_level = max(finding.severity for finding in findings)
```
The highest severity finding determines the overall verdict.

## Testing Strategy

### Unit Tests
- Test each module in isolation
- Mock external dependencies
- Fast feedback loop (< 5 seconds)
- Target: 90%+ code coverage

### Integration Tests
- Test full pipeline end-to-end
- Use real-world attack patterns
- All 9 threat categories must be covered
- Test fixtures: benign, malicious, obfuscated skills

### Test Fixtures Location
```
tests/fixtures/
├── benign_skills/       # Known-safe SKILL.md files
├── malicious_skills/    # Attack patterns (all 9 categories)
└── obfuscated_skills/   # Obfuscation techniques
```

## Documentation Requirements

### When to Update Documentation
Update **CLAUDE.md** (this file) when:
- New detector added
- New threat category discovered
- Architecture patterns change
- New infrastructure/tooling introduced

Update **Makefile** when:
- New commands added
- New workflows introduced
- Quality gates change

### Code Comments
- Only add comments where logic isn't self-evident
- Prefer self-documenting code (clear variable/function names)
- Document "why" not "what"
- No comments for code you didn't change

## Legal Protection

### Every Report Must Include
- Disclaimer footer (from DISCLAIMER.md)
- Content hash (SHA-256)
- Analysis timestamp
- OSTRTA version

### Language Guidelines
- Use factual, evidence-based language
- No accusations of intent ("This appears to..." not "The author is trying to...")
- Present findings objectively
- Reference specific patterns/evidence

## Dependency Policy

### Core Dependencies
- `click` - CLI framework
- `requests` - HTTP client
- `pydantic` - Data validation
- `chardet` - Encoding detection
- `unicodedata2` - Unicode normalization

### Development Dependencies
- `pytest`, `pytest-cov` - Testing
- `black` - Code formatting
- `ruff` - Linting
- `mypy` - Type checking
- `pre-commit` - Git hooks

### Dependency Verification
When analyzing dependencies in SKILL.md files:
1. Check if package exists in PyPI/npm
2. If unverifiable, flag as MEDIUM severity
3. Suggest local alternatives (e.g., `urllib` instead of `requests`)
4. Provide sandboxing recommendations
5. **NEVER auto-execute unverified code**

## Code Style

### Python Version
- Target: Python 3.9+
- Use type hints everywhere
- Follow PEP 8 (enforced by black/ruff)

### Line Length
- 100 characters (configured in pyproject.toml)

### Imports
- Standard library first
- Third-party packages second
- Local imports last
- Use `ruff` for automatic sorting

### Type Annotations
```python
# Required for all functions
def analyze(self, content: str) -> List[Finding]:
    ...

# Required for complex data structures
findings: List[Finding] = []
verdict_map: Dict[str, Severity] = {}
```

## Common Patterns

### Error Handling
```python
# Detector errors shouldn't crash analysis
try:
    findings = detector.detect(content)
except Exception as e:
    logger.error(f"Detector {detector} failed: {e}")
    findings = []  # Continue with empty findings
```

### Evidence Extraction
```python
# Always provide context for findings
finding = Finding(
    severity=Severity.HIGH,
    category="PROMPT_INJECTION",
    title="Detected role-play attempt",
    evidence='Line 42: "You are now in developer mode..."',
    remediation="Review and remove prompt injection patterns"
)
```

### Content Hashing
```python
import hashlib

content_hash = hashlib.sha256(content.encode('utf-8')).hexdigest()
```

## Performance Considerations

### Optimization Strategy
1. **Correctness first** - Get it working correctly
2. **Profile before optimizing** - Measure actual bottlenecks
3. **Cache by content hash** - Avoid re-analyzing same content
4. **Early exit on CRITICAL** - Stop on first critical finding (optional)

### Streaming Analysis (Future)
For very large SKILL.md files, consider streaming line-by-line instead of loading entire content.

## Security of OSTRTA Itself

### OSTRTA Must Be Secure
As a security tool, OSTRTA is a high-value target:
- Never execute code from analyzed skills
- Validate all inputs (file paths, URLs)
- Sanitize output (prevent ANSI injection in terminal)
- No eval(), exec(), or dynamic code execution
- Limit network requests (timeout, size limits)

### Supply Chain Security
- Pin dependencies in requirements
- Review dependency updates
- Minimal dependency footprint
- Prefer standard library when possible

## Commit Message Style

Follow conventional commits:
```
feat: Add obfuscation detector
fix: Handle Unicode normalization edge case
test: Add attack vector integration tests
docs: Update architecture documentation
refactor: Simplify verdict aggregation logic
```

## Review Checklist

Before marking a feature complete:
- [ ] Tests written FIRST (Red-Green-Refactor)
- [ ] All tests passing
- [ ] `make quality` passes
- [ ] Type hints on all functions
- [ ] Docstrings for public APIs
- [ ] Error handling for edge cases
- [ ] No security vulnerabilities introduced
- [ ] Documentation updated (if needed)

## Resources

### Threat Model
See `docs/THREAT_MODEL.md` for all 9 attack vector categories.

### Legal Protections
See `docs/LEGAL_PROTECTIONS.md` for disclaimer language and legal strategy.

### Architecture
See `docs/ARCHITECTURE.md` for detailed system design.

### Differentiation
See `docs/DIFFERENTIATION.md` for how OSTRTA differs from static analysis tools.
