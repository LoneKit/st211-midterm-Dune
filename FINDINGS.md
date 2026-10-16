# Bug-Hunt Findings Log

Group: <group name>   Members: <names>
(Five entries maximum. Re-run each "Tried" snippet yourselves and paste YOUR output.)

## Bug 1 -- cart.py -> total()
- **Suspected:** the loop is `range(len(self.items) - 1)`, which stops one item early.
- **Tried:** three products priced 10, 20, 30; added all three; called `total()`.
- **Observed:** `total()` returned `30` (10 + 20 only).
- **Expected:** `60`. "Total price of everything currently in the cart" includes every item. A one-item cart returned 0.
- **Fixed:** iterate over `self.items` directly.
- **Author:** <name>

## Bug 2 -- cart.py -> import_products()
- **Suspected:** the function ends with `return count + 1`, while the docstring says "Returns how many were imported".
- **Tried:** imported three `(id, title, price)` tuples.
- **Observed:** returned `4`.
- **Expected:** `3`, the number imported.
- **Fixed:** `return count`.
- **Author:** <name>

## Bug 3 -- cart.py -> checkout()
- **Suspected:** the docstring promises `None` for an empty cart, but there is no empty-cart check.
- **Tried:** `Cart(Catalog()).checkout()` on a brand-new cart, then `history()`.
- **Observed:** returned `[]`, and `history()` returned `[[]]` (an empty order was recorded).
- **Expected:** `None`, and no order recorded.
- **Fixed:** `if not self.items: return None` at the top of the method.
- **Author:** <name>

## Bug 4 -- users.py -> login()
- **Suspected:** the typed password is run through `isalnum()` filtering but the stored one is not, so the comparison is asymmetric.
- **Tried:** register `dave / "p@ss!word"` then log in with the same string; register `erin / "password"` then log in with `"pass!word"`.
- **Observed:** `login("dave","p@ss!word")` -> `False`; `login("erin","pass!word")` -> `True`.
- **Expected:** exact match only: the first is `True`, the second `False`. Passwords must not be silently altered.
- **Fixed:** compare `stored == password` directly.
- **Author:** <name>

## Bug 5 (suspected) -- catalog.py -> search()
- **Suspected:** `keyword in info["title"]` is case-sensitive.
- **Tried:** product "Python Testing", search for `"python"`.
- **Observed:** `[]`.
- **Expected:** `[1]` -- users do not type exact capitalisation. (The docstring only says "contains", so this is a judgement call; confirm with the instructor.)
- **Fixed:** compare `keyword.lower()` with `title.lower()`.
- **Author:** <name>
