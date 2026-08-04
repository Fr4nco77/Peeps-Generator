# ✍️ Peeps Generator

![Peeps Generator](https://res.cloudinary.com/dcav0ojcf/image/upload/v1767064041/videos%20de%20webs%28en%20uso%2C%20no%20borrar%29/hero_kkpcfd.jpg)

**Peeps Generator** is a library designed for developers who need to create fun, unique avatars in their applications.  
With it, you can:

- 🎲 **Generate random or deterministic avatars** using a `seed`.
- 🎨 **Customize every part of the avatar** (head, face, accessories, facial hair, background, colors, etc.).
- 🖼️ **Render SVG** ready to be sent to the frontend and displayed in the browser.
- 📤 **Export to PNG, JPEG, WebP, or AVIF** to save or share on any platform.
- ⚙️ **Integrate a visual editor into your frontend**, generating avatars from the server.

The library is designed to be **simple, modular, and extensible**: you can use it in both Node.js projects and modern web applications.

## 🔗 Official links

- 📦 **GitHub repository**  
  https://github.com/Fr4nco77/Peeps-Generator

- 🎨 **Online visual editor (demo)**  
  https://peep-generator.vercel.app/

## 📦 Installation

You can install **Peeps Generator** with your preferred package manager:

### Using pnpm

```bash
pnpm add peeps-generator
```

### Using npm

```bash
npm install peeps-generator
```

### Using yarn

```bash
yarn add peeps-generator
```

### Using bun

```bash
bun add peeps-generator
```

## 🧠 Runtime environment (important)

**Peeps Generator is designed to run in Node.js environments.**

The library uses native Node APIs to work correctly, including:

- File system access to load SVG assets
- Image processing and composition
- Export to rasterized formats via `sharp`

For this reason, avatar generation **does not happen in the browser**, but in the environment where Node is running.

### ✔️ Supported environments

- Node.js
- Backends (Express, Fastify, Nest, etc.)
- Server-side rendering (SSR) running on Node, for example:
  - Next.js (`runtime: "nodejs"`)
  - Remix (Node adapter)
  - Astro with SSR on Node
- Scripts, workers, or image generation pipelines

### ❌ Directly unsupported environments

- Browser-only environments
- React/Vite applications running only on the client side
- Edge runtimes (Cloudflare Workers, Vercel Edge, etc.)

> If you want to use Peeps Generator in a web application,  
> avatar generation must happen **on the server side**,  
> and then the SVG or resulting image should be sent to the frontend.

This decision is intentional and allows the library to remain **simple, predictable, and consistent**, while avoiding duplicated logic or browser-specific dependencies.

## 🎨 Visual editor (interactive demo)

If you are building a frontend or have questions about how to integrate **Peeps Generator** into a web application,  
you can try the **online visual editor**:

👉 https://peep-generator.vercel.app/

This editor:

- Generates avatars from the **server**
- Renders the SVG in the browser
- Allows you to enable or disable layers
- Changes sizes, seeds, and combinations
- Downloads the avatar in different formats

It is a real example of how to use the library in a **Next.js SSR environment**,  
and it can serve as an architectural reference if you want to build your own editor or configurator.

## 🧩 Usage

With **Peeps Generator**, everything starts with a single function: `createPeep`.  
It is the core of the library: this is where the logic to generate peep avatars and compose each part lives.
Its result is a string containing a complete SVG, already composed and ready to render or export.

### Create your first peep

```ts
import { createPeep } from "peeps-generator";

const svg = createPeep();
```

This call generates a random peep with minimal configuration, where only the essential layers are rendered:

- A monochrome avatar
- No background
- No accessories
- No facial hair

---

### Control avatar layers (`enable*`)

A peep is built in layers.  
Each layer may or may not exist depending on your intention.

That is what the `enable*` options are for.

```ts
import { createPeep } from "peeps-generator";

const svg = createPeep({
  enableAccessories: true,
  enableFacialHair: true,
  enableBackground: true,
  enableColors: true,
});
```

These options indicate **which parts may appear** in the avatar:

- `enableAccessories`  
  Allows the peep to include accessories such as glasses.

- `enableFacialHair`  
  Enables the generation of beards or mustaches.

- `enableColors`  
  Allows the system to apply skin and hair colors.

- `enableBackground`  
  Adds a background color to the SVG.

If an option is not enabled, that layer simply is not rendered.

---

### Control avatar size

You can define the final size of the avatar using the `size` option.

```ts
import { createPeep } from "peeps-generator";

const svg = createPeep({
  size: 128,
});
```

This generates an avatar of **128 × 128 pixels.**

#### What does `size` do?

- Defines the width and height of the final SVG.
- Keeps all avatar proportions intact.
- Does not affect the generation logic or internal parts.

#### Default value

If `size` is not specified, an avatar of **600 × 600 pixels** is generated.

#### Common examples

```ts
createPeep({ size: 64 }); // icons, lists
createPeep({ size: 256 }); // previews
createPeep({ size: 1024 }); // export or print
```

---

### Use a reproducible `seed`

The `seed` allows peep generation to be **deterministic**.  
This means that using the same seed, the visual result will always be the same.

```ts
import { createPeep } from "peeps-generator";

const svg = createPeep({
  seed: "usuario123",
  enableAccessories: true,
  enableFacialHair: true,
});
```

In this example, the text `"usuario123"` is used as the seed for generation.
As long as the `seed` does not change, the generated peep will be identical on each execution.

This is useful for:

- Avatars associated with a user
- Persistent visual identities
- Systems where the avatar must not change over time

If no seed is specified, each call to `createPeep` may produce a different peep.

---

### Create a custom or partially random peep

In addition to automatic generation, **Peeps Generator** allows you to manually define some or all parts of the peep and let the library fill in the rest.

This is done using the `peep` option.

```ts
import { createPeep } from "peeps-generator";

const svg = createPeep({
  peep: {
    head: "Bun",
    face: "Smile",
    hairColor: "#2E2E2E",
    skinColor: "#F1C27D",
  },
  enableAccessories: true,
  enableBackground: true,
});
```

In this example:

- The head and face are explicitly defined
- Hair and skin colors are set manually
- Accessories are generated randomly
- The background color is generated automatically

---

#### Available customization options

When creating a peep, you can optionally define any of the following properties inside `peep`:

| Property      | Description                      | Value type                         |
| ------------- | -------------------------------- | ---------------------------------- |
| `head`        | Type of head or hairstyle        | String from `peepParts.heads`      |
| `face`        | Facial expression                | String from `peepParts.faces`      |
| `hairColor`   | Hair color                       | String (CSS color)                 |
| `skinColor`   | Skin color                       | String (CSS color)                 |
| `facialHair`  | Type of facial hair              | String from `peepParts.facialHair` |
| `accessories` | Accessories                      | String from `peepParts.accessories`|
| `background`  | Avatar background color          | String (CSS color)                 |

These options only apply if the corresponding layer is enabled via the `enable*` options.

---

#### Full or partial customization

- If you define **all** properties, the peep will be fully customized and there will be no random decisions.
- If you define **only some**, the library will automatically generate the missing parts.
- If you define **none**, the peep will be generated completely randomly.

The system only decides on the parts that were not specified and that are enabled via the `enable*` options.

---

### Accessing peep parts

**Peeps Generator** exposes the complete list of available parts for each avatar layer: heads, faces, accessories, and facial hair.

These lists contain the exact names of each asset and represent the visual universe used by the library.  
Using them guarantees that any combination you choose is valid and renderable.

To access these parts, simply import them from the library:

```ts
import { peepParts } from "peeps-generator";

const { heads, faces, accessories, facialHair } = peepParts;
```

Having access to these parts enables, for example:

- Building visual editors or avatar configurators
- Creating guided selectors or forms
- Validating user input before generating a peep
- Generating previews, catalogs, or grids of avatars
- Limiting or expanding options according to your application's own rules

In this way, **Peeps Generator** does not only generate avatars:  
it also gives you the building blocks to design experiences around them.

---

### What can I do with the SVG it returns?

The result of `createPeep` is not a complex object or opaque structure:  
it is a **plain SVG string**.

That string is, at the same time:

- Text you can save, send, or transform
- A vector image ready to render
- A format independent of framework or platform

With it, you can:

- **Render it directly in the DOM**:

  ```jsx
  <div dangerouslySetInnerHTML={{ __html: svg }} />
  ```

- **Use it as the `src` of an image** (base64-encoded) without intermediate files.
- **Save it as an `.svg` file**, exactly as returned by the function.
- **Convert it to other formats** such as PNG, JPEG, WebP, or AVIF, either with the library's `to*` functions or with your own tools.
- **Send it via API or store it in a database**, since it is just text.

## 🖼️ Image export

The result of **createPeep** is always an SVG.  
That SVG is the original source of the avatar.

When you need to work with rasterized images (pixels), the library allows you to convert that SVG into different image formats using the `to*` functions.

### Per-format export

If you need a specific format, you can use the dedicated functions directly:

#### Convert to PNG

```ts
import { toPNG } from "peeps-generator";

const pngBuffer = await toPNG(svg);
```

#### Convert to JPEG

```ts
import { toJPEG } from "peeps-generator";

const jpegBuffer = await toJPEG(svg);
```

#### Convert to WebP

```ts
import { toWebP } from "peeps-generator";

const webpBuffer = await toWebP(svg);
```

#### Convert to AVIF

```ts
import { toAvif } from "peeps-generator";

const avifBuffer = await toAvif(svg);
```

Each of these functions receives the SVG string and returns an image buffer ready for:

- Saving to disk
- Sending over an API
- Uploading to a CDN
- Using in asset generation workflows

---

### Universal exporter (`exportTo`)

In addition to the individual functions, **Peeps Generator** exposes a universal exporter that allows converting the SVG to the desired format using a single function.

```ts
import { exportTo } from "peeps-generator";

const buffer = await exportTo(svg, "png");
```

#### Supported formats

- `"png"`
- `"jpg"` / `"jpeg"`
- `"webp"`
- `"avif"`

This approach is useful when:

- The format is decided dynamically
- You want to simplify conditional logic
- You need a more generic and expressive API

The result is always a **Buffer**, regardless of the chosen format.

---

### Important notes about export

- Export functions work in Node.js.
- They internally use `sharp`.
- They return a Buffer ready to be saved to disk, sent over the network, or processed.

## ⚠️ Common errors and solutions

### 500 error in `/api` in production (works locally)

**Symptoms**

- `POST /api 500 (Internal Server Error)`
- The application works correctly in local development
- Vercel logs show an error similar to:

```text
ENOENT: no such file or directory, open
.../node_modules/peeps-generator/dist/assets/heads/SomeFile.svg
```

### Cause

`peeps-generator` loads SVG files from its internal `dist/assets` folder.

In serverless environments (such as Vercel), **Next.js does not automatically include static files that are inside `node_modules`**, unless they are explicitly specified during the build.

That is why, in production, the API cannot find the required SVGs and fails.

---

### Solution

1. **Force the Node.js runtime in the API route**

   ```ts
   export const runtime = "nodejs";
   ```

   **`peeps-generator` is not compatible with the Edge Runtime.**

2. **Include the generator assets in `next.config.ts`**

   ```ts
   import type { NextConfig } from "next";

   const nextConfig: NextConfig = {
     outputFileTracingIncludes: {
       "/api/**": ["node_modules/peeps-generator/dist/assets/**/\*"],
     },
   };

   export default nextConfig;
   ```

3. **Deploy the project again**

After applying the changes:

- If necessary, clear the build cache
- Make a new deployment on Vercel

## 🪶 Credits and attribution

The visual assets used by **Peeps Generator** are inspired by **Open Peeps**, an illustration collection created by Pablo Stanley.

Original project:
https://www.openpeeps.com/

This library does not seek to replace or redistribute the original project,
but to provide a programmatic way to **compose, combine, and generate avatars**
from modular illustrations.

All artistic credit belongs to the original author.

## 📄 License

This project is distributed under the **MIT** license.

This means you can:

- Use it in personal or commercial projects
- Modify it freely
- Redistribute it
- Integrate it into your own applications

As long as the copyright notice and original license are retained.
