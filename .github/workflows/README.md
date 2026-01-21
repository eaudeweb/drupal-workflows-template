# GitHub Actions Workflows - Automated Sync

This repository uses automated workflow synchronization from a central template repository to maintain consistent CI/CD practices across all projects.

Action used for synchronization: https://github.com/AndreasAugustin/actions-template-sync

## How It Works

Workflows in this repository are automatically synchronized from the central template repository:  
**[eaudeweb/drupal-workflows-template](https://github.com/eaudeweb/drupal-workflows-template)**

- **Sync Frequency**: Daily at midnight UTC
- **Sync Method**: Automatic pull requests
- **Template Branch**: `main`

When the template repository is updated, a pull request will be automatically created in this repository with the latest workflow changes.

## Available Workflows

The template includes workflows for:

- **Production Deployment** - Automated deployment to production environment
- **Test Deployment** - Automated deployment to test environment  
- **SQL Dump** - Database backup workflows for test/prod
- **Database Sync** - Synchronization between environments

## Project Configuration

### Workflow Synchronization Setup

To enable automatic workflow synchronization, you need to configure one secret:

| Secret Name | Description | How to Get |
|-------------|-------------|------------|
| `TEMPLATE_SYNC_TOKEN` | Personal Access Token for syncing workflows | Ask someone from DevOps Team |

### Workflow Configuration

The workflows in this repository are standardized and use the following **GitHub Secrets** and **Variables** for project-specific configuration.

#### Required Secrets

Configure these in **Settings** → **Secrets and variables** → **Actions** → **Secrets**:

| Secret Name | Description | Example |
|-------------|-------------|---------|
| `PROD_SSH_USER` | SSH username for deployments | web |
| `PROD_SSH_HOST` | Host for production deployments | IP |
| `PROD_SSH_KEY` | SSH private key for production deployments | (private key content) |
| `STATUSCAKE_API_KEY` | api key for connection to StatusCake |  |
| `PROD_STATUSCAKE_ID` | id of the production website in StatusCake |  |

#### Required Variables

Configure these in **Settings** → **Secrets and variables** → **Actions** → **Variables**:

| Variable Name | Description | Example |
|-------------|-------------|---------|
| `PROD_PROJECT_DIR` | where the project is located on the production server | /var/www/html/example.org |
| `PROD_ARTIFACTS_DIR` | where artifacts are stored on the production server | /var/www/artifacts/example.org |
| `PROD_SETTINGS_FILE ` | where settings.local.php file is located on the production server | /var/www/config/example.org/settings.local.php |
| `PROD_PUBLIC_FILES_DIR` | where public files are stored on the production server | /var/www/config/example.org/files |
| `PROD_ROBO_FILE` | where the Robo file is located on the production server | /var/www/config/example.org/robo.yml |
| `PROD_LOCAL_SERVICES_FILE` | if used, where the local services file is located on the production server | /var/www/config/example.org/services.local.yml |
| `PROD_DATABASE_DUMP_DIR` | where database dumps are stored on the production server | /var/www/config/example.org/sync |
| `RETAIN_RELEASES` | how many releases to retain on the production server | 5 |
| `PROD_URL` | production website URL | https://example.org  |

> **Note**: Same variables and secrets are needed for the test environment workflows, prefixed with `TEST_` instead of `PROD_`.

## Customizing Workflow Synchronization

Not all projects need all workflows. For example, if your project doesn't have a test environment, you don't need test deployment workflows.

### Using `.template-sync-ignore`

Create a `.template-sync-ignore` file in `.github/workflows/`exclude specific workflows from being synced.:
```bash

# Ignore test-related workflows (if no test environment exists)
.github/workflows/*test*.yml
.github/workflows/deploy-test.yml
```

## Handling Sync Pull Requests

When workflows are updated in the template, you'll receive an automated pull request with the changes, labeled with `template-sync`, `automated` and specific message.

## Manual Sync Trigger

If you need to sync workflows immediately (without waiting for the daily schedule):

1. Go to **Actions** tab
2. Select **"Sync workflows from template"** workflow
3. Click **"Run workflow"** button

A new pull request will be created with any available updates.
