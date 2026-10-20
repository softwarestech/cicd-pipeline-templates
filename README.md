# CI/CD pipeline templates

Starter pipelines for **GitHub Actions**, **GitLab CI** and **Jenkins**, maintained by [Softwares Tech](https://www.softwarestech.com).

Each file is a working starting point with sensible defaults: cached dependencies, least-privilege permissions, no long-lived cloud keys, and a manual approval before anything reaches production. Copy the one you need and adjust names, regions and branches to your project.

## What's inside

| File | Platform | What it does |
| --- | --- | --- |
| [`github-actions/node-ci.yml`](github-actions/node-ci.yml) | GitHub Actions | Install, lint, test and build a Node.js app on Node 20 and 22 |
| [`github-actions/docker-build-push.yml`](github-actions/docker-build-push.yml) | GitHub Actions | Build a Docker image with layer caching and push it to ghcr.io on `main` and version tags |
| [`github-actions/terraform-plan-apply.yml`](github-actions/terraform-plan-apply.yml) | GitHub Actions | `fmt`, `validate` and `plan` on pull requests; `apply` on `main` behind an approval, using AWS OIDC |
| [`gitlab-ci/.gitlab-ci.yml`](gitlab-ci/.gitlab-ci.yml) | GitLab CI | Test, build into the GitLab registry, manual deploy to Kubernetes |
| [`jenkins/Jenkinsfile`](jenkins/Jenkinsfile) | Jenkins | Declarative pipeline: test in a container, build and push, approve, deploy to Kubernetes |

## How to use

**GitHub Actions** — copy a file from `github-actions/` into `.github/workflows/` in your repository.

- `docker-build-push.yml` uses the built-in `GITHUB_TOKEN`; nothing else to set up.
- `terraform-plan-apply.yml` expects your Terraform code in `infra/`, a repository variable `AWS_ROLE_ARN` (an IAM role that trusts GitHub's OIDC provider), and an environment named `production` with required reviewers.

**GitLab CI** — copy `gitlab-ci/.gitlab-ci.yml` to your project root. The deploy job expects a [GitLab agent for Kubernetes](https://docs.gitlab.com/ee/user/clusters/agent/) and a `KUBE_CONTEXT` CI/CD variable.

**Jenkins** — copy `jenkins/Jenkinsfile` to your project root and create a Multibranch Pipeline job. Install the Docker Pipeline and Kubernetes CLI plugins, and add credentials named `registry-creds` and `kubeconfig-prod`.

## Design choices

- **Least privilege:** workflows declare only the permissions they need.
- **No stored cloud keys:** AWS access goes through OIDC role assumption.
- **Humans approve production:** deploys and `terraform apply` wait for a person.
- **Fast feedback:** dependency and Docker layer caches, and in-progress runs cancel when a newer commit lands.

## Validation

Every push runs [`validate.yml`](.github/workflows/validate.yml), which lints all YAML and checks the GitHub Actions workflows with [actionlint](https://github.com/rhysd/actionlint).

## Need help?

Softwares Tech sets up and maintains CI/CD, Kubernetes and cloud infrastructure on AWS and Azure. Write to **sales@softwarestech.com** or visit [softwarestech.com](https://www.softwarestech.com).

## License

[MIT](LICENSE)
