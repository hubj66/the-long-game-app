# Installation

This guide installs The Long Game as a Home Assistant app. You do not need Git
or a development environment for the first way described here.

> **Status: early test release, not ready for family use yet.** The Long Game
> is in private development. Use it with pretend money only. Installing from the app
> repository (way A) works from the first public release; it has not been
> validated on a real Home Assistant yet. Until then, use the local bundle
> (way B), which is how the app is installed and tested today.

## What you need

- Home Assistant OS or Supervised, on `aarch64` (for example Home Assistant
  Yellow) or `amd64`.
- A PostgreSQL database, version 15 or later, that Home Assistant can reach.
- A Home Assistant backup made before you start.

## 1. Install PostgreSQL

Install a PostgreSQL app in Home Assistant (there are community apps), or use
a PostgreSQL server you already run. Then create:

- an empty database, for example `the_long_game`;
- a user with a strong password that owns this database.

The Long Game creates and updates its own tables when it starts. Do not share
the database with another application.

Write down the database URL. It has this form:

```text
postgresql://USER:PASSWORD@HOST:5432/DATABASE
```

`HOST` is the host name of the PostgreSQL app as shown on its information page,
or the address of your own server. If the password contains characters such as
`@`, `:` or `/`, they must be percent-encoded.

## 2. Install The Long Game

### Way A: from the app repository

1. In Home Assistant open **Settings > Apps > App store**.
2. Open the menu (top right), choose **Repositories** and add the address of
   The Long Game app repository:

   ```text
   https://github.com/hubj66/the-long-game-app
   ```

3. Close the dialog, find **The Long Game** in the store and choose **Install**.

Home Assistant downloads a ready-made image. Nothing is built on your device.

### Way B: as a local app from a bundle

Use this before the first public release, or to run your own build.

1. On a computer with the repository checked out, run
   `python scripts/stage_ha_app.py`. This writes `dist/ha-app-bundle/`.
2. Copy the content of that folder to `/addons/the_long_game/` on the Home
   Assistant device (for example with the Samba or SSH app).
3. In Home Assistant open **Settings > Apps > App store**, open the menu and
   choose **Check for updates**.
4. Install **The Long Game** from **Local apps**. Home Assistant builds the
   image on the device; this takes a few minutes.

## 3. Enter the database URL

Open the app's **Configuration** tab.

| Option | What to enter |
|---|---|
| `database_url` | The database URL from step 1. It is stored as a password and is not shown in logs. |
| `log_level` | Leave `info`. |
| `workers` | Leave `1`. |
| `enable_demo_financial` | Leave off for family use. It adds a synthetic demo portfolio. |
| `skip_migrations` | Leave off. The app then updates its database tables at start. |
| `factory_module` | Leave empty. |

Save the configuration.

## 4. Start and create the parent account

1. Start the app and wait until the log shows that the server is running.
2. Choose **Open web UI**, or use **The Long Game** in the sidebar.
3. On the sign-in page choose **Create a parent account** and create the first
   parent. Signing up is only possible while no parent account exists.

If the page does not open, read the app's **Log** tab first. A wrong database
URL is the most common cause.

## 5. Set up inside The Long Game

Everything else happens in the app:

1. **Manage** page: add your child and set the child's password.
2. **Library**: add the starter library (synthetic investments and price
   histories), or import your own histories.
3. **Invest > Yearly cycles**: create the first year. You can start from the
   example plan. Choose the start date, the period and the investments, then
   publish the year.
4. The child signs in on the child sign-in page, works through the chapters and
   opens **My year**.

See [yearly cycles](yearly-cycles.md) and [tool ladder](tool-ladder.md) for
how a year and the unlocking of tools work.

## Privacy and safety

- The app is reached through Home Assistant ingress. Do not expose its port to
  the internet.
- Keep the database URL and all passwords out of YAML files, issues and logs.
- The money in a yearly cycle is pretend money.

## Next

- [Upgrading](upgrading.md): update, back up and roll back.
- [PostgreSQL migrations](postgresql-migrations.md) and
  [PostgreSQL recovery](postgresql-recovery.md) for database details.
