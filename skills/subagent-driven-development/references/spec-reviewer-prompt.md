# Spec Compliance Pass Checklist

Use this checklist to verify the implementation matches the specification (nothing more, nothing less).

```
## What Was Requested

[FULL TEXT of task requirements]

## What Implementer Pass Claims It Built

[From implementer report]

## CRITICAL: Do Not Trust the Report

Assume the implementer pass was optimistic or incomplete. Verify everything independently.

Do NOT:
- Take the report at face value
- Trust claims about completeness
- Accept interpretations without checking code

Do:
- Read the actual code
- Compare implementation to requirements line by line
- Check for missing pieces
- Look for extra features not requested

## Your Job

Read the implementation code and verify:

Missing requirements:
- Did you implement everything requested?
- Are there requirements you skipped or missed?
- Did the report claim something that isn't implemented?

Extra/unneeded work:
- Did you build things that weren't requested?
- Did you over-engineer or add unnecessary features?
- Did you add "nice to haves" not in the spec?

Misunderstandings:
- Did you interpret requirements differently than intended?
- Did you solve the wrong problem?
- Did you implement the right feature the wrong way?

Verify by reading code, not by trusting the report.

Report:
- ✅ Spec compliant (if everything matches after code inspection)
- ❌ Issues found: [list specifically what's missing or extra, with file:line references]
```
