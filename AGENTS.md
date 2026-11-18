# SolidCraft Bearing Library

## Purpose

Maintain reusable, parametric bearing models for SolidCraft designs. These are
simplified bearing representations, not detailed internal assemblies or
manufacturing specifications. All dimensions are in millimetres.

Read `README.md` for the available sizes and current limitations. Keep it updated
when models are added, removed, or corrected.

## Files and naming

- Native designs use `.sc`; preserve their editable parameters and feature history.
- `rubber-seal/` holds rubber-sealed (`2RS`) bearing variants.
- The rubber-sealed template is `MR106-2RS - 10mmx6mm.sc` at the repository root.
- Other root-level bearing designs and the existing STEP export are separate
  assets; do not rename or regenerate them unless requested.
- New rubber-sealed files follow
  `<part number>-2RS - <outer diameter>x<inner diameter>x<width>.sc`.
  Example: `MR106-2RS - 10x6x3.sc`.
- Use decimal points and omit unnecessary trailing zeros in filenames.

## Generate variants

1. Use SolidCraft tools to open and edit designs. Do not directly rewrite the
   contents of `.sc` containers to generate variants.
2. Inspect the active design before replacing it; do not discard unrelated
   unsaved work. If file access is refused, request the required `--allow-path`
   configuration rather than bypassing the restriction.
3. Reopen the original template for every bearing. Never derive the next size
   from the previous generated variant.
4. Change `InnerDiameter`, `OuterDiameter`, and `Width` together, preferably in a
   coalesced parameter batch.
5. For `2RS` variants, use the supplied sealed/shielded width, not the open width.
   Ask when dimensions are missing or ambiguous; do not invent them.
6. Retain `RaceWidth = 0.5 mm` and `RaceGasket = Width - 0.25 mm` unless the user
   explicitly approves a change. Preserve expression-driven model dimensions.
7. Verify the rebuild before saving to the destination filename. Leave the
   original template unchanged and do not overwrite existing variants without
   checking whether replacement is intended.

## Verification

- Calculate expected dimensions and volume before generation.
- Check feature errors and warnings, body count, closed solids, bounding box,
  and volume. A successful command alone does not prove correct geometry.
- For the current template, X is the bearing axis. Expected bounds are
  `[-Width/2, -OuterDiameter/2, -OuterDiameter/2]` to
  `[Width/2, OuterDiameter/2, OuterDiameter/2]`, with one closed body.
- With distinct, non-crossing race circles, expected volume is
  `pi * ((R*R - r*r)*Width - ((R-t)**2 - (r+t)**2)*(Width-RaceGasket))`,
  where `R = OuterDiameter/2`, `r = InnerDiameter/2`, and `t = RaceWidth`.
- Confirm bore and outer radii, axis, and axial extents using cylindrical face
  topology; bounding dimensions alone cannot prove the bore exists.
- Confirm the saved filename and parameter expressions match the requested size.
- Stop on mismatches; do not save or report invalid geometry as complete.
- Report exactly what was verified, any omitted sizes, and any checks not run.
  Documentation-only changes need review, not new automated tests.

## Known exception: MR31

MR31 has approved dimensions of 1 mm ID, 3 mm OD, and 1.5 mm width, despite no
sealed width being listed in the supplied table. It has not been generated:
`RaceWidth = 0.5 mm` makes two template circles coincide and the rebuild loses
its bore. A proposed 0.25 mm race width has not been approved. Ask before changing
that parameter; do not silently generate a solid cylinder or claim MR31 exists.

## Repository hygiene

- Keep changes limited to the requested models and their documentation.
- Do not commit backups, lock files, `.DS_Store`, browser artifacts, credentials,
  or machine-specific `opencode.json` configuration.
- Use SSH for the Git remote:
  `git@github.com:jcustenborder/solidcraft-bearings.git`.
- The primary branch is `main`. Commit and push only when requested; do not
  force-push or discard unrelated work.
