# rails-app-heroku-stack

[![Use this template](https://github.com/stack-instance/badge.svg)](https://github.com/stack-instance?stack_template_owner=gscho&stack_template_repo=rails-app-heroku-stack)

# Why should you use this stack?

This github stack will create a new rails application along with a test and deploy github workflow for deploying to an existing heroku account. It also configures a repository with branch protection rules, security settings and an environment.

- [Prerequisites](#prerequisites)
- [Stack Inputs](#stack-inputs)
- [What This Stack Does in Detail](#what-this-stack-does-in-detail) 
  - [Repo Creation](#repo-creation)
  - [Branch Protection Rules](#branch-protection-rules)
  - [Environments](#environments)
  - [Security](#security)
  - [Stack Init Workflow](#stack-init-workflow)
  - [CD Workflow](#cd-workflow)
      - [Test Job](#test-job)
      - [Deploy Job](#deploy-job)

# Prerequisites

- A heroku account
- The email address used to login to heroku
- Your heroku API token

# Stack Inputs

- `RUBY_VERSION`: The desired version of ruby
- `RAILS_VERSION`: The desired version of rails
- `RAILS_RC_URL`: An optional URL to fetch a .railsrc file from. Eg: https://raw.githubusercontent.com/gscho/rails-app-heroku-stack/main/.railsrc
- `HEROKU_EMAIL`: Your heroku email
- `HEROKU_API_TOKEN`: Your heroku API token
- `HEROKU_APP_NAME`: An optional heroku app name

# What This Stack Does in Detail

## Repo Creation

This stack will configure a new github repository with the `HEROKU_API_TOKEN` and `HEROKU_EMAIL` set at repository secrets. It also sets the topic to "rails".

## Branch Protection Rules

The branch protection rules are configured to be sane defaults, requiring at least 1 review from code owners before merging and dismissing stale reviews.

## Environments

The default environment created is "heroku" and only protected branches can deploy to this environment.

## Security

Dependabot scanning is enabled by default.

## Stack Init Workflow

This workflow generates a new rails app from a .railsrc file. It also creates a new heroku app using the provided credentials. Once that's done it opens a pull-request on your new repository (since main is a protected branch) for you to review and approve.

## CD Workflow

### Test Job

The "test" job uses a postgresql service container and configures the rails application to use it for testing. It then sets up the database, webpacker and runs tests.

### Deploy Job

**Note:** *The deploy job only runs on pushes to the main branch and not pull-requests*. 

The deployment job reads the `.heroku-app-name` file that gets created in the `stack-init.yml` workflow and sets up a git remote to point to the existing heroku app. It then migrates the database and deploys to the heroku remote. If you rename the heroku app, you will need to also change the `.heroku-app-name` file.

## Contributors

- Greg Schofield ([@gscho](https://github.com/gscho))
