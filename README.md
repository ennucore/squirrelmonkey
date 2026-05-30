# squirrelmonkeyskull.com

Interactive 3D viewer comparing a squirrel monkey skull (*Saimiri sciureus*)
with a human skull. Static `index.html` using [three.js](https://threejs.org/).

## Hosting

- **Site:** Cloudflare Pages project `squirrelmonkeyskull` → https://squirrelmonkeyskull.com
- **Models:** Cloudflare R2 bucket `squirrelmonkey-assets`, served from
  `https://assets.squirrelmonkeyskull.com`

`index.html` loads the Draco-compressed `.glb` models and the texture from that
R2 domain at runtime.

## Assets

| File | Notes |
| --- | --- |
| `assets/squirrel_mokey_skull.draco.glb` | Squirrel monkey skull, Draco-compressed (53 MB OBJ → 2.1 MB) |
| `assets/HumanSkull.draco.glb` | Human skull, Draco-compressed (156 MB OBJ → 1.8 MB) |
| `textures/squirrel monkey 357.06.png` | Photogrammetry texture for the squirrel skull |
| `*.obj` | Original raw scans (Git LFS) |

`.glb` and `.obj` are tracked with Git LFS.

### Regenerating the Draco models

```sh
npx obj2gltf -i Model.obj -o Model.glb -b
npx gltf-pipeline -i Model.glb -o Model.draco.glb -d --draco.compressionLevel 10
```

Then upload to R2:

```sh
npx wrangler r2 object put squirrelmonkey-assets/Model.draco.glb \
  --file Model.draco.glb --content-type model/gltf-binary --remote
```

## Deploy

Currently deployed manually:

```sh
npx wrangler pages deploy . --project-name squirrelmonkeyskull --branch master
```
