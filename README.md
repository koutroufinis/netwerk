# Netwerk

Netwerk is a Flask-based direct messaging app.

## Deploy to Render

The repository includes a Render Blueprint in `render.yaml`. In Render, create a
new Blueprint and connect the `koutroufinis/netwerk` repository. The Blueprint
installs the Python requirements, runs Flask with Gunicorn, checks `/health`,
and attaches a persistent disk at `/var/data` for the SQLite database and JSON
data files.

The web service uses the `starter` plan because Render persistent disks require
a paid service plan. The app is configured to use one Gunicorn worker because
SQLite and the local JSON snapshots are not designed for multiple service
instances. To scale beyond one instance, move application data to a shared
database and storage service.

Set these environment variables when Render prompts for them:

- `SERVICE`: the sender email address used for verification emails.
- `APP_PASSWORD`: the sender's SMTP app password. Verification email currently
  uses Gmail SMTP.

The Blueprint generates `FLASK_SECRET_KEY` and configures HTTPS-only session
cookies. Keep the generated secret stable between deploys so existing sessions
remain valid. Do not commit mail credentials or production secrets.

For local development, install `requirements.txt` and run `python server.py`.
