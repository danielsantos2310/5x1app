# 5x1 Brand Identity

Status: approved 14 Oct 2026. Internal, not published.
Source: `5x1-brand-identity-sheet.png` (the approved brand sheet, raster).

## Marks

| Mark | Use | Light file | Dark file |
| --- | --- | --- | --- |
| Primary logo (hexagon + "5x1") | Landing, sign-in, marketing and platform headers | `5x1-logo-light.svg` | `5x1-logo-dark.svg` |
| Standalone icon (hexagon only) | Small or square spaces: navigation, avatars, social, favicon source | `5x1-icon-light.svg` | `5x1-icon-dark.svg` |
| App icon | Home screen, install, store listings | `5x1-app-icon-light.svg` (white tile) | `5x1-app-icon-dark.svg` (navy tile, default) |

PNG exports of the app icon are in `app-icon/` at 512, 192 and 180 px (`5x1-app-icon-dark-512.png` and so on).

## Colour

| Name | Hex | Role |
| --- | --- | --- |
| Navy | #021C37 | Primary brand colour; navy parts of the mark on light backgrounds |
| Green | #09CD71 | Accent: the top facet of the hexagon and the "x". Same in light and dark |
| Off-white | #F8F8F5 | Navy parts of the mark on navy backgrounds |
| Orange | #FE6D09 | Optional secondary accent (see the design tokens) |

Palette tokens are in `docs/internal/design-tokens.md`. Green stays fixed in both themes. Only the navy parts change.

## Light and dark

- Light background: navy and green. Use the light file on white or the light page (#F8FAF7).
- Dark background: off-white and green. Use the dark file on navy (#021C37 or the dark page #021426).
- App icon: navy tile with off-white hexagon is the default. The white tile with navy hexagon is for light contexts.
- Never place a dark-mode mark on a light surface, or the reverse.

## Clear space and size

- Clear space on every side: at least one quarter of the logo height.
- Minimum size: primary logo 120 px wide on screen; standalone icon 24 px; app icon not below 48 px.
- Design the app icon at 512 px; check at 180 px and 48 px.

## Do not

- Recolour the green facet or the "x".
- Stretch, skew, rotate or add shadows, gradients or outlines.
- Put the mark on a busy photo or a colour that lacks contrast.
- Separate the hexagon from its green facet in the primary logo.
- Use the old text-only "5x1" wordmark.

## Where each mark goes

- Landing: the primary logo, centred above the tagline.
- Sign-in: the primary logo in the top bar, centred.
- Platform app icon: the dark app icon (default), with the light tile for light contexts.
- Club pages: the IVI crest, with IVI colours. The 5x1 mark does not replace the club brand.

## Open items

- The approved sheet is a raster image. The vector files were traced from it, so edges are close but not exact. Ask the designer for the vector source (SVG or Figma) and replace these files when it arrives.
- Android maskable icon and the favicon set are not generated yet. Keep the hexagon inside the central 80% safe zone when they are.
- COURT-SIDE remains an open question (see `design-tokens.md`). It is not part of this identity.
