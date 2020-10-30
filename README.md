# Switch Action

**Github Action for to allow switch/case logic.**

Allows performing logic tasks like:
```javascript
switch (${{ job.status == 'success' }}, ${{ github.ref == 'master' }}) {
  case (false, true):
    return "a job failed on master!"
  case (false, false):
    return "a job failed on some branch."
  default:
    return "the job succeeded"
}
```

## Example usage

**True/False Evaluation:**
```yaml
- uses: justAnotherDev/switch-action@v1
  id: switch
  with:
    expression: ${{ job.status != 'failure' }}
    case_a: true
    statement_a: happy
    case_b: false
    statement_b: sad

- run: echo "${{ steps.switch.outputs.value }}"   # will be "happy" or "sad"
```

**String Evaluation:**
```yaml
- uses: justAnotherDev/switch-action@v1
  id: switch
  with:
    expression: ${{ job.status }}
    case_a: success
    statement_a: happy
    case_b: failure
    statement_b: sad
    case_c: cancelled
    statement_c: tbd
    default: pensive

- run: echo "${{ steps.switch.outputs.value }}"   # will be "happy", "sad", "tbd" or "pensive"
```

**Tuple Evaluation (can provide as many variables in the expression as you'd like):**
```yaml
- uses: justAnotherDev/switch-action@v1
  id: switch
  with:
    expression: (${{ job.status == 'success' }}, ${{ github.ref == 'master' }})
    case_a: (false, true)
    statement_a: a job failed on master!
    case_b: (false, false)
    statement_b: a job failed on some branch.
    default: the job succeeded

- run: echo "${{ steps.switch.outputs.value }}" # will be "a job failed on master!", "a job failed on some branch." or "the job succeeded"
```

## Inputs

### `expression`

The expression to evaluate. **Required**

### `case_a`

The first case to handle. **Required**

### `case_b`

The second case to handle.

`...`

### `case_z`

The twenty-sixth case to handle.

### `statement_a`

The first statement to handle. **Required**

### `statement_b`

The second statement to handle.

`...`

### `statement_z`

The twenty-sixth statement to handle.

### `default`

The statement to use when a matching case is not found.

## Outputs

### `value`

The matching statement.
