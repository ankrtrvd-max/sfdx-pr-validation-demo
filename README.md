# sfdx-pr-validation-demo
This is for the Jenkins with Github Exercise 2 of DevOps enablement

Branching Strategy

main: Production branch. Only release-ready code.

develop: Integration branch for features.

feature/*: Used by developers for new changes.

2. PR Workflow

Developers raise PRs from feature/* → develop (or develop → main for releases).

GitHub branch protection ensures:

PR required before merging.

Jenkins validation status check must pass.

3. Jenkins Integration

Jenkins is integrated with GitHub via webhook / PR Builder plugin.

Jenkinsfile is triggered on PR creation/update.

Pipeline stages:

Checkout code.

Authenticate with DevHub.

Create scratch org.

Validate deployment (force:source:push).

Run Apex tests.

4. Notifications

Jenkins sends email notifications on failure to dev-team@example.com.

5. Expected Outcomes

Only validated PRs can be merged.

Logs display validation + test results.

Team notified immediately if validation fails.

6. Benefits

Prevents broken code from entering shared branches.

Early detection of deployment/test issues.

Automated enforcement of Salesforce DevOps best practices.
