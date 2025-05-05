# Token-2022 Confidential Transfer Talk

This repository contains a Slidev presentation for demonstrating the Token2022 Confidential Transfer (CT) Extension. It explains the concepts and workflow of confidential transfers on Solana, complementing the demo repository below.

- **Presentation**: [token-2022-confidential-transfer-talk](https://pupplecat.github.io/token-2022-confidential-transfer-talk)
- **Demo Repository**: [token-2022-confidential-transfer-example](https://github.com/pupplecat/token-2022-confidential-transfer-example)

## Prerequisites

- **Node.js**: Install Node.js (LTS version recommended). See [Node.js installation](https://nodejs.org/en/download/).
- **Yarn**: Install Yarn as the package manager. Run `npm install -g yarn` or follow the [Yarn installation guide](https://yarnpkg.com/getting-started/install).
- **Slidev CLI**: (optional) Install Slidev globally by running:
  ```bash
  npm i -g @slidev/cli
  ```

## Development

### 1. Install Dependencies
Clone the repository and install dependencies:

```bash
git clone https://github.com/pupplecat/token-2022-confidential-transfer-talk.git
cd token-2022-confidential-transfer-talk
yarn install
```

### 2. Run Development Server
Start the Slidev development server to preview the presentation:

```bash
yarn dev
```

This will launch the presentation at `http://localhost:3030`. Edit `slides.md` to update the content, and changes will hot-reload automatically.

### 3. Build for Production
Build the presentation for static hosting (e.g., GitHub Pages):

```bash
yarn build
```

The built files will be output to the `dist` folder, ready for deployment.

## Deployment

The presentation is configured to deploy to GitHub Pages using a GitHub Actions workflow (`deploy.yml`). To deploy:

1. Ensure GitHub Pages is enabled in your repository settings (Settings > Pages > Source: `GitHub Actions`).
2. Push changes to the `main` branch or trigger the workflow manually via the Actions tab.
3. Once deployed, access the presentation at `https://pupplecat.github.io/token-2022-confidential-transfer-talk/`.

**Note**: If the presentation uses assets (e.g., images in `/assets`), ensure they are included in the build output (`dist`). The `deploy.yml` workflow handles this automatically.

## Viewing the Demo

To see the concepts in action, check out the [demo repository](https://github.com/pupplecat/token-2022-confidential-transfer-example). It includes shell scripts and Rust examples for running a Token2022 Confidential Transfer demo on a local Solana validator.