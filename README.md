# GitHub Actions

# What do we need?
1. Run tests every time code is pushed to the server
2. Run tests before merging branches into `develop` and `master`
3. Move code from one env (QA) to the other (PROD)

## Workflows

- `name`: Workflow name. Displayed in the Actions menu.
- `run-name`: Workflow's run name. It can be diff every time you run the worflow (if omitted, then it's set to event-specific information about the run i.e. last commit message).
- `on`: Event name which defines when a workflow should run. It accepts multiple event names via `[name_1, name_2]` syntax.

    Some events have activity types, providing a more granular way to control when a workflow should run.

    For example, to run a workflow when a label is created, the `on` prop should be configured as follow:
    ```yml
    on:
        label:
            types:
                - created
    ```

    It's also possible to filter events, another way of controlling when a worker should run.

    In this example, the workflow will run when a push occurs in the branches named `main` or `'releases/**'` (release/anything)
    ```yml
    on:
        push:
            branches:
                - main
                - 'releases/**'
    ```


    https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows

- `jobs`: A workflow is made up of one or more `jobs`, which run in parallel by default. Each job runs in a runner env specified by `runs-on`.
- `runs-on`: When using GitHub-hosted runnner, each job runs in a fresh instance of a runner image specified by this prop. https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#choosing-github-hosted-runners
- `jobs.<job_id>.steps.run`: Runs command-line programs using the OS's shell. Each `run` keyword represents a new process and shell in the runner env. Multi-line commands run in the same shell.
    ```yml
    - name: Install Dependencies
      run: npm install

    - name: Multi-Line Command
      run: |
      npm ci
      npm run build
    ```