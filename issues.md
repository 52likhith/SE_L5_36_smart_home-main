# Static Code Analysis Issues

| Issue | Type | Line(s) | Description | Fix Approach |
|---|---|---|---|---|---|
| Unsafe shell command execution | Security - High | 49-51 | `runScene()` constructs a shell command using a supplied script name and executes it with `shell=True`, creating a command-injection risk. | Replace shell-based execution with a validated script selection and safe argument-list execution. |
| Binding to all network interfaces | Security - Medium | 4 | `HUB_HOST` is set to `0.0.0.0`, which exposes a service on all available network interfaces if used as a bind address. | Use an appropriate configurable bind address and distinguish it from the client-facing address. |
| Mutable default argument | Code quality / Bug | 11 | `settings={}` is a mutable default that can be shared across calls, potentially causing settings to leak between devices. | Use `None` as the default and create a new dictionary inside the function. |
| Incorrect settings assigned to a light | Logic bug | 28, 63, 67 | The temperature-setting function accesses the light's settings before checking its device type, and the demonstration output shows the light receiving a temperature setting. | Validate the device type and initialize thermostat settings only for thermostats. |
| Inconsistent naming conventions | Style | 11, 16, 23, 32, 36, 40, 45, 49 | Several function names and parameters use camelCase instead of Python's `snake_case` convention. | Rename functions and parameters consistently and update their call sites. |
| Missing docstrings | Maintainability | Module and function definitions | The module and functions lack descriptive docstrings. | Add concise docstrings explaining the module and each function's purpose. |
| Formatting violations | Style | 6, 40, 66, 68 | Flake8 reports whitespace, blank-line, inline-comment spacing, and line-length violations. | Apply PEP 8 formatting and wrap long lines. |
| File encoding unspecified | Code quality | 41 | `open()` does not explicitly specify a text encoding. | Specify `encoding="utf-8"` when opening the JSON file. |