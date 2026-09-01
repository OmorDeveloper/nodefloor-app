# Third-party licenses

Software redistributed inside NodeFloor installers, and the notices that must
travel with it.

npm dependencies are not listed here — they are resolved at install time from
`package.json` / `package-lock.json` and carry their own licenses in
`node_modules`. This file covers third-party software **we bundle into the
shipped application**, where the license obliges us to pass its notice on.

---

## The upstream NodeFloor project — MIT

NodeFloor is built on an earlier MIT-licensed project by **Chaitanya Giri** and
its contributors. Substantial portions of this software originate there.

The MIT licence allows that work to be built on and redistributed under
different terms — which is what `LICENSE` does — on the condition that the
notice below travels with every copy. It is reproduced here to satisfy that
condition, and it must not be removed.

```
MIT License

Copyright (c) 2026 Chaitanya Giri

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

NodeFloor is an independent product and is not affiliated with, endorsed by, or
sponsored by the upstream project or its contributors.

---

## Hermes Agent — MIT

- **Upstream:** https://github.com/NousResearch/hermes-agent
- **Bundled at:** `resources/hermes/` → `<app resources>/hermes/` in the
  installer (staged by `npm run bundle:hermes`; see `tools/bundle-hermes.cjs`
  for the exact pinned ref).
- **Used as:** the engine behind the **CEO** role — selectable in NodeFloor as
  the `CEO · Hermes` provider (`src/shared/agentProvider.ts`), spawned through
  `src/main/hermesRuntime.ts`.

Hermes Agent is a separate work by Nous Research, redistributed unmodified under
the MIT license. NodeFloor's own branding applies to NodeFloor; it does not
extend to Hermes Agent, and NodeFloor is not affiliated with or endorsed by Nous
Research.

The MIT license requires that the copyright and permission notice below be
included with any copy or substantial portion of the software. It ships inside
the bundle as `resources/hermes/LICENSE`, and the bundler **fails the build**
rather than produce a bundle without it.

```
MIT License

Copyright (c) 2025 Nous Research

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

### Transitive dependencies

The bundle also contains a Python interpreter and the packages Hermes pins
(`uv sync --locked --extra all`) — each under its own license, predominantly
MIT / BSD / Apache-2.0 / PSF. Their license files ship inside the bundle's
`runtime/` tree as installed by their own package metadata. To enumerate them
for a compliance review, run against a staged bundle:

```
resources/hermes/runtime/bin/python -m pip list      # posix
resources\hermes\runtime\Scripts\python.exe -m pip list   # windows
```

---

## Office pixel art — LimeZu, Modern Interiors (licensed)

The office floor is drawn with **Modern Interiors - RPG Tileset [16X16]** by
**LimeZu**, used under the Complete Version licence purchased 2026-08-20.

**Credit is a condition of that licence, not a courtesy:**

> Modern Interiors - RPG Tileset [16X16] by LimeZu — <https://limezu.itch.io/>

The terms, verbatim:

```
CAN:
YOU CAN EDIT AND USE THE ASSET IN ANY COMMERCIAL OR NON COMMERCIAL PROJECT

CAN'T:
YOU CAN'T RESELL OR DISTRIBUTE THE ASSET TO OTHERS
YOU CAN'T EDIT AND RESELL THE ASSET TO OTHERS

CREDITS:
CREDITS ARE REQUIRED. LINK TO https://limezu.itch.io/
```

Two things follow, and both are live obligations rather than notes:

- **The credit above must ship with every copy.** This file is what discharges
  that for the installer. It previously pointed at
  `src/renderer/src/assets/ATTRIBUTION.md`, which is source and is *not*
  packaged — so an installed copy carried a pointer to a file its user did not
  have, and no credit at all.
- **The tilesets must not be handed out as assets.** Using them to draw the app
  is exactly what the licence permits; serving the raw `.png` files from a web
  page is closer to distributing them, so the marketing site uses its own
  artwork rather than these files.

The map data (`assets/maps/*.tmj`) and the loader they came from are vendored
from [`shahar061/the-office`](https://github.com/shahar061/the-office), ISC.
