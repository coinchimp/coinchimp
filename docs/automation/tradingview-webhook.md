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

### Using Your Service
Once your service is up and running on Google Cloud Run:
- **Set your service URL**: It will look something like `https://your-service-url/webhook`.
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
  ```bash
  WEBHOOK_URL="your_discord_webhook_url" PORT="8080" RUST_LOG="info" ./target/debug/whistle
  ```
- **Send a test alert**:
  ```bash
  curl -X POST http://localhost:8080/webhook -H "Content-Type: application/json" -d '{"event": "Crossing trend line"}'
  ```

#### With Docker
- **Build and run with Docker** for an isolated environment:
  ```bash
  docker build -t whistle:latest .
  docker run -d -e WEBHOOK_URL="your_discord_webhook_url" -e PORT="8080" -e RUST_LOG="debug" whistle:latest
  ```
- **Test the Docker setup**:
  ```bash
  curl -X POST http://localhost:8080/webhook -H "Content-Type: application/json" -d '{"event": "Crossing trend line"}'
  ```

## Contribute and Collaborate
Interested in contributing? Great! You can start by [reporting issues](https://github.com/your-repo/issues) or [submitting pull requests](https://github.com/your-repo/pulls). Your feedback and contributions are welcome!
