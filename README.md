# Github Action Vulnerabilities
## Code Injection
1. Vulnerable Action
    ```yaml
    name: Demo vulnerable workflow

    on:
    issues:
        types: [opened]

    jobs:
    vuln_job:
        runs-on: ubuntu-latest

        steps:
        # Checkout used for demonstration purposes
        - uses: actions/checkout@v2
        
        - run: |
            echo "ISSUE TITLE: ${{github.event.issue.title}}"
            echo "ISSUE DESCRIPTION: ${{github.event.issue.body}}"
    ```
2. Patch - Use `env` to sanitize the user control input
    ```yaml
    name: Demo vulnerable workflow
    on:
    issues:
        types: [opened]

    jobs:
    vuln_job:
        runs-on: ubuntu-latest

        steps:
        # Checkout used for demonstration purposes
        - uses: actions/checkout@v2
        - env:
            TITLE: ${{github.event.issue.title}}
            DESCRIPTION: ${{github.event.issue.body}}
            
            run: |
            echo "ISSUE TITLE: $TITLE"
            echo "ISSUE DESCRIPTION: $DESCRIPTION"
    ```

### Reference
1. https://cycode.com/blog/github-actions-vulnerabilities/