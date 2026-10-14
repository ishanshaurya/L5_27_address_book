# Lab 5: Issues Table (address_book.py)

Line numbers refer to the original file (before fixes).

## Bandit (commit 6d0580a)

| Issue | Type | Line(s) | Description | Fix Approach |
|-------|------|---------|-------------|--------------|
| B324 weak MD5 hash | Security (High) | 19 | `hashlib.md5` flagged as weak hash for security use | Pass `usedforsecurity=False`, since the hash only makes a short contact id |
| B301 pickle.load | Security (Medium) | 67 | Unpickling can execute arbitrary code from untrusted files | Replace pickle with `json.load` |
| B403 pickle import | Security (Low) | 3 | Importing pickle has security implications | Remove pickle import, use `json` |
| B105 hardcoded password | Security (Low) | 6 | `SYNC_PASSWORD = "contacts-demo"` committed in source | Read it from `os.environ.get("SYNC_PASSWORD", "")` |

## Pylint (commit 58fa2ab), 5.47 to 10.00

| Issue | Type | Line(s) | Description | Fix Approach |
|-------|------|---------|-------------|--------------|
| C0114 missing module docstring | Convention | 1 | No module docstring | Add module docstring |
| C0116 missing function docstring | Convention | 10, 18, 22, 30, 39, 47, 53, 60, 65, 70 | 10 functions had no docstring | Add one-line docstring to each |
| C0103 function names not snake_case | Convention | 10, 18, 22, 30, 39, 47, 53, 60, 65 | camelCase names such as `addContact`, `exportCsv` | Rename to `add_contact`, `export_csv`, etc. and update all calls |
| C0103 argument names not snake_case | Convention | 10, 18, 30, 53, 60, 65 | `fullName`, `fileName` | Rename to `full_name`, `file_name` |
| W0718 broad exception | Warning | 34 | `except Exception` hides unrelated errors | Catch `KeyError`, the only expected failure |
| C0200 range(len()) loop | Refactor | 49 | Index loop over `names` | Use `enumerate(names, 1)` |
| W1514 unspecified encoding | Warning | 54 | `open()` with no encoding | Add `encoding="utf-8"` (also on JSON backup/restore) |
| Module-level `main()` call | Best practice | 88 | Script runs on import | Wrap in `if __name__ == "__main__":` |

## Flake8 (commit 7d5077c)

| Issue | Type | Line(s) | Description | Fix Approach |
|-------|------|---------|-------------|--------------|
| E225 missing whitespace around operator | Style | 23 | `found=[]` | `found = []` |
| E303 too many blank lines (3) | Style | 47 | Three blank lines before function | Reduce to two |
| E231 missing whitespace after ',' | Style | 84 | `"| first:",sorted(...)` | Add space after comma |
| E501 line too long (86 > 79) | Style | 85 | Long `print` call | Break the call across two lines |
| E501 line too long (81 > 79) | Style | 24 (after bandit fix) | `md5(...)` line grew past 79 chars | Split into `digest = ...` then `digest.hexdigest()[:6]` |

## Result

| Tool | Before | After |
|------|--------|-------|
| Pylint | 5.47/10 | 10.00/10 |
| Bandit | 1 High, 1 Medium, 2 Low | 0 issues |
| Flake8 | 4 issues | 0 issues |
