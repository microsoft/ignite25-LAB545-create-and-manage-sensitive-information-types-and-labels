# GitHub Workflows

This directory contains GitHub Actions workflows for automating repository management tasks.

## add-to-project.yml

Automatically manages new issues by:

1. **Adding to Project**: Adds newly opened issues to a specified GitHub project
2. **Applying Labels**: Automatically applies relevant labels based on issue content

### Configuration

#### Required Setup

1. **Project URL**: Set the `PROJECT_URL` repository variable to your GitHub project URL
   - Go to repository Settings → Secrets and variables → Actions → Variables
   - Add variable: `PROJECT_URL` with value like `https://github.com/orgs/microsoft/projects/123`

#### Default Labels Applied

- `triage` - Applied to all new issues for initial review
- `ignite-lab` - Applied to identify issues related to this Ignite lab

#### Priority Labels (Auto-applied)

- `priority:high` - Issues containing "urgent" or "critical" keywords
- `priority:medium` - Issues containing "bug" or "error" keywords  
- `priority:low` - All other issues

### Permissions

The workflow uses the default `GITHUB_TOKEN` which has sufficient permissions for:
- Adding issues to projects (when properly configured)
- Adding labels to issues

### Usage

The workflow automatically triggers when new issues are opened. No manual intervention required.

### Troubleshooting

If issues are not being added to the project:
1. Verify the `PROJECT_URL` variable is set correctly
2. Ensure the repository has access to the specified project
3. Check workflow run logs in the Actions tab for specific error messages