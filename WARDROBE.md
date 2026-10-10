# Wardrobe — CharacterStudio (Tifa + bundled characters)

**This app is the wardrobe.** It is the asset/character management app and Create flow.
The live 3D interface is `apps/tifa-viewer` (the viewer) in the same playground.

## Layout (no symlinks — physical dirs only)

- Wardrobe app home: `/mnt/hybrid-apps/dev/playground/apps/CharacterStudio/`
  - Git repo: fork `BlackenedMist/CharacterStudio`, `origin` = that fork,
    `upstream` = `https://github.com/M3-org/CharacterStudio`.
  - Work-in-progress branch name chosen by user: `Nicks` (git commit/push was deferred on
    request — branch not yet created/pushed).
- Wardrobe contents: `/mnt/hybrid-apps/dev/playground/apps/CharacterStudio/public/loot-assets/`
  - Real directory (NOT a symlink). Moved here 2026-10-10 from `/mnt/storage/3dlib/loot-assets`.
  - Still its own git repo: origin/upstream `https://github.com/M3-org/loot-assets.git`, branch `main`.
  - Contains per-character packs: `tifa/` (untracked in that repo — new), `anata/`, `animations/`, icons.
  - `public/loot-assets` is gitignored in the wardrobe repo (2.6 GB / licensed assets stay out of GitHub).
- Tifa registration: `public/manifest.json` — now the ONLY character class (bundled
  Anata/ON1FORCE/TUBBY CAT entries removed 2026-10-10; `anata/male/` pack still on disk,
  orphaned). Portrait: `public/assets/portraitImages/tifa.jpg`.
- Tifa structure (restructured 2026-10-10: base skin + mix-and-match pieces):
  - `Body` trait (required) = base skin: `Body/Tifa_FullNude.vrm`
    (body+face+hair+eyes/lashes+earrings).
  - Piece traits (all optional → each shows a "remove/none" control in the Create flow):
    - `Top`: Crop Top (`Clothes/Tifa_Top.vrm`) | Bra Only (`Clothes/Tifa_Top_BraOnly.vrm`)
    - `Bottom`: Skirt (`Clothes/Tifa_Skirt.vrm`)
    - `Legs`: Boots & Stockings (`Clothes/Tifa_Legs.vrm`) — boots are NOT a separate mesh;
      they ride inside this one garment mesh.
    - `Arms`: Arm Warmers (`Clothes/Tifa_Arms.vrm`)
    - `Belt`: Materia Belt (`Clothes/Tifa_Belt.vrm`)
    - `Undies`: Underwear (`Clothes/Tifa_Undies.vrm`)
    - `Materia`: Materia (`Clothes/Tifa_Materia.vrm`)
    - `Pubes`: Pubes (`Clothes/Tifa_Pubes.vrm`) — from the HalfNude FBX; off by default.
  - Each piece VRM = exactly ONE garment mesh, sharing the FullNude skeleton, built via
    `/tmp/opencode/fbx_piece_to_vrm2.py` (FBX→VRM0, keep-one-mesh mode). All VRM 0.x,
    55 humanBones, zero missing.
  - Classic looks are now compositions of pieces (no combined VRMs needed):
    FullClothes = Top(Crop)+Bottom+Legs+Arms+Belt+Undies+Materia;
    Underwear = Top(BraOnly)+Undies+Arms+Legs+Materia; HalfNude = Arms+Legs+Materia+Pubes.
  - `initialTraits: [Body, Top, Bottom, Legs, Arms, Belt, Undies, Materia]`
    → default look = classic FullClothes over nude skin (Built piece-by-piece).
    `randomTraits` adds `Pubes` for full randomization.
  - Manifest: `public/loot-assets/tifa/manifest.json` (`assetsLocation: "/loot-assets/"`).
  - Old combined outfit VRMs (`Clothes/Tifa_FullClothes.vrm`, `Tifa_Underwear.vrm`,
    `Tifa_HalfNude.vrm`) are orphaned on disk (no longer in the manifest) — delete once
    the piece look is visually confirmed.
  - Next phase: `animationPath` references `./animations/1_T-Pose.fbx` … which do NOT exist
    under `loot-assets/tifa/` yet — populate before the create-flow animator can play clips.

## Serving

- Dev server: `cd apps/CharacterStudio && npm run dev` → `vite --host`, port **5174/5175**.
- Trait URLs resolve root-relative: e.g. `/loot-assets/tifa/Body/Tifa_FullNude.vrm` and
  per-piece `/loot-assets/tifa/Clothes/Tifa_Top.vrm` etc — real files under `public/`, no symlink.

## Roles

- Wardrobe (this app) = asset management + Create flow (VRM 0.x only import).
- Viewer (`apps/tifa-viewer`) = live interface for the player/dance work (Track A).