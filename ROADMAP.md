- [ ] More rules:
    * [X] No columns
    * [X] One column
    * [ ] No surrogate PK
    * [X] PK not first
    * [X] Too many columns
        + [ ] Also for views
    * [ ] Huge number of procedure args
    * [ ] Attribute-less join tables with a surrogate key
    * [X] All tables have a primary key
    * [X] No PG rules
    * [X] No PG inheritance
    * [X] `DEFAULT now()`
    * [X] `DEFAULT CURRENT_TIME`
    * [X] `SERIAL`
    * [X] Non-generated primary key, even if UUID
    * [X] `password_encryption` `scram-sha-256`
    * [X] Inconsistent PK types across the whole schema or database
    * [ ] Other uncommented DB objects:
        + [X] Tables and views
        + [X] Schemas
        + [ ] The current database
        + [X] Functions, procedures (yes, like a docstring)
            - [ ] But not ones from extensions.
        + [ ] The other types at the notice level.
    * [ ] Duplicate relationships
    * [X] Disconnected table
        + [ ] Except from extensions, e.g., `pg_stat_statements`
    * [ ] Unused UDT
        + [ ] Also enums, which aren't under `user_defined_types`.
    * [ ] `column1`, `column2` or `spouse1_id`, `spouse2_id`
    * [ ] Too many large objects
    * [ ] Names that require quoting
        + [ ] Databases
        + [X] Schemas
        + [X] Tables
        + [X] Columns
        + [X] Procedures
        + [ ] Types
        + [ ] Everything else, roles, constraints, domains, etc., everything in
              `information_schema`, `pg_catalog`, `pg_class`.
    * [X] All, all minus PK, or too many nullable
        + [ ] Except from extensions
    * [ ] Table cycles
    * [ ] Table cycles, recursively
    * [X] Self-referencing
    * [ ] Inherently redundant indexes, of the same index type
    * [X] Unindexed tables
    * [ ] All PK (NOTICE)
    * [X] Not `bigint` PK
    * [ ] Column Tetris
    * [ ] Redundant index when composite prefix will do
    * [ ] Run views, functions/procedures, defaults, and generated through a query linter.
    * [X] Deprecated extensions
        + [X] `uuid-ossp` (debatable), just use `gen_random_uuid()` unless you really need not completely random UUIDs at your own peril)
        + [X] (lib)`xml2`, per v14 F.45. xml 2 § F.45.1 Deprecation Notice, use SQL standard instead
        + [X] `dblink`, use postgres_fdw instead
        + [X] `hstore`?
        + [X] `intagg`, v14 F.17, “The intagg module provides an integer aggregator and an enumerator. intagg is now obsolete, because there are built-in functions that provide a superset of its capabilities. However, the module is still provided as a compatibility wrapper around the built-in functions.”
    * [X] Forbidden extensions
        + [X] `plpython2`
        + [X] `pltcl`/`pltclu`
        + [X] `file_fdw`, Don't access files, especially in a cloud where it's probably not available and you have yet another way of getting onto a box we wanted to lock down
        + [X] `adminpack`, same reason as file_fdw
        + [X] `test_decoding`, it's an examply only
    * [ ] warn on routine language
    * [X] error on routine language
    * [X] `plpythonu` instead of `plpython3u` explicitly
    * [X] Non-IMRSV languages
    * false sense of security `pg_hba_file_rules`
        + [ ] address ≠ all or 127.0.0.1 or ::1
        + [ ] netmask not null or 255.255.255.255 or ffff:…:ffff
        + [X] auth-method md5, password, ident
        + [X] separately warn about pam, bsd, cert for cloud or k8s
        + [X] sspi, why are you running on Windows, what a terrible idea
        + [X] auth-method peer not used on UNIX domain socket
        + [ ] a lot of rules, arbitrary number, may be easy to make mistakes
    * [ ] Unenforced constraints?
    * [ ] Disabled triggers?
    * [ ] `ON UPDATE` on surrogate key
    * [ ] Owned by `postgres`
        + [ ] If possible runtime owner by role other than migration role
    * [ ] `SECURITY DEFINER`
    * [ ] Type warnings and errors per non-column objects:
        + [ ] procedure args
        + [ ] procedure return values
        + [ ] Type subclasses
        + [ ] Type conversions
        + [ ] Operators
    * [ ] Too many triggers to be tractable
    * [ ] For each row instead of for each statement trigger
    * [ ] Groups with `LOGIN`
    * [ ] Roles with `LOGIN`
    * [ ] Groups instead of roles
    * [ ] `?column?`s
    * [ ] Anything relevant in the
      [specific](https://www.stigviewer.com/stig/postgresql_9.x/) or
      [generic](https://www.stigviewer.com/stig/database_security_requirements_guide/)
      DISA STIGs.
    * [ ] CIS benchmark or just recommend [Puppet/`pg_secured`][pg_secured] or
      [EasyAppSecurity/postgres-baseline][postgres-baseline]?
    * [ ] Missing security-sensitive [§ 33.1.2 Parameter Key
      Words][paramkeywords] or [env vars][pgenv] in current connection string.
        * [ ] `host` if `hostaddr` and should-TLS transport
        * [ ] `channel_binding=require`, not default `prefer`
        * [ ] No `tty`, ignored, meaningless
        * [ ] `gssencmode=require` if set
        * [ ] `sslmode` not `verify-full`
        * [ ] No `requiressl`, deprecated
        * [ ] Not `sslcompression`
        * [ ] Permissions of `sslcert`
        * [ ] Permissions of `sslkey`
        * [ ] Permissions of `passfile`
        * [ ] `passfile` unsupported for `zxcvbn`
        * [ ] Weak password for `sslpassword` per zxcvbn
        * [ ] No `sslrootcert`
        * [ ] No `sslcrl`, perhaps `NOTICE` only to start.
        * [ ] `requirepeer=postgres` if UNIX domain socket
        * [ ] `ssl_min_protocol_version=TLSv1.3`, and notice on 1.2, error on 1.1 or lesser
        * [ ] Warning `ssl_max_protocol_version`, error if ≤1.1
        * [ ] `service` unsupported
        * [ ] `sslsni=1`
    * [ ] ITSP 40.062 crypto.
- [ ] Set additional [libpq connection options][paramkeywords]:
    * `application_name=imrsv-schema-linter`
    * `target_session_attrs=read-only` if possible?
- [ ] User config files.
- [ ] Generic schema/table/column filtering.
- [ ] Properly handle schemas instead of assuming `public`.
- [ ] Output format `sql` to pipe into `psql` or whatnot.
- [ ] JSON and CSV output formats.
- [ ] Reviewdog snippet.
- [ ] Replace mediocre `print` format with nicer, aligned tables.
- [ ] PEP 621 packaging, for your other favourite tools.
- [ ] Unit tests… oh wait, that's not a feature but expected!
- [ ] Rename rules.
- [ ] Docker image, unfortunately.
    * No, never Flatpak, AppImage, or Snap.
- [ ] Keep a changelog.
- [ ] Man page.
- [ ] Basic README. Just RTF-`--help` in try it in the meantime.
- [ ] Rule filtering and selection based on rule attributes or a glob.
- [ ] logfmt all logs, not just rule violations.
    * [ ] Also JSON format.
- [ ] Override OpenSSL defaults with more secure ones in image, by default?

[pg_secured]: https://forge.puppet.com/modules/enterprisemodules/pg_secured
[postgres-baseline]: https://github.com/EasyAppSecurity/postgres-baseline
[paramkeywords]: https://www.postgresql.org/docs/13/libpq-connect.html#LIBPQ-PARAMKEYWORDS "33.1.2. Parameter Key Words"
[pgenv]: https://www.postgresql.org/docs/current/libpq-envars.html
