# Errors I Found In Phase 1

## Error 1

### Error: Using `#` prefix inside `getElementById`

### Context:

    document.getElementById("#todo-form");

### Why: `getElementById` does not accept CSS selector syntax — passing `#` returns `null`, crashing everything that follows

### Solution: Remove the `#` prefix

    document.getElementById("todo-form");

---

## Error 2

### Error: Loading `<script>` in `<head>` before the DOM is ready

### Context:

    <head>
      <script src="app.js"></script>
    </head>

### Why: JS runs before the browser parses `<body>`, so every `getElementById` returns `null` — throws `Cannot read properties of null (reading 'addEventListener')`

### Solution: Move `<script>` to the bottom, just before `</body>`

---

## Error 3

### Error: Registering `addEventListener` inside `render()`

### Context:

    function render() {
      todoForm.addEventListener("submit", function() { ... });
    }

### Why: Every `render()` call stacks a new listener on the same element — behavior multiplies with each re-render

### Solution: Move event listeners outside `render()`, to the top level of the file

---

## Error 4

### Error: Variable name collision inside `forEach`

### Context:

    todos.forEach(function(todo) {
      let todo = document.createElement("li");
    });

### Why: The callback parameter and the inner `let` share the same name — JS throws a duplicate declaration error

### Solution: Rename the inner variable to something distinct like `li`

---

## Error 5

### Error: Not assigning the result of `filter` back to the array

### Context:

    todos.filter((t) => t.id !== todo.id);

### Why: `filter` returns a new array — the original `todos` stays unchanged and nothing gets deleted

### Solution: Assign the result back

    todos = todos.filter((t) => t.id !== todo.id);

---

## Error 6

### Error: Using `=` (assignment) instead of `===` (comparison) inside `filter`

### Context:

    return (todo.completed = false);

### Why: `=` assigns `false` to `todo.completed` on every filter run — silently corrupts data in the array

### Solution: Use strict equality

    return todo.completed === false;

---

## Error 7

### Error: Not syncing checkbox state when re-rendering

### Context:

    const checkbox = document.createElement("input");
    checkbox.type = "checkbox";
    // missing: checkbox.checked = todo.completed

### Why: Every `render()` creates a fresh checkbox with no knowledge of the existing state — always appears unchecked

### Solution: Set `checked` from the source of truth

    checkbox.checked = todo.completed;

---

## Error 8

### Error: Using `await` outside an `async` function

### Context:

    const data = fetch("...");
    return await data;

### Why: `await` is only valid inside an `async` function — and `return` at script level is also invalid

### Solution: Wrap inside an `async` function

    async function getData() {
      const response = await fetch("...");
      const data = await response.json();
    }
    getData();

---

## Error 9

### Error: Calling `.push()` on a DOM element

### Context:

    li.push({ title: todo.title });

### Why: `.push()` is an Array method — DOM elements don't have it

### Solution: Use `textContent` to set text on the element

    li.textContent = todo.title;

---

## Error 10

### Error: Accessing a `const`/`let` variable outside the block it was declared in

### Context:

    try {
      const response = await fetch("...");
    }
    const data = await response.json(); // response is not defined here

### Why: Block-scoped variables (`const`/`let`) only exist inside the `{}` they were declared in

### Solution: Keep all logic that depends on `response` inside the same `try` block