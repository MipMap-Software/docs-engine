# MipMap Engine Documentation

MipMap Engine documentation site built with VitePress, with multi-language support and local search.

[View Online Documentation](https://docs.mipmap3d.com/engine)

## Install and Run

### Install Node.js

Node.js is required for local development. Download and install the LTS release from the [Node.js website](https://nodejs.org/en/download).

### Install Project Dependencies

Open a terminal and install `yarn`:

```bash
npm install -g yarn
```

Install project dependencies from the repository root:

```bash
yarn
```

To edit documentation, start the development server:

```bash
yarn dev
```

Once the dev server is running, open port 4002 in your browser to preview the site. Changes in your editor are reflected in the browser in real time.

## Build and Deploy

After editing, build the static site:

```bash
yarn build
```

Deploy the contents of the `build/` directory to your hosting service.
