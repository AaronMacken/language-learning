This file is used as a devlog to track my progress and learning

Phase 1: Repo and webapp scaffold

---

Ran this in the terminal

```
corepack enable
corepack prepare pnpm@latest --activate
```

Explanation: corepack is a tool that ships with Node v16.9+
It turns corepack on and lets node manage package managers like pnpm and yarn
No need to globally install pnpm

the second line downloads the latest version of pnpm and activates it as the version on your machine

Other devs on the project will also need to run this command while setting up their workspace

why pnpm? because its good for monorepos

It uses a global store so you dont have to reinstall packages that have already been installed before

---

Why a monorepo anyways? Shared Code.

I can easily share common components, TypeScript types, domain logic to be reused across apps.
One ESLint config, one TS config, one prettier config, one dependency strategy.
I can update a shared component and all consuming apps in one PR, test together, deploy confidently.
No version mismatch chaos or additional overhead with having mutiple PRs to consume the new dependency package version.

---

Some package json config,

```
{
  "name": "language-learning",
  "private": true,
  "scripts": {
    "dev:web": "pnpm --filter ./apps/web dev",
    "build:web": "pnpm --filter ./apps/web build"
  }
}
```

Private is set to true because we don't want to publish this repo to npm
Have some scripts in there so you can run the web app from the root,
unsure of how the architecture will look as of right now, this is just AI boiler plate
Jurys out on if we will use that or not

---

Workspaces (monorepo magic) -> pnpm-workspace.yaml

```
packages:
  - 'apps/*'
  - 'packages/*'
```

This tells pnpm, "treat any folder inside apps/ or packages/ as a package"

Meaning, each of the following below (example) can have it's own package.json

```
apps/
  web/
packages/
  domain/
  utils/
```

When you run `pnpm install`, pnpm will detect all folders matching apps/_ and packages/_
then look for a package.json inside of each
it treats them like local packages and automatically links them via symlinks

A what? a symlink! (symbolic link)

As mentioned previously, the benefit of our mono repo is having it act as a source of truth, as opposed to having multiple repos and shared code packages that we need to manage the overhead of.

When we do something like this

```
import { myUtil } from 'domain';
```

thanks to the workspace config, pnpm treats packages/domain as a local package.

So when we attempt to import `domain`, Node resolves it from `node_modules`,
but pnpm created a symlink there that points to the local packages/domain folder.

This lets us use the live code without publishing anything - less friction.

---

Ran this command

```
pnpm create vite apps/web --template react-ts
```

This scaffolds a React + TypeScript app inside apps/web using Vite.
It generates the base project structure along with config files like
vite.config.ts, tsconfig.json, and ESLint setup
similar to Create React App, but without hiding the build configuration.
