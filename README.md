# Planning and tracking work with GitHub Projects and Issues

This repository contains lab materials and automation for learning how to plan
work with GitHub Issues and Projects. The examples cover issue templates,
labels, milestones, issue types, parent/sub-issue relationships, project views,
and a small issue-comment workflow.

The lab was created for Microsoft Build 2025 and currently expects the
environment provided for that event. It is primarily Markdown documentation,
with a Bash helper and GitHub Actions workflows; it is not a standalone
application.

## Roles and issue types

This repository does not implement application permissions or access-control
roles. Its documented project roles are:

- **Maintainers** — The project lists [@chrisreddington](https://github.com/chrisreddington)
  and [@dmckinstry](https://github.com/dmckinstry) as maintainers. The Code of
  Conduct assigns maintainers responsibility for clarifying community
  standards and responding to unacceptable conduct.
- **Contributors** — People proposing changes. Follow the contribution process
  in [CONTRIBUTING.md](CONTRIBUTING.md) and the expectations in the
  [Code of Conduct](CODE_OF_CONTENT.md).

The issue seed data uses GitHub issue types **Feature**, **Task**, and **Bug**.
Some Tasks name a Feature as their parent, creating parent/sub-issue
relationships. These are sample work-item types, not access roles.

## Requirements

For the event lab, use the environment provided by Microsoft Build 2025. To run
the repository's issue-seeding helper locally, you also need Bash, the GitHub
CLI (`gh`), and `jq`; authenticate `gh` to a disposable lab repository with
permission to create, update, and delete issues. The helper operates on the
repository selected by the GitHub CLI.

## Using the lab automation

The issue helper and seed data are in `.github/scripts/`. From the repository
root, run the helper from its own directory so it can find the seed JSON file:

```bash
cd .github/scripts
gh auth status
./prepare-lab-env.sh
```

With no argument, the helper creates the sample issues defined in
`issue-seeds.json`. `--seed-all` first deletes all issues it can list (up to
1,000), then creates the built-in Issue Basics example and the seeded examples.
`--delete` deletes all issues it can list (up to 1,000) without seeding new
ones. **These cleanup options are destructive; use only a disposable lab
repository.**

The `lab-issue-automation` workflow can be run manually with `--delete` or
`--seed-all`. It also runs when `.github/workflows/.run-once` is pushed to the
`main` branch; that file exists as a template-creation trigger, so changing it
can start the workflow again. The `issue-ping` workflow responds with `pong`
when a newly created issue comment contains the text `ping`.

## Development and checks

There is no application build or automated test suite in this repository. For
a local syntax/data check, run:

```bash
bash -n .github/scripts/prepare-lab-env.sh
jq empty .github/scripts/issue-seeds.json
```

Running the issue helper or workflow end to end changes issues in the selected
repository; use a disposable lab repository for that check.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the pull request process and
[CODE_OF_CONTENT.md](CODE_OF_CONTENT.md) for community expectations.

## License and support

This project is licensed under the [MIT License](LICENSE). For help, see
[SUPPORT,md](SUPPORT%2Cmd).
