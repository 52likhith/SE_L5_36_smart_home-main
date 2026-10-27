# Static Code Analysis Issues

The line numbers below refer to the original `smart_home.py` used for the
`*_before.txt` reports. Pylint reports code-quality categories rather than
security severity levels.

| Issue | Type / Severity | Original line(s) | Original issue and implemented fix |
|---|---|---|---|
| Shell command execution | Bandit B602 - High | 49-51 | The original `runScene()` concatenated a supplied script name into a command and ran it with `shell=True`. The implementation now resolves and validates a script path inside the scenes directory, uses an argument list, and sets `shell=False`. |
| Binding to all interfaces | Bandit B104 - Medium | 4 | The original host was `0.0.0.0`, which represents all network interfaces. It was changed to the loopback address `127.0.0.1`. |
| Hardcoded password | Bandit B105 - Low | 7 | The original password was embedded in source. It is now read from the `SMART_HOME_PASSWORD` environment variable. |
| Mutable default and shared settings | Pylint W0102 / behavior bug | 11 | The original `settings={}` default was shared between calls, so updating the thermostat settings could also appear in other devices, including the light. The default is now `None` and each device receives a copy of its settings. |
| Inconsistent return values | Pylint R1710 | 16 | `toggleDevice()` returned a state for known devices but had no explicit return expression for an unknown one. The renamed `toggle_device()` returns `None` for an unknown device. |
| Naming and missing documentation | Pylint C0103, C0114, C0116 | Module and function definitions | The original lacked module/function docstrings and used camelCase names. Docstrings and snake_case names were added. |
| Unspecified file encoding | Pylint W1514 | 41 | The original JSON file was opened without an explicit encoding. `save_state()` now opens it with UTF-8 encoding. |
| Formatting violations | Flake8 E203, E302, E261, E501 | 6, 40, 66, 68 | The original report identified whitespace before a colon, insufficient blank lines, inline-comment spacing, and an overlong line. Those issues were formatted; a later E501 at line 106 was the remaining violation and is now wrapped without changing the message. |
| `subprocess` import warning | Bandit B404 - Low (still reported) | 2 | Bandit flags use of the `subprocess` module in general. It remains because the scene runner needs subprocess execution; the command is constrained and does not use a shell. |
| Non-shell subprocess warning | Bandit B603 - Low (still reported) | Added in the fix | Bandit flags subprocess execution even with `shell=False`. It remains documented rather than suppressed; the implementation validates the path and invokes the executable with a separate argument list. |

The two remaining Low-severity Bandit warnings are reported without suppressions.