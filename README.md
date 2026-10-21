# ci-log-cleanup-demo

Demo repository for the team session on automating GitHub repo housekeeping (workflow run log cleanup).

This repo contains a minimal, manually triggered workflow at `.github/workflows/sweep.yml` (event: `workflow_dispatch`) that echoes a couple of lines. We use its runs to demo clearing out workflow run logs.

## Cleaning up run logs

Workflow run logs can pile up in busy repositories. You can delete the log files from your workflow runs either through the GitHub web interface or programmatically. Deleting run logs requires write access to the repository.

### Deleting logs from the web UI

1. On GitHub, navigate to the main page of the repository.
2. Under your repository name, click **Actions**.
3. In the left sidebar, select the workflow you want (for example, **Sweep**).
4. In the list of workflow runs, select the run whose logs you want to delete.
5. In the upper-right corner, select the **{...}** (kebab) dropdown menu, then click **Delete all logs**.
6. Review the confirmation prompt.

After the logs are deleted, the **Delete all logs** button is removed from the run page, which indicates that no log files remain in the workflow run.

### Deleting logs with the REST API

For cleanup at scale (for example, across many runs), use the REST API:

```
DELETE /repos/{owner}/{repo}/actions/runs/{run_id}/logs
```

Example:

```bash
gh api --silent --method DELETE \
  /repos/mcpmark-eval-liuhezi/ci-log-cleanup-demo/actions/runs/RUN_ID/logs
```

A successful call returns `204 No Content`. The official documentation also ships a reusable `delete-logs.sh` script that paginates over all completed runs of a given workflow and deletes each run's logs, which is a good starting point for automating this in CI.

For the full, authoritative instructions (including the example script), see the GitHub Docs: [Using workflow run logs — Deleting logs](https://docs.github.com/en/actions/how-tos/monitor-workflows/use-workflow-run-logs#deleting-logs).
