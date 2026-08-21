# Errors I Found In Phase 2

## Error 1

### Error: Renaming an object destructur with `=`,

### Context:

    //const {name, role} = user
    // const {name = resName, role = resRole} = response

### Why: = active only if the property is undefined in the object

### Solution: The `:` character should be used instead.

## Error 2

### Error: Dont use 'type="module"' when using ES Modules

### Context:

    <script src="main.js" type=""></script>

### Why: Browser treat JavaScript As Classic Script and import/export will not work

### Solution: Add 'type="module"'

## Error 3

### Error: Using the wrong path like 'utils.js'

### Context:

    import {name} from 'utils.js'

### Why: ES Modules browsers must make explicit relative paths

### Solution: Add './' like './utils.js'

## Error 4

### Error: Not fill anything in .then()

### Context:

    fetch('https://jsonplaceholder.typicode.com/users/1').then()

### Why: .then() is after this promise, just run next function

### Solution: Use Arrow Function and fill res with something

## Error 5

### Error: response.json not return data first, but promise

### Context:

    fetch('https://jsonplaceholder.typicode.com/users/1').then(res => {
            const data = res.json()
            name.textContent = data.name
        })

### Why: Because will return promise

### Solution: Use .then(with arrow function) again and show to dom

        const response = fetch('https://jsonplaceholder.typicode.com/users/1').then(res => {
        const data = res.json().then(data => {
            name.textContent = data.name
        })
    })

## Error 6

### Error: Nested .then(), that should be flat chain

### Why: Flat chain is better than Nested .then()

### Solution: Use Flat Chain

    fetch(url)
    .then(res => res.json())
    .then(data => {
    name.textContent = data.name
     })