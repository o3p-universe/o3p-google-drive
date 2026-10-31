# Agent guidance

## Local deployment

These notes describe this development environment. Keep machine-specific
deployment instructions out of public README files.

Deployment helpers operate on real services and may commit, push, restart Odoo,
or install/upgrade modules. Run them only when the user explicitly requests
the corresponding action; never use them for validation.

The deployment scripts target the same Odoo 20 instance as the Persona addon:

- `./restart_odoo20.sh` pulls or bootstraps the deployment checkout, links the
  addon into `/opt/odoo20/addons`, clears Python caches, and restarts Odoo.
- `./upush.sh "commit message"` commits and pushes the current branch, then
  optionally runs the restart script.
- `./fulldeploy.sh "commit message"` commits, pushes, deploys, and automatically
  installs or upgrades `o3p_google_drive` on `persona.standalone.io`.

Paths, service names, database names, and users can be overridden through the
environment variables declared at the top of each script.
