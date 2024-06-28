---
date: 2024-05-28
categories:
  - radiant
  - coding
tags:
  - radiant
  - google cloud run
  - containers
  - photonic
authors:
  - coinchimp
---
# How to Deploy Razoo's Photonic Wallet on Google Cloud Run

Hello everyone! I’m excited to share my latest adventure – setting up Razoo’s Photonic Wallet on Google Cloud Run. This wallet isn’t just for sending and receiving tokens; it allows you to create new tokens, including mineable meme tokens. It’s fair, secure, and efficient, thanks to Radiant's induction proofs. Let's dive into the details!

## Step 1: Setting Up the Dockerfile

Here’s the Dockerfile that you’ll need to build the Photonic Wallet image:

```Dockerfile
FROM node:18
RUN npm install -g pnpm
RUN git clone https://github.com/coinchimp/photonic-wallet
WORKDIR /photonic-wallet
RUN pnpm install
RUN pnpm build
RUN npm install -g http-server
EXPOSE 8080
CMD ["http-server", "/photonic-wallet/packages/app/dist", "-p", "8080"]
```

If you’re running it locally with self-signed certificates, use this Dockerfile:

```Dockerfile
FROM node:18
RUN npm install -g pnpm
RUN git clone https://github.com/coinchimp/photonic-wallet
WORKDIR /photonic-wallet
RUN pnpm install
RUN pnpm build
RUN npm install -g http-server
RUN mkdir -p /photonic-wallet/certs && \
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /photonic-wallet/certs/selfsigned.key -out /photonic-wallet/certs/selfsigned.crt -subj "/C=US/ST=State/L=City/O=Organization/OU=Unit/CN=localhost"
EXPOSE 8080
CMD ["http-server", "/photonic-wallet/packages/app/dist", "-p", "8080", "-S", "-C", "/photonic-wallet/certs/selfsigned.crt", "-K", "/photonic-wallet/certs/selfsigned.key"]
```

## Step 2: GitHub Actions Workflow

Here’s the workflow file to automate the build and deployment process to Google Cloud Run:

```yaml
name: Deploy Photonic Wallet to Google Cloud Run

on:
  push:
    branches:
      - master

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Log in to Google Container Registry
        uses: docker/login-action@v2
        with:
          registry: gcr.io
          username: _json_key
          password: ${{ secrets.GCP_RUN_CREDENTIALS }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v3
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: gcr.io/${{ secrets.GOOGLE_PROJECT_ID }}/photonic-wallet:${{ github.sha }}

      - name: Authenticate to Google Cloud manually
        env:
          GCP_JSON_KEY: ${{ secrets.GCP_RUN_CREDENTIALS }}
        run: |
          echo "$GCP_JSON_KEY" > /tmp/gcp-key.json
          gcloud auth activate-service-account --key-file=/tmp/gcp-key.json

      - name: Set Google Cloud Project
        env:
          GCP_PROJ_ID: ${{ secrets.GOOGLE_PROJECT_ID }}
        run: gcloud config set project "$GCP_PROJ_ID"

      - name: Verify Authentication
        run: |
          gcloud auth list
          gcloud config list project

      - name: Deploy to Google Cloud Run
        run: |
          gcloud run deploy photonic-wallet \
            --image gcr.io/${{ secrets.GOOGLE_PROJECT_ID }}/photonic-wallet:${{ github.sha }} \
            --platform managed \
            --region us-central1 \
            --allow-unauthenticated
        shell: bash
```

## Step 3: Setting Up Google Cloud Run

Before deploying, make sure to create an instance in Cloud Run and set the necessary permissions:

1. **Create an Instance:**
   - Go to the [Google Cloud Console](https://console.cloud.google.com/).
   - Navigate to Cloud Run and create a new service.
   - Choose your region and the container image URL (from Google Container Registry).

2. **Set Permissions:**
   - Ensure the Cloud Run service account has the following roles:
     - `Cloud Run Admin`
     - `Storage Admin`
     - `Artifact Registry Reader`
   - You can set these permissions in the IAM section of the Google Cloud Console.

### Conclusion

That’s it! You’ve successfully deployed Razoo’s Photonic Wallet to Google Cloud Run. For more details, check out the [Photonic Wallet repo](https://github.com/coinchimp/photonic-wallet). Happy deploying and enjoy creating your own tokens!

Cheers

