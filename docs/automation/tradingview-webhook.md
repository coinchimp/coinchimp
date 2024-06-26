[:fontawesome-brands-github: App Github Repo](https://github.com/coinchimp/whistle)

## Overview
Welcome to the TradingView Webhook for Discord! This tool allows you to set up a service on Google Cloud Run that captures TradingView alerts and posts them directly to your Discord server through a webhook. It's perfect for keeping your trading team updated with the latest alerts in real-time!

## Getting Started
Before you dive in, make sure you have:

* A TradingView account ([Sign up here](https://www.tradingview.com/))
* Admin rights on your Discord server
* A Discord webhook URL set up in your server (you'll use this in the steps below)
* Google Cloud credentials: you'll need a JSON file for Google Cloud Run access (GCP_RUN_CREDENTIALS) and your Google project ID (GOOGLE_PROJECT_ID)

### Setup Guide
1. **Fork this repository** to get started with your own version.
2. **Add your Google Cloud and Discord secrets** to your repo's action secrets:
   - `GCP_RUN_CREDENTIALS`: your Google Cloud credentials file.
   - `GOOGLE_PROJECT_ID`: your project ID on Google Cloud.
   - `WEBFHOOK_URL`: the webhook URL from your Discord server.
3. **Clone and push your repo**: After forking and adding secrets, clone your repo to your local system, make any changes you want, and push them back. The automated workflow will handle the deployment and provide logs to track the deployment status.

## Working with multiple discord webhooks in your secret DISCORD_WEBHOOKS

github actions can't manage json lists properly (I found out that in the hard way) in secrets.
This is why you will have to code the Json to be used as a secret.
Let's assume the following list
```json
[
    {
        "path" : "bitcoin",
        "url" : "http://your_webwook_url_bitcoin_alerts"
    },
    {
        "path" : "kaspa",
        "url" : "http://your_webwook_url_kaspa_alerts"
    }    
]
```

Now you need to code it:

```bash
export WEBHOOKS='[
    {
        "path" : "bitcoin",
        "url" : "http://your_webwook_url_bitcoin_alerts"
    },
    {
        "path" : "kaspa",
        "url" : "http://your_webwook_url_kaspa_alerts"
    }    
]
'
```

You can check if the env is working"
```bash
echo $WEBHOOKS
[ { "path" : "bitcoin", "url" : "http://your_webwook_url_bitcoin_alerts" }, { "path" : "kaspa", "url" : "http://your_webwook_url_kaspa_alerts" } ]
```

And then code it
```bash
printf '%s' "${WEBHOOKS}" | jq -sRr @uri
%5B%0A%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%22path%22%20%3A%20%22bitcoin%22%2C%0A%20%20%20%20%20%20%20%20%22url%22%20%3A%20%22http%3A%2F%2Fyour_webwook_url_bitcoin_alerts%22%0A%20%20%20%20%7D%2C%0A%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%22path%22%20%3A%20%22kaspa%22%2C%0A%20%20%20%20%20%20%20%20%22url%22%20%3A%20%22http%3A%2F%2Fyour_webwook_url_kaspa_alerts%22%0A%20%20%20%20%7D%20%20%20%20%0A%5D%0A
```

And copy and paste this whole coded line to the secret: `DISCORD_WEBHOOKS`

### Using Your Service
Once your service is up and running on Google Cloud Run:
- **Set your service URL**: It will look something like `https://your-service-url/webhook/kaspa`.
- **Configure alerts on TradingView** using a JSON format like below. This is how you tell TradingView what data to send to your Discord:
  ```json
  { 
    "exchange": "{{exchange}}", 
    "ticker": "{{ticker}}",
    "close": "{{close}}",
    "open": "{{open}}",
    "volume": "{{volume}}",
    "event": "Crossing ATH",
    "interval": "{{redacted}}"
  }
  ```
- **Add the webhook URL to TradingView** to start receiving alerts.

### Testing It Locally
#### Without Docker
- **Clone the repo** and prepare your environment:
  ```bash
  git clone [your-repo-url]
  cargo init
  # Add dependencies to Cargo.toml if necessary
  cargo build --release
  cargo clean # optional: cleans old builds
  ```
- **Run the application**:
  use the coded value for the env.
  ```bash
  DISCORD_WEBHOOKS="%5B%0A%20%0A%5D%0A.." \ # ..rest of the coded content
  PORT="8080" RUST_LOG="info" ./target/debug/whistle
  ```
- **Send a test alert**:
  ```bash
  curl -X POST http://localhost:8080/webhook/bitcoin -H "Content-Type: application/json" -d '{"event": "Crossing trend line"}'
  ```

#### With Docker
- **Build and run with Docker** for an isolated environment:
  ```bash
  docker build -t whistle:latest .
  docker run -d -e DISCORD_WEBHOOKS="%5B%0A%20%0A%5D%0A.." \ # ..rest of the coded content
  -e PORT="8080" -e RUST_LOG="debug" whistle:latest
  ```
- **Test the Docker setup**:
  ```bash
  curl -X POST http://localhost:8080/webhook/bitcoin -H "Content-Type: application/json" -d '{"event": "Crossing trend line"}'
  ```

## Contribute and Collaborate
Interested in contributing? Great! You can start by [reporting issues](https://github.com/your-repo/issues) or [submitting pull requests](https://github.com/your-repo/pulls). Your feedback and contributions are welcome!
