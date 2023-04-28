# Google Cloud SQL proxy buildpack

This heroku buildpack adds the Google Cloud SQL proxy to your app to enable
connections to SQL instances in the GCP.

## Prerequisites

- Google service account with the Cloud SQL client role
- JSON key for the service account
- Google Cloud SQL instance set up with a postgresql user able to connect
  to the database

## Install

Add the proxy to your buildpacks. It's important that this buildpack should
not be the last one in the list, as that's used by heroku to determine your
startup processes. (--index=1)

     heroku buildpacks:add --index=1 luisp128/cloud-sql-proxy-heroku

Add the GCP JSON credentials as `GCP_CREDENTIALS` env variable to you app.

Set the port the proxy should connect to with the `GCP_PORT` env
variable.

Add the sql instance connection name in the `GCP_INSTANCE_CONNECTION_NAME` env variable.

Set the connection string for your DB library to
`postgres://<username>:<password>@localhost:$GCP_PORT/<database-name>`

Start the proxy before your app tries to connect to the database by e.g. adding
`bin/run-cloud-sql-proxy` to the `.profile` in the root of your project.
