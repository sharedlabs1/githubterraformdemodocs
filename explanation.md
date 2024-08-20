# GitHub Actions Terraform Workflow: Line-by-Line Explanation

```yaml
name: 'Terraform CI/CD'
```
- This line sets the name of the workflow. It will appear in the GitHub Actions tab of your repository.

```yaml
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
```
- This section defines when the workflow will run.
- It triggers on two events:
  1. When code is pushed directly to the "main" branch.
  2. When a pull request targeting the "main" branch is opened, updated, or reopened.

```yaml
jobs:
  terraform:
    name: 'Terraform'
    runs-on: ubuntu-latest
```
- This begins the definition of jobs in the workflow.
- There's one job named "Terraform".
- It runs on the latest version of Ubuntu provided by GitHub Actions.

```yaml
    steps:
```
- This begins the list of steps that the job will execute.

```yaml
    - name: Checkout
      uses: actions/checkout@v2
```
- This step checks out your repository code so the workflow can access it.
- It uses version 2 of the `actions/checkout` action.

```yaml
    - name: Setup Terraform
      uses: hashicorp/setup-terraform@v1
```
- This step sets up Terraform in the workflow environment.
- It uses version 1 of the `hashicorp/setup-terraform` action.

```yaml
    - name: Terraform Init
      run: terraform init
```
- This step initializes the Terraform working directory.
- It downloads necessary providers and modules.

```yaml
    - name: Terraform Format
      run: terraform fmt -check
```
- This step checks that all Terraform configuration files are formatted correctly.
- The `-check` flag means it will fail if files are not formatted properly.

```yaml
    - name: Terraform Plan
      run: terraform plan
```
- This step creates an execution plan, showing what actions Terraform will take.
- It doesn't make any changes to real resources.

```yaml
    - name: Terraform Apply
      if: github.ref == 'refs/heads/main' && github.event_name == 'push'
      run: terraform apply -auto-approve
```
- This step applies the Terraform changes, creating/modifying/deleting real resources.
- The `if` condition ensures this only runs on pushes to the main branch, not on pull requests.
- `-auto-approve` flag means it won't ask for interactive approval.

This workflow ensures that:
1. Code is checked and planned on all pull requests to main.
2. Changes are only applied when code is merged (pushed) to the main branch.
3. Terraform files are always properly formatted.
4. The team can review the planned changes before they're applied.
