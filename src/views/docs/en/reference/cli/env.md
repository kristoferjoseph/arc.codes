---
title: env
description: Architect env is a module to read and write environment variables to AWS SSM Parameter store
sections:
  - Overview
  - Usage
  - Flags
---

## Overview

[Architect env](https://github.com/architect/env) allows you to read & write environment variables that are immediately made available to all deployed Lambda functions. An environment variable is a variable whose value is set outside the program. Sensitive configuration data, such as API keys, needs to happen outside of the codebase in revision control. You can use this tool to ensure an entire team, and the deployment targets are in sync.

> Currently `app.arc` uses AWS Systems Manager Parameter Store as a centralized backing storage mechanism for app environment variables. [Read more about AWS Systems Manager Parameter Store.](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html)

### The `.arc-env` File

Create a file called `.arc-env` in the root of your project to load environment variables locally in the sandbox. This file **should not** be committed into your project git repository. Always add the `.env` file extension to your `.gitignore` file.

- It's generated by adding or removing env vars using `arc env`.
- Automatically read by `arc-sandbox` and populates `process.env` when `arc sandbox` runs locally.
- Lists env vars for `testing` or `staging` for local development.

## Set environment variables

```bash
# This is an example `.arc-env` file:
@testing 
FOO 1

@staging
FOO 2

@production
FOO 3
```

## Usage

- `arc env` displays environment variables for the current `app.arc`
- `arc env staging FOOBAZ somevalue` writes env variable `FOOBAZ=somevalue` to staging Lambda functions
- `arc env production FOOBAR othervalue` writes env variable `FOOBAR=othervalue` to production Lambda functions
- `arc env remove staging FOOBAZ` removes env variable `FOOBAZ=somevalue` from the staging Lambda functions
- `arc env remove production FOOBAR` removes env variable `FOOBAR=othervalue` from the production Lambda functions

> Things to note: `NODE_ENV`, `ARC_APP_NAME` and `SESSION_TABLE_NAME` are reserved names.
> Values that contain special characters, like e-mail addresses should be wrapped in double quotes. 
> If no `.arc-env` exists, this module will create one and populate it from the terminal options