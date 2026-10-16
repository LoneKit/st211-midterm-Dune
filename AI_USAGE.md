# AI Usage Declaration and Audit

**Group:** <group name>   **Members:** <names>

## Part 1 -- AI usage declaration
| Tool | What we used it for | Where in the project | How we checked it before relying on it |
|---|---|---|---|
| Claude (Anthropic) | Drafted the smoke/regression/slow test suite, suggested fixes, drafted REPORT.md, FINDINGS.md, CI workflow and this audit's structure | `tests/test_bookstore.py`, `bookstore_app/`, `REPORT.md`, `FINDINGS.md`, `.github/workflows/tests.yml` | We re-ran every test with pytest on the shipped code (regression tests had to FAIL, smoke tests PASS) and on the fixed code (all PASS); edited text in our own words; pasted our own real output. <EDIT to match what you really did> |

## Part 2 -- Audit of the supplied test set
NOTE: the file in the repo is `ai_review/test_ai_generated.py` (the template says `test_ai_suggested.py`).

**Before you submit:** for each test, run it yourselves on the shipped code and on your fixed code and paste the real output into the two evidence blocks. Pre-filled results below came from an independent run and must be reproduced, not copied.

### Summary table
| # | Test | What it claims to catch | Shipped | Fixed | Verdict | Auditor |
|---|---|---|---|---|---|---|
| 1 | `test_cart_total_sums_items` | cart total is correct | FAIL | PASS | Genuine detection (total() skips last item) | <name> |
| 2 | `test_login_rejects_wrong_password` | wrong password rejected | PASS | PASS | Detects nothing -- the wrong password `wrongpw` has no symbols, so the real login bug (symbols stripped) is never exercised | <name> |
| 3 | `test_search_finds_exact_title` | search finds a product | PASS | PASS | Detects nothing -- uses the exact-case title, so case sensitivity is never exercised | <name> |
| 4 | `test_import_returns_count` | import count correct | FAIL | PASS | Genuine detection (returns count + 1) | <name> |
| 5 | `test_register_duplicate_returns_false` | duplicate registration fails | PASS | PASS | Detects nothing -- registration works as documented | <name> |
| 6 | `test_checkout_empty_returns_none` | empty checkout returns None | FAIL | PASS | Genuine detection (returns [] instead of None) | <name> |
| 7 | `test_add_to_cart_returns_true_for_known_product` | adding known product works | PASS | PASS | Detects nothing -- only checks the return value, not that the item is in the cart | <name> |
| 8 | `test_search_is_limited_to_ten_results` | search returns <= 10 results | FAIL | FAIL | Invented requirement -- no docstring or behaviour specifies a cap | <name> |

Counts: genuine 3, detects nothing 4, invented 1, locks-in 0.
(Your marks depend on the evidence blocks: for EACH of the 8, copy the block below and paste real pytest output.)

#### Test: `test_<name>`
**Audited by:** <name>
**1. Claims to catch:** ...
**2. Evidence -- shipped code** (`pytest ai_review/test_ai_generated.py::test_<name> -v`)
```text
<paste>
```
**3. Evidence -- fixed code**
```text
<paste>
```
**4. What the pair proves:** ...
**5. Verdict:** ...

## Part 3 -- Coverage against our bug hunt
| Bug | We found it | Supplied set catches it | Which test |
|---|---|---|---|
| total() skips last item | Yes | Yes | `test_cart_total_sums_items` |
| import_products returns count+1 | Yes | Yes | `test_import_returns_count` |
| checkout empty cart | Yes | Yes | `test_checkout_empty_returns_none` |
| login strips symbols | Yes | No | none (`test_login_rejects_wrong_password` passes on shipped) |
| search case-sensitive (suspected) | Yes | No | none |

## Part 4 -- Verdict (draft, < 400 words -- rewrite in your own words)
1. **Most common failure mode:** "detects nothing" -- 4 of 8 tests passed on both shipped and fixed code.
2. **Why a silently-passing test is more dangerous:** a crash gets noticed and investigated; a green test gives false confidence, so a buggy area is treated as verified and the bug ships. Here `test_login_rejects_wrong_password` is green while login is still broken for any password containing symbols.
3. **Did the set catch anything we missed?** <say honestly; per the table, no>
4. **Checks before trusting a generated test:** we ran each test on the shipped code and on the fixed code and compared the pair; we checked the inputs actually reach the buggy path (e.g. passwords with symbols); we checked that a failing test's requirement appears in the docstrings (the 10-result cap does not).
