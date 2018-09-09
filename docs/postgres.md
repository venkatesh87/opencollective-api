# Database

You need to have Postgres 9.x or 10.x with the Postgis extension. More specifically:
 - 9.3.x: 9.3.22 or newer
 - 9.4.x: 9.4.17 or newer
 - 9.5.x: 9.5.12 or newer
 - 9.6.x: 9.6.8 or newer
 - 10.x: 10.3 or newer

## Installation

### On macOS

Last time we checked, the 2 following options were working:
 - Postgres 9.x or 10.x through [Postgres.app](http://postgresapp.com/).
 - Postgres 10.x through Homebrew: `brew install postgresql postgis`

## Database initialization

### For development:

Everything should be normally taken care of by `npm install`.

If something goes wrong, drop the `opencollective_dvl` database and the `opencollective`

```
dropdb opencollective_dvl
dropuser opencollective
```

Then, starts again with `npm install`

Notes:

- under the hood `npm install` is ultimately triggering `./scripts/db_restore.sh -d opencollective_dvl -f test/dbdumps/opencollective_dvl.pgsql`
- an alternative version is also available (`npm run db:setup && npx babel-node ./scripts/db_restore.js opencollective_dvl`) but we don't recommend to use it in dev environment at the moment

## Troubleshooting

For development, ensure that local connections do not require a password. Locate your `pg_hba.conf` file by running `SHOW hba_file;` from the psql prompt (`sudo -i -u postgres` + `psql` after clean install). This should look something like `/etc/postgresql/9.5/main/pg_hba.conf`. We'll call the parent directory of `pg_hba.conf` the `$POSTGRES_DATADIR`. `cd` to `$POSTGRES_DATADIR`, and edit `pg_hba.conf` to `trust` local socket connections and local IP connections. Restart `postgres` - on Mac OS X, there may be restart scripts already in place with `brew`, if not use `pg_ctl -D $POSTGRES_DATADIR restart`.

## FAQ

### error: type "geometry" does not exist

Make sure Postgis is available and activated.
