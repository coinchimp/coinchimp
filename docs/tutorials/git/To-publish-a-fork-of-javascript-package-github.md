# Publish forked randiant.js project package at GitHub
This tutorial can actually apply to any package, however I will do it more focused on randiant.js, because is the project I am working on now.
I am doing this to make it easy for me to use as dependency for [Razoo's Photonic Wallet](https://github.com/1razoo/photonic-wallet) and start playing with truly descentralized and trustless fungible tokens.

Ok, back to our topic. to publish a fork of `https://github.com/RadiantBlockchain/radiantjs` as an optional registry on GitHub, follow these steps:

1. **Fork the repository:**
  
    Go to the `RadiantBlockchain/radiantjs` repository on GitHub: [RadiantBlockchain/radiantjs](https://github.com/RadiantBlockchain/radiantjs).
    Click the "Fork" button in the top-right corner to create a copy of the repository under your own GitHub account.

2. **Clone the forked repository:**

    Clone your forked repository to your local machine:
     ```bash
     git clone https://github.com/YOUR_USERNAME/radiantjs.git
     cd radiantjs
     ```

3. **Make any necessary changes:**
    
    Make any changes to the `package.json` or other files as needed.
    In my case, I change the repo and package info to direct it to my github account:
    ```json
    {
    "name": "@coinchimp/radiantjs", //replace with your account
    "description": "Javascript library for Radiant Blockchain (RXD)",
    "version": "1.9.4",
    "author": "Radiant Blockchain <radiantblockchain@protonmail.com>",
    "main": "index.js",
    "scripts": {
        "lint": "standard",
        "lint_test": "standard && mocha",
        "test": "mocha",
        "radiant": "mocha --grep=\"radiant\"",
        "exact_match": "mocha --grep=\"exact_match\"",
        "coverage": "nyc --reporter=text npm run test",
        "build-radiant": "webpack index.js --config webpack.config.js",
        "build-ecies": "webpack ecies/index.js --config webpack.subproject.config.js --output-library radiantEcies -o radiant-ecies.min.js",
        "build-message": "webpack message/index.js --config webpack.subproject.config.js --output-library radiantMessage -o radiant-message.min.js",
        "build-mnemonic": "webpack mnemonic/index.js --config webpack.subproject.config.js --output-library radiantMnemonic -o radiant-mnemonic.min.js",
        "build": "yarn build-radiant && yarn build-ecies && yarn build-message && yarn build-mnemonic",
        "prepublishOnly": "yarn build"
    },
    "unpkg": "radiant.min.js",
    "keywords": [
        "radiant",
        "radiant-blockchain",
        "transaction",
        "address",
        "p2p",
        "ecies",
        "cryptocurrency",
        "blockchain",
        "payment"
    ],
    "repository": {
        "type": "git",
        "url": "https://github.com/coinchimp/radiantjs" //replace with your repo
    },
    "browser": {
        "request": "browser-request"
    },
    "dependencies": {
        "aes-js": "^3.1.2",
        "bn.js": "=4.11.9",
        "bs58": "=4.0.1",
        "clone-deep": "^4.0.1",
        "elliptic": "6.5.4",
        "hash.js": "^1.1.7",
        "inherits": "2.0.3",
        "js-sha512": "^0.8.0",
        "unorm": "1.4.1"
    },
    "devDependencies": {
        "brfs": "2.0.1",
        "buffer": "^6.0.3",
        "chai": "4.2.0",
        "mocha": "^8.4.0",
        "nyc": "^14.1.1",
        "sinon": "7.2.3",
        "standard": "12.0.1",
        "webpack": "^4.47.0",
        "webpack-cli": "^3.3.12"
    },
    "license": "MIT",
    "standard": {
        "globals": [
        "afterEach",
        "beforeEach",
        "describe",
        "it"
        ],
        "ignore": [
        "dist/**"
        ]
    }
    }
    ```
4. **install and build the package**
    
    Normally is only `npm install` and then `npm run build`, but in my case, I needed to prep a few things first:
    ```bash
    npm install
    npm install --save-dev webpack webpack-cli
    npm install --save-dev buffer
    export NODE_OPTIONS=--openssl-legacy-provider #probably this won't be require later
    npm run build
    ```
5. **Commit and push your changes:**
    
    Add and commit your changes:

     ```bash
     git add .
     git commit -m "Your commit message"
     ```
    Push your changes to your forked repository:

      ```bash
      git push origin master
      ```

6. **Configure GitHub Packages registry (optional):**
    
    If you want to publish your package to GitHub Packages, you need to configure the registry in your `.npmrc` file. Create or edit the `.npmrc` file in your project root directory with the following content:

     ```plaintext
     @YOUR_USERNAME:registry=https://npm.pkg.github.com/
     //npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN
     ```

7. **Publish the package:**
    
    To publish your package to the GitHub Packages registry, use the following command:

     ```bash
     npm publish --registry=https://npm.pkg.github.com
     ```

    Replace `YOUR_GITHUB_TOKEN` with a personal access token that has the `write:packages` and `repo` scopes. You can create a personal access token in your GitHub settings under Developer settings -> Personal access tokens.

8. **Use the package:**
    
    To install your package from the GitHub Packages registry, you need to configure the registry in the consuming project's `.npmrc` file:

     ```plaintext
     @YOUR_USERNAME:registry=https://npm.pkg.github.com/
     ```

    Then, install the package using npm or yarn:

     ```bash
     npm install @YOUR_USERNAME/radiantjs
     ```

By following these steps, you will have successfully forked, modified, and published the `radiantjs` package from `RadiantBlockchain` under your own GitHub account, and made it available as an optional registry for others to install.