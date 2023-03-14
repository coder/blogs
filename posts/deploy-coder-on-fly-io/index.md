# Deploy Coder on Fly.io

Fly.io is a platform for deploying applications to the edge. It's a great fit for Coder because it's easy to deploy and scale, and it's easy to manage. In this guide, we'll walk through deploying Coder on Fly.io and creating workspaces as Fly.io (firecracker) machines.

## Prerequisites

- A Fly.io [account](https://fly.io/signup).
- [Fly CLI](https://fly.io/docs/getting-started/installing-flyctl/) installed on your machine.

## Deploy Coder

1. Log in to Fly.io with the CLI:

```bash
flyctl auth login
```

2. Create a new fly postgres database:

```bash
flyctl postgres create DATABASE-NAME
```

3. Create a new fly app:

```bash
flyctl apps create APP-NAME
```

4. Connect to the database with the coder fly app:

```bash
flyctl postgres connect DATABASE-NAME --app APP-NAME
```

This will print out a connection string. Copy the connection string and save it to be used in the next step.

5. Edit the `fly.toml` file and update as per the example below:

```toml
app = "APP-NAME"
kill_signal = "SIGINT"
kill_timeout = 5
primary_region = "ams"  # See a list of regions here: https://fly.io/docs/reference/regions/

[experimental]
  auto_rollback = true
  private_network = true  # Allows Coder to connect to the database

[build]
   image = "ghcr.io/coder/coder:latest"

[env]
  CODER_ACCESS_URL = "https://APP-NAME.fly.dev" # make sure to replace APP-NAME with your app name
  CODER_HTTP_ADDRESS = "0.0.0.0:3000"
  CODER_PG_CONNECTION_URL = "ADD YOUR POSTGRES CONNECTION STRING HERE"
  #CODER_VERBOSE = "true" # Uncomment this if you want to see more logs
  CODER_TELEMETRY_INSTALL_SOURCE = "fly.io"

[[services]]
  protocol = "tcp"
  internal_port = 3000
  processes = ["app"]

  [[services.ports]]
    port = 80
    handlers = ["http"]
    force_https = true

  [[services.ports]]
    port = 443
    handlers = ["tls", "http"]
  [services.concurrency]
    type = "connections"
    hard_limit = 25

```

6. Deploy the app:

Run the following command to deploy the app from the directory where the `fly.toml` file is located:

```bash
flyctl deploy
```

7. Scale the to use more memory:

```bash
flyctl scale memory 1024 --app APP-NAME
```

8. Go to the URL of your app and start using Coder!

> If you want to use a custom domain, you can do so by following the instructions [here](https://fly.io/docs/getting-started/custom-domains/).

## Update Coder

To update the Coder version, run `flyctl deploy --aap APP-NAME` again and it will pull the latest version of Coder.

Congratulations! You've deployed Coder on Fly.io!

## Next Steps

> To write your first coder template, check out the [template docs](https://coder.com/docs/v2/latest/templates).
> We have an official [fly.io template](https://github.com/coder/coder/main/examples/templates/fly-docker-image) that you can use as a starting point.
