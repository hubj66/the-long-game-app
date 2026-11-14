# Installation

This guide installs The Long Game as a Home Assistant app. You need no Git,
no Python and no computer setup: everything happens in Home Assistant. It takes
about 30 minutes.

> **Status: early test release, not ready for family use yet.** The Long Game
> is in private development. Use it with pretend money only. Home Assistant
> marks the app as *experimental* and shows a warning before you install it.

| Step | What you do | Where |
|---|---|---|
| 1 | Install PostgreSQL and pgAdmin | Home Assistant app store |
| 2 | Create a database user and a database | pgAdmin |
| 3 | Install The Long Game | Home Assistant app store |
| 4 | Enter the database URL and start | The Long Game app page |
| 5 | Create the parent account | The Long Game |
| 6 | Add your child and the first year | The Long Game |

## What you need

- Home Assistant OS or Supervised, on `aarch64` (for example Home Assistant
  Yellow) or `amd64`.
- A Home Assistant backup made before you start.
- A password manager for the passwords and the database URL you create.

## 1. Install PostgreSQL and pgAdmin

You can skip this step if you already run PostgreSQL 15 or later.

1. In Home Assistant open **Settings > Apps > App store**, open the menu (⋮, top
   right) and choose **Repositories**.
2. Add `https://github.com/alexbelgium/hassio-addons` and close the dialog.
3. Install **Postgres 17**. On its **Configuration** tab set a strong
   `POSTGRES_PASSWORD`, save, and start the app. This is the administrator
   password; keep it in your password manager.
4. Install **pgAdmin4** from the same repository, start it and turn on
   **Show in sidebar**.

## 2. Create the database user and the database

### Connect pgAdmin to PostgreSQL

The first time only:

1. Open **pgAdmin4** from the sidebar.
2. Right-click **Servers** and choose **Register > Server**.
3. **General** tab: Name `Home Assistant`.
4. **Connection** tab:

   | Field | Value |
   |---|---|
   | Host name/address | `db21ed7f-postgres-latest` |
   | Port | `5432` |
   | Maintenance database | `postgres` |
   | Username | `postgres` |
   | Password | the `POSTGRES_PASSWORD` from step 1 |
   | Save password? | on |

5. Choose **Save**. The server appears under **Servers**.

### Create the user and the database

1. Open **Servers > Home Assistant > Databases**, right-click the database
   **postgres** and choose **Query Tool**.
2. Paste the following, replace `ChooseAStrongPassword1` with your own password
   (letters and numbers only, so the URL needs no special characters), and
   press **F5**:

   ```sql
   CREATE ROLE tlg_user WITH LOGIN PASSWORD 'ChooseAStrongPassword1';
   CREATE DATABASE the_long_game OWNER tlg_user;
   ```

3. The answer should be `CREATE DATABASE`.

The Long Game creates and updates its own tables when it starts. Do not share
this database with another application.

### Build the database URL

```text
postgresql://USER:PASSWORD@HOST:5432/DATABASE
```

| Part | Value |
|---|---|
| `USER` | `tlg_user` |
| `PASSWORD` | the password from the step above |
| `HOST` | the host name of your PostgreSQL app. For **Postgres 17** (alexbelgium) it is `db21ed7f-postgres-latest`; other apps show theirs on their **Info** page. |
| `DATABASE` | `the_long_game` |

Example:

```text
postgresql://tlg_user:ChooseAStrongPassword1@db21ed7f-postgres-latest:5432/the_long_game
```

**Keep this URL in your password manager.** Home Assistant deletes the app's
settings when the app is uninstalled; the data in PostgreSQL stays.

## 3. Install The Long Game

1. In Home Assistant open **Settings > Apps > App store**, open the menu (⋮)
   and choose **Repositories**.
2. Add `https://github.com/hubj66/the-long-game-app` and close the dialog.
3. Find **The Long Game** in the store. If it does not appear, open the menu
   and choose **Check for updates**.
4. Choose **Install**. Home Assistant warns that the app is *experimental*;
   confirm to continue. The download takes a few minutes.
5. On the app's **Info** tab turn on **Show in sidebar**.

## 4. Enter the database URL and start

1. Open the app's **Configuration** tab.
2. Paste your URL into `database_url`. Leave every other option as it is.
3. Choose **Save**, go to the **Info** tab and choose **Start**.
4. Open the **Log** tab. After about half a minute the last lines are:

   ```text
   INFO __main__: Starting ASGI server on port 8099
   INFO:     Application startup complete.
   ```

If the log shows a line starting with `Cannot start:` instead, it names the
cause: a wrong user or password, a database that does not exist, an unknown
host or a database server that cannot be reached. Correct `database_url`, save
and start again. The log never shows the URL itself.

## 5. Create the parent account

1. Open **The Long Game** from the sidebar.
2. Choose **Create a parent account**. Signing up is only possible while no
   parent account exists.
3. Choose a short sign-in name (for example `alex`) and a password, and keep both
   in your password manager.

## 6. Set up inside The Long Game

Everything else happens in the app:

1. **Manage** page: under **Add child** enter your child's first name or a
   nickname and the age group, then choose **Add child**. The child's card shows
   the sign-in name the app made from it (for example `lena`). Set the child's
   password on the same card.
2. **Library**: add the starter library (synthetic investments and price
   histories), or import your own histories.
3. **Invest > Yearly cycles**: create the first year. You can start from the
   example plan. Choose the start date, the period and the investments, then
   publish the year.
4. Your child opens The Long Game, chooses **Child**, and signs in with the
   sign-in name and password. Then the child works through the chapters and
   opens **My year**.

See [yearly cycles](yearly-cycles.md) and [tool ladder](tool-ladder.md) for
how a year and the unlocking of tools work.

### Check that everything works

- [ ] The app's **Info** tab shows it as running.
- [ ] You can sign in as the parent.
- [ ] **Manage** lists your child.
- [ ] **Invest > Yearly cycles** shows the first year as published.
- [ ] Your child can sign in and sees **My year** and **My chapters**.
- [ ] The database URL, your sign-in name and both passwords are in your password manager.

## Troubleshooting

### I lost the database URL

The host and port are known (see [Build the database URL](#build-the-database-url)).
Find the rest in pgAdmin, in the Query Tool of the database **postgres**:

```sql
SELECT datname AS database, pg_get_userbyid(datdba) AS owner
FROM pg_database
WHERE NOT datistemplate
ORDER BY 1;
```

The Long Game's database is the one whose Query Tool answers
`SELECT version_num FROM alembic_version;` with a revision such as
`0019_child_display_name`. Its owner is the user. A password cannot be read back;
set a new one for that user (this changes nothing else if only The Long Game
uses the user):

```sql
ALTER ROLE tlg_user WITH PASSWORD 'ANewStrongPassword2';
```

Never change the password of the user `postgres` if other apps such as
Firefly III use it.

### The sign-in page offers "Create a parent account"

Signing up is only offered while the database has no parent account. If you
already had one, the URL points to a new or empty database: stop the app and
check the database name in the URL. Do not create a second account.

### I forgot the parent sign-in name or password

**Sign-in name.** In pgAdmin, in the Query Tool of The Long Game's database:

```sql
SELECT principal_id FROM application_parent_credentials;
```

The sign-in name is the part after `principal_` (`principal_alex` signs in as
`alex`).

**Password.** Only a Home Assistant administrator can do this:

1. Open **Settings > Apps > The Long Game > Configuration**.
2. Turn on **Show unused optional configuration options** if the two reset
   options are hidden.
3. Enter the sign-in name in `reset_parent_sign_in_name` and a new password
   (at least 8 characters) in `reset_parent_password`. Choose **Save**.
4. Restart the app. The log shows `The parent password was reset.` The
   password itself is never shown in the log.
5. **Remove both reset options again and save.** While they are set, every
   start resets the password again.
6. Sign in with the new password.

Children's passwords are reset by a parent on the **Manage** page.

## Privacy and safety

- The app is reached through Home Assistant ingress. Do not expose its port to
  the internet.
- Keep the database URL and all passwords out of YAML files, issues and logs.
- The money in a yearly cycle is pretend money.

## For developers: install from a local bundle

Use this only to test your own build before it is released.

1. On a computer with the repository checked out, run
   `python scripts/stage_ha_app.py`. This writes `dist/ha-app-bundle/`.
2. Copy the content of that folder to `/addons/the_long_game/` on the Home
   Assistant device (for example with the Samba or SSH app).
3. In Home Assistant open **Settings > Apps > App store**, open the menu and
   choose **Check for updates**.
4. Install **The Long Game** from **Local apps**. Home Assistant builds the
   image on the device; this takes a few minutes.

The local app and the app from the repository are two different apps: never run
both against the same database.

## Next

- [Upgrading](upgrading.md): update, back up and roll back.
- [PostgreSQL migrations](postgresql-migrations.md) and
  [PostgreSQL recovery](postgresql-recovery.md) for database details.
