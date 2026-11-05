# payments consumer

[![CI](https://github.com/bhavyakeniya/team-payments/actions/workflows/infrastructure.yml/badge.svg?branch=main&event=push)](https://github.com/bhavyakeniya/team-payments/actions/workflows/infrastructure.yml)

Configurations live in `environments/dev`, `environments/staging` and
`environments/prod`. Edit the JSON declaration and expectations for that environment.
`team.json` supplies the stable team ID. Each JSON explicitly names its environment.

This project is checked against the
[platform repository](https://github.com/bhavyakeniya/terraform-platform-abstraction).
Either point `PLATFORM_DIR` at your checkout:

```bash
make check PLATFORM_DIR=/path/to/terraform-platform-abstraction
```

or clone the platform as a sibling using the directory name the default expects
(`git clone` would otherwise produce the hyphenated repository name):

```bash
git clone https://github.com/bhavyakeniya/terraform-platform-abstraction.git terraform_platform_abstraction
cd payments && make check
```

`make check` checks every environment without AWS. `make localstack` exercises
all three against the isolated free Hobby emulator, including S3 state/DynamoDB locks.
The platform supplies every Terraform file and all pipeline implementation.

`.github/workflows/infrastructure.yml` is active and calls the platform's
shared workflow at one pinned commit. Pushes and pull requests run `check`:
every environment is validated against the real platform module without AWS
credentials. `plan` and `apply` are deliberately manual — dispatch the workflow
and choose the mode. AWS deployment is not enabled, so a dispatched plan stops
at the platform's registration guard until a real account is configured.

Bumping to a newer platform release means changing the pinned SHA in that file;
the team never copies pipeline logic. See the platform's
[delivery guide](https://github.com/bhavyakeniya/terraform-platform-abstraction/blob/main/docs/environments-and-delivery.md)
for registration, state migration, approval and retirement.

Team configuration lives in environments/ exclusively; there are no duplicate
root declarations to accidentally edit.
