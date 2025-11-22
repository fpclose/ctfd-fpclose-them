# CTFd Theme - FPClose

A custom CTFd theme featuring the beautiful Pipeline background animation from Ambient Canvas Backgrounds.

## Features

- **Pipeline Background Animation**: Dynamic animated canvas background on challenge pages
- Based on the core CTFd theme with enhanced visual effects
- Responsive design that works on all devices
- Optimized performance with hardware-accelerated canvas rendering

## Pipeline Animation

The Pipeline effect creates flowing, interconnected lines that move across the jumbotron background, creating an ambient and engaging visual experience. This is based on the work by Sean Free from the [Ambient Canvas Backgrounds](https://github.com/crnacura/AmbientCanvasBackgrounds) project.

### Customization

You can customize the Pipeline animation by editing `/static/js/pipeline/pipeline-jumbotron.js`:

- `pipeCount`: Number of pipe elements (default: 30)
- `baseHue`: Base color hue (default: 180 - cyan/blue)
- `rangeHue`: Color variation range (default: 60)
- `backgroundColor`: Canvas background color (default: dark green)
- `baseSpeed` / `rangeSpeed`: Animation speed

### Color Scheme

The current theme uses a blue/cyan color palette (`#0ab5e4`) to match the Pipeline animation.

## Installation

This theme is already installed in your CTFd instance. To activate it:

1. Go to **Admin Panel** → **Config** → **Theme**
2. Select **ctfd-fpclose** from the dropdown
3. Click **Update**

## Credits

- **Pipeline Animation**: Sean Free ([Codrops](https://tympanus.net/codrops/?p=36743))
- **Simplex Noise Library**: jwagner
- **CTFd Core Theme**: CTFd Team

## Subtree Installation

### Add repo to themes folder

```
git subtree add --prefix CTFd/themes/ctfd-fpclose git@github.com:CTFd/ctfd-fpclose.git main --squash
git subtree add --prefix CTFd/themes/core-beta git@github.com:CTFd/core-beta.git main --squash
```

### Pull latest changes to subtree

```
git subtree pull --prefix CTFd/themes/core-beta git@github.com:CTFd/core-beta.git main --squash
```

### Subtree Gotcha

Make sure to use Merge Commits when dealing with the subtree here. For some reason Github's squash and commit uses the wrong line ending which causes issues with the subtree script: https://stackoverflow.com/a/47190256.

## Creating Custom Theme (based on core-beta)

To create a custom theme based on the core-beta one, here are the steps to follow:

1. Clone core-beta theme locally to a seperate folder

   ```
   git clone https://github.com/CTFd/core-beta.git custom-theme
   ```

   To clarify the structure of the project, the `./assets` folder contains the uncompiled source files (the ones you can modify), while the `./static` directory contains the compiled ones.

2. Install [Yarn](https://classic.yarnpkg.com/en/) following the [official installation guides](https://classic.yarnpkg.com/en/docs/install).

   - **Yarn** is a dependency management tool used to install and manage project packages
   - **[Vite](https://vite.dev/guide/)** handles the frontend tooling in CTFd by building optimized assets that are served through Flask.

3. Run `yarn install` in the root of `custom-theme` folder to install the necessary Node packages including `vite`.

4. Run the appropriate yarn build mode:

   - Run `yarn dev` (this will run `vite build --watch`) while developing the theme.
   - Run `yarn build` (which will run `vite build`) for a one-time build.
     Vite allows you to preview changes instantly with hot reloading.

5. Now, you can start your modifications in the `assets` folder. Each time you save, Vite will automatically recompile everything (assuming you are using `yarn dev`), and you can directly see the result by importing your compiled theme into a CTFd instance.
   Note: You do not need the `node_modules` folder, you can simply zip the theme directory without it.

6. When you are ready you can use `yarn build` to build the production copy of your theme.

## Todo

- Document how we are using Vite
- Create a cookie cutter template package to use with Vite
