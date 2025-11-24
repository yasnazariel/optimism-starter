<div align="center">
  <a href="https://optimism.io">
    <img
      alt="Optimism"
      src="https://raw.githubusercontent.com/ethereum-optimism/brand-kit/main/assets/svg/OPTIMISM-R.svg"
      width="320"
    />
  </a>
  <br />
  <br />
</div>

This repository is an opinionated starter kit for building full‑stack dapps on Optimism using a modern web3 stack: [Optimism](https://github.com/ethereum-optimism), [wagmi](https://wagmi.sh), [Foundry](https://book.getfoundry.sh/), [RainbowKit](https://www.rainbowkit.com/), and [Vite](https://vitejs.dev/). It was originally bootstrapped with [`create-wagmi`](https://github.com/wagmi-dev/wagmi/tree/main/packages/create-wagmi) and is designed to get hackers shipping quickly.

## Who is this for?

This starter is a great choice for any of the following groups:

- Hackers building apps on [Optimism](https://www.optimism.io/).
- Hackers exploring the [Attestation Station](https://community.optimism.io/docs/identity/build/).
- Developers interested in using a modern, production‑ready web3 full‑stack template with contracts, React, and wallet connectivity already wired up.

If you are new to Optimism or to this stack, this project gives you a concrete example you can fork, customize, and deploy.

## Getting Started

### Install Node

To work with this repository locally, you will need Node.js and npm. You can download Node from the official website: [https://nodejs.org/en/download/](https://nodejs.org/en/download/).

- Make sure you are using Node version later than 14.18.0, or 16 and above.  
- These instructions have been verified with Node 18.

You can check your version with:

node --version

text

### Install Foundry

Foundry is used to build, test, and deploy the smart contracts in this starter. Follow the official installation guide here: [Foundry installation docs](https://book.getfoundry.sh/getting-started/installation).

A typical installation flow looks like this:

1. Run the installer script:

curl -L https://foundry.paradigm.xyz | bash

text

2. Follow the instructions printed in your terminal to source your shell configuration (for example by running `source ~/.bashrc` or `source ~/.zshrc`).

3. Finally, install or update Foundry by running:

foundryup

text

After installation, you should be able to run `forge --version` and `anvil --version` to confirm everything is set up correctly.

## Set up environment

Before deploying contracts or generating typed ABIs, you will need to configure a few environment variables.

### Get an Etherscan key for Optimism

1. Register for an account on [Etherscan for Optimism](https://explorer.optimism.io/register).  
This account is separate from your standard Etherscan (Ethereum mainnet) account.

2. After registration, go to the [API keys page](https://explorer.optimism.io/myapikey) and click **Add** to create a new API key.

Keep this key safe; it will be used to verify and interact with your contracts.

### Configure `.env`

The project uses a `.env` file to provide configuration to Forge and the frontend.

1. Copy the example environment file:

cp .env.example .env

text

2. Open `.env` in your editor and set the following variables:

- `ETHERSCAN_API_KEY`: Your Optimism Etherscan API key from the step above.
- `FORGE_RPC_URL`: The RPC URL of the network you want to deploy to.  
  If you are using [Alchemy](https://github.com/ethereum-optimism/optimism-tutorial/tree/main/ecosystem/alchemy), the URL will look like:

  ```
  https://opt-goerli.g.alchemy.com/v2/<ALCHEMY_API_KEY>
  ```

- `FORGE_PRIVATE_KEY`: The private key of the wallet you want to deploy from. Use a development or test key, not a production key.
- `VITE_WALLETCONNECT_PROJECT_ID`: A WalletConnect v2 project ID from your WalletConnect dashboard. See:  
  https://docs.walletconnect.com/2.0/web/web3wallet/installation#obtain-project-id

Make sure you never commit your real private keys or secrets to version control.

## Start the application

The repository includes a simple frontend that connects to Optimism and your contracts.

<img
width="450"
alt="starter-app-screenshot"
src="https://user-images.githubusercontent.com/389705/225778318-4e6fb8c0-c5d7-4aea-9fc2-2efd17ca435c.png"
/>

If you are only browsing the code or editing from the GitHub web interface, you can skip the local steps below and use this README as a reference. When you are ready to run the app locally, follow these commands:

1. Clone or fork the `optimism-starter` repo:

git clone https://github.com/ethereum-optimism/optimism-starter.git

text

2. Install the necessary Node packages:

cd optimism-starter
npm install

text

3. Start the frontend development server:

npm run dev

text

If you see build errors at this step, you may need to [update Foundry to the latest version](#install-foundry) or ensure your Node.js version satisfies the requirements.

4. Open `http://localhost:5173` in your browser.

Once the page has loaded, changes made to files inside the `src/` directory (for example `src/App.tsx`) will automatically hot‑reload in the browser so you can iterate quickly.

For answers to common questions such as “Where to get Goerli ETH?” or “How to deploy a public version of your app?”, see the [FAQ](./FAQ.md).

- [Where to get Goerli ETH](./FAQ.md#where-to-get-goerli-eth).  
- [How to deploy a public version of your app](./FAQ.md#how-do-i-deploy-this).

## Generate ABIs & React Hooks

This project ships with `@wagmi/cli` preconfigured so that you can generate type‑safe ABIs and React Hooks directly from your contracts.

To generate ABIs and Hooks from your Foundry project, run:

npm run wagmi

text

This command reads the wagmi configuration (`wagmi.config.ts`) and generates a `src/generated.ts` file containing:

- Typed ABIs for your contracts.  
- Ready‑to‑use hooks that integrate with wagmi.

You can then import and use these hooks in your components. For example, see the usage in the [`Attestoooooor` component](https://github.com/ethereum-optimism/optimism-starter/blob/main/src/components/Attestoooooor.tsx#L77).

## Deploying Contracts

To deploy your contracts to a live network, this starter uses Foundry’s [Forge](https://book.getfoundry.sh/forge/) toolchain.

You can find a full overview of deploying with Forge in the official docs:  
https://book.getfoundry.sh/forge/deploying

This repository also includes a convenience script in `package.json` to help you get started quickly.

### Deploy contract

After configuring your `.env` and building your contracts, you can deploy using:

npm run deploy

text

This script will:

- Use Forge with the RPC and private key from your `.env`.  
- Deploy your contract(s) to the configured network (for example Optimism Goerli or Optimism mainnet, depending on your RPC).

Check your terminal output for the deployed contract address and any verification steps.

## Developing with Anvil (Optimism Mainnet Fork)

You can develop against a local fork of Optimism using Anvil, while the frontend and wagmi hooks update automatically.

### Start dev server with Foundry

Run:

npm run dev:foundry

text

This command will:

- Start a Vite development server for the frontend.  
- Start `@wagmi/cli` in [watch mode](https://wagmi.sh/cli/commands/generate#options) so changes to your contracts automatically regenerate `src/generated.ts`.  
- Start an Anvil instance that forks Optimism (for example Goerli Optimism) and exposes an RPC URL.

### Deploy your contract to Anvil

With Anvil running, deploy your contracts to the forked Optimism network:

npm run deploy:anvil

text

This uses the Anvil RPC target so that you can interact with your contracts locally with mainnet‑like state.

## Start developing

After deploying to Anvil, you can interact with your contracts directly from the web UI.

1. Open `http://localhost:5173` in your browser.  
2. Connect your wallet (for example MetaMask or Coinbase Wallet).  
3. Use the generated hooks from `src/generated.ts` to read and write contract state. The [`Attestoooooor` component](https://github.com/ethereum-optimism/optimism-starter/blob/main/src/components/Attestoooooor.tsx) shows a concrete usage pattern.

> Tip: If you import one of the Anvil private keys into your browser wallet, you will have a balance of test ETH (for example `10,000 ETH`) to experiment with. When you run `npm run dev:foundry`, Anvil prints these keys under the “Private Keys” section in your terminal.

## Alternatives

If this stack is close but not exactly what you need, there are several related templates you can explore:

- [create wagmi cli](https://wagmi.sh/cli/create-wagmi) – A flexible CLI with many templates; this starter kit began from the `vite-react-cli-foundry` template.
- [scaffold-eth-2](https://github.com/scaffold-eth/se-2) – The new iteration of a popular Next.js‑based starter including Hardhat, burner wallets, rich documentation, and an active community.
- [Awesome wagmi templates](https://github.com/wagmi-dev/awesome-wagmi#templates) – A curated list of additional wagmi examples and starter projects.
- [Create Eth App](https://usedapp-docs.netlify.app/docs/Getting%20Started/Create%20Eth%20App) – Uses `useDapp` (a wagmi alternative) and is used by projects at OP Labs.

Each of these options targets slightly different preferences around frameworks (Vite vs Next.js), contract toolchains (Foundry vs Hardhat), and wallet UX (burner wallets vs standard wallets).

## Learn more

To dive deeper into the tools and concepts used in this starter, check out:

- [Optimism docs](https://optimism.io) – Learn about the OP Stack, the Superchain, and how to deploy and operate on Optimism.  
- [Foundry documentation](https://book.getfoundry.sh/) – Learn more about Forge, Anvil, and the Foundry toolchain.  
- [wagmi documentation](https://wagmi.sh) – Explore wagmi hooks, configuration, and patterns.  
- [wagmi examples](https://wagmi.sh/examples/connect-wallet) – A collection of small, focused examples for common tasks.  
- [@wagmi/cli documentation](https://wagmi.sh/cli) – Learn how to customize code generation for your own contracts.  
- [Vite documentation](https://vitejs.dev/) – Understand Vite’s dev server, build pipeline, and configuration options.

This starter should give you a solid foundation for hacking on Optimism, whether you are only exploring from the GitHub UI or running the full local development stack.
