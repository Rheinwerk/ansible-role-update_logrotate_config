Configure Logrotate Jobs
=========


[![Build Status](https://github.com/Rheinwerk/ansible-role-update_logrotate_config/actions/workflows/ci.yml/badge.svg)](https://github.com/Rheinwerk/ansible-role-update_logrotate_config/actions/workflows/ci.yml)

Requirements
------------

None.


Role Variables
--------------

Every setting is one variable with a default in `defaults/main.yml`; set only the ones that
differ. The defaults are the CenterDevice values.

| Variable | Default |
|---|---|
| `_logrotate_default_definitions` | `database_shell`: daily rotation of `/home/cd-admin/.mysql_history` and `/root/.mysql_history` |
| `_logrotate_definitions` | `[]` |

The role writes one file `/etc/logrotate.d/<name>` per entry of both lists (default ones first).
Entry keys: `name` (file name), `logs` (paths or globs), `options` (logrotate directives, one per
line) and the optional script lists `postrotate`, `preremove`, `lastaction` and `firstaction`.

Until v0.3.0 the role took one `_logrotate` dict with the keys `default_definitions` and
`definitions` instead.

Dependencies
------------

None.


Example Playbook
----------------

A host that keeps the default definitions and adds one:

    - hosts: servers
      vars:
        _logrotate_definitions:
          - name: "pgbouncer"
            logs:
              - "/var/log/pgbouncer/pgbouncer.log"
            options:
              - "daily"
              - "rotate 30"
              - "missingok"
            postrotate:
              - "/bin/kill -HUP `cat /var/run/pgbouncer/pgbouncer.pid 2>/dev/null` 2>/dev/null || true"
      roles:
         - { role: update_logrotate_config, tags: [ 'logrotate' ] }

License
-------

Please see LICENSE.

Author Information
------------------

Original author is [Daniel Schneller](https://github.com/dschneller) as member of the [Rheinwerk](https://github.com/Rheinwerk) project.

