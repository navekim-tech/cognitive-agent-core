# Action Classification

## Purpose

This file defines how requested actions are classified before policy decisions are made.

Action classification must happen before execution.

The classification result is used by the policy engine to determine risk, approval requirements, allowed scope, and audit behavior.

## Input Fields

### action_type

The requested operation category.

Examples:

- `read`
- `inspect`
- `create_file`
- `edit_file`
- `delete`
- `move`
- `rename`
- `commit`
- `push`
- `pull`
- `merge`
- `rebase`
- `checkout`
- `reset`
- `deploy`
- `publish`
- `change_permissions`
- `access_credentials`
- `database_migration`
- `network_request`
- `external_api_call`

### target

The object affected by the action.

Examples:

- file
- directory
- repository
- server
- production environment
- database
- user account
- external service

### environment

The execution context.

Allowed values:

- `local`
- `repo`
- `server`
- `production`
- `external`

### source

The origin of the requested action.

Allowed values:

- `user`
- `agent`
- `tool`
- `memory`
- `external_api`

## Action Categories

### Read-Only Actions

Actions that inspect state without changing anything.

Examples:

- list files
- read file
- inspect Git status
- inspect logs
- inspect configuration
- search repository content

Default risk: `low`

Default decision: `allow`, if target and environment are clear.

### Local Write Actions

Actions that modify local or repository files but do not publish externally.

Examples:

- create file
- edit file
- rename file
- move file

Default risk: `medium`

Default decision: `require_approval` if scope is broad or target is unclear.

### Destructive Actions

Actions that remove, overwrite, reset, or make recovery difficult.

Examples:

- delete file
- delete directory
- reset Git state
- clean untracked files
- overwrite configuration
- remove dependency
- drop database table

Default risk: `high` or `critical`

Default decision: `require_approval` or `block`.

### Git State-Changing Actions

Actions that alter repository history, branch state, remote state, or working tree state.

Examples:

- `git add`
- `git commit`
- `git push`
- `git pull`
- `git merge`
- `git rebase`
- `git checkout`
- `git reset`
- `git clean`
- branch creation
- branch deletion

Default risk: `medium` to `high`

Default decision: `require_approval`.

### External Side-Effect Actions

Actions that affect systems outside the local repository.

Examples:

- deploy
- publish
- send email
- call external API
- modify remote service
- update production configuration
- trigger automation
- create public artifact

Default risk: `high`

Default decision: `require_approval`.

### Credential and Permission Actions

Actions involving secrets, authentication, authorization, or access control.

Examples:

- read credentials
- modify `.env`
- rotate keys
- change permissions
- change roles
- expose tokens
- connect external account

Default risk: `critical`

Default decision: `block` unless explicit approval and safe scope exist.

### Database Actions

Actions affecting stored data or schema.

Examples:

- run migration
- alter table
- drop table
- delete records
- update records
- seed production data

Default risk: `high` or `critical`

Default decision: `require_approval` or `block`.

## Classification Rule

When an action matches multiple categories, classify it by the highest risk category.

Example:

Editing a production credential file is both a write action and a credential action.

Final classification: `critical`.
