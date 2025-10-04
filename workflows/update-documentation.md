---
description: Update docstrings, comments, and logs to be up-to-date with source code
auto_execution_mode: 1
---

1. Carefully review the active file to understand the source code. If needed, review other related files as well.

2. Identify any outdated documentation like docstrings, comments, and logs that need to be updated or can be improved.

3. Finally, update the identified outdated documentation to accurately reflect the source code following the best documentation practice detailed in Google Python Style Guide "3.8 Comments and Docstrings" section (https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)

## 1. General Principles

- **Clarity before brevity**: Code should be readable; comments explain *why*, not *what* (the code itself shows *what*).

- **Keep it up‑to‑date**: Out‑of‑date comments are more harmful than missing ones.

- **One style, one place**: Stick to the conventions defined here; don't mix them with other style guides.

## 2. Inline & Block Comments

### Inline comment

- Placed on the same line as the code.

- Begin with a single space and two spaces before the `#`.

- Keep the comment brief (≤ 80 characters).

### Block comment

- Starts on a line above one or more lines of code.

- Each line begins with `#` followed by a space.

- Describe *why* something is done, not *what*.

## 3. Docstrings

Docstrings provide documentation for modules, classes, functions, and methods. They are written in triple‑quoted string literals ("""Docstring""").

Example (Function):

```python
def add(a, b):
    """Return the sum of *a* and *b*.

    The function simply returns the arithmetic sum. It does not
    modify the input arguments.

    Args:
        a (int): The first operand.
        b (int): The second operand.

    Returns:
        int: The sum of *a* and *b*.

    Raises:
        TypeError: If either *a* or *b* is not an int.

    Example:
        >>> add(2, 3)
        5
    """
    if not isinstance(a, int) or not isinstance(b, int):
        raise TypeError('Both arguments must be integers')
    return a + b
```

Example (Class):

```python
class Point:
    """Represent a point in 2‑D space.

    The :class:`Point` class stores X and Y coordinates and provides
    basic geometric operations.

    Attributes:
        x (float): The X coordinate.
        y (float): The Y coordinate.

    Example:
        >>> p = Point(3, 4)
        >>> p.distance_from_origin()
        5.0
    """

    def __init__(self, x, y):
        self.x = float(x)
        self.y = float(y)

    def distance_from_origin(self):
        """Return the Euclidean distance from the origin."""
        return (self.x ** 2 + self.y ** 2) ** 0.5
```

Example (Module):

```python
"""Utility functions for mathematical operations.

This module contains functions that perform basic arithmetic,
as well as helpers for working with complex numbers.

Modules in the project should start with a one‑line summary
followed by a blank line and a more detailed description if needed.

Examples:
    >>> from math_utils import factorial
    >>> factorial(5)
    120
"""
```