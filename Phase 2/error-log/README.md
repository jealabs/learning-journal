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
