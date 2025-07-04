# astro-get-remote-assets

An Astro integration that automatically downloads remote images from your CMS during the build process and updates HTML references to use local paths.

## Installation

```bash
npm install astro-get-remote-assets
```

## Usage

Add the integration to your `astro.config.mjs`:

```js
import { defineConfig } from 'astro/config';
import getRemoteAssets from 'astro-get-remote-assets';

export default defineConfig({
  integrations: [
    getRemoteAssets({
      url: 'https://images.microcms-assets.io',
      imageDir: './images',
    }),
  ],
});
```

## Configuration Options

- `url` (string): The base URL of your CMS image server to match and download images from
- `imageDir` (string): The directory where downloaded images will be saved (default: `'./images'`)

## How it Works

This integration:
1. Runs after your Astro site builds
2. Scans all HTML files in the build output
3. Finds all `<img>` and `<source>` tags with `src` or `srcset` attributes matching your CMS URL
4. Downloads these images to your local `imageDir`
5. Updates the HTML to reference the local image paths

## Example

If your HTML contains:
```html
<img src="https://images.microcms-assets.io/assets/blog/hero.jpg" alt="Hero">
```

After building, it will be updated to:
```html
<img src="./images/cms/cms-image-[hash].jpg" alt="Hero">
```

## Features

- Automatic image downloading during build
- Handles both `src` and `srcset` attributes
- Prevents duplicate downloads with caching
- Supports redirects
- 30-second timeout per image download
- Preserves image extensions when possible
- Creates unique filenames using MD5 hashes to avoid conflicts
