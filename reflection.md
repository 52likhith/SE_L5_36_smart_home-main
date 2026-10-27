# Reflection

## 1. Which tool found the most useful issues?

Pylint was useful for identifying code quality problems, including naming conventions, missing documentation, and maintainability issues. Flake8 helped identify formatting violations, while Bandit highlighted potential security risks, particularly unsafe shell command execution.

## 2. Which issue was the most dangerous?

The original use of `subprocess.call()` with `shell=True` and a command constructed from the supplied script name was the most dangerous finding: Bandit classified it High (B602) because it could allow command injection. The updated implementation validates that the resolved script remains inside the scenes directory, restricts it to `.sh` files, and uses an argument list with `shell=False`.

## 3. What did you learn about static analysis?

Static analysis tools identify potential defects without requiring every possible execution path to be tested. Different tools focus on different areas: Pylint checks code quality, Flake8 checks style and formatting, and Bandit checks common security risks. The original Bandit findings also included a Medium B104 warning for binding to all interfaces and a Low B105 warning for a hardcoded password; changing the host to loopback and loading the password from the environment addressed them. Passing the tools does not guarantee that a program is completely secure or bug-free.

## 4. What would you improve next?

I would add automated unit tests for device registration, device toggling, temperature limits, room filtering, power calculations, and state saving. Bandit still reports Low B404 and B603 warnings because the program intentionally uses `subprocess`; neither warning has been suppressed. I would continue to review the subprocess boundary and test scene execution with valid and invalid paths.
