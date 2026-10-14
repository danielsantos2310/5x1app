# 5x1 Interface Palette and Tokens (v0.1, for approval)

Source: the two design boards in `docs/internal/design/` (light and dark), reviewed 14 Oct 2026.
Status: proposed. Values marked **sampled** were read from pixels on the board images, so they are approximate until set in the design tool. Values marked **label** are printed on the board.

Scope: the 5x1 interface palette. IVI club colours (`ivi-brand.md`) are used only on club-branded pages and are not the interface base.

## Core palette

| Name | Hex | Source | Role |
| --- | --- | --- | --- |
| Navy | `#021C37` | label | Text on light, primary dark surfaces, secondary buttons |
| Green | `#09CD71` | label | Primary action (Vou, Entrar, Salvar), confirmed, accents |
| Orange | `#FE6D09` | label | Waitlist, maybe, warning |
| Flame | `#F84C04` | label | Cancelled, destructive, closed |
| Off-white | `#F8F8F5` | label | Light surface, text on dark |

## Light theme

| Token | Hex | Source | Use |
| --- | --- | --- | --- |
| `bg-page` | `#F8FAF7` | sampled | Page background |
| `bg-card` | `#F1F6F4` | sampled | Cards, list rows |
| `bg-surface` | `#FFFFFF` | derived | Sheets, inputs |
| `text-primary` | `#021C37` | label | Headings, body |
| `text-secondary` | `#5B6675` | derived | Captions, helper text |
| `action-primary-bg` | `#09CD71` | label | Primary button fill |
| `action-primary-text` | `#021C37` | derived | Text on green |
| `action-secondary-bg` | `#021C37` | label | Navy primary button fill |
| `action-secondary-text` | `#FFFFFF` | derived | Text on navy |
| `status-waitlist-bg` | `#FE6D09` | label | Waitlist chip fill |
| `status-waitlist-text` | `#021C37` | derived | Text on orange |
| `status-danger-bg` | `#F84C04` | label | Cancelled chip fill |
| `status-danger-text` | `#021C37` | derived | Text on flame |
| `border` | `#D9DDE3` | derived | Dividers, outlines |

## Dark theme

| Token | Hex | Source | Use |
| --- | --- | --- | --- |
| `bg-page` | `#021426` | sampled | Page background (darker than navy label) |
| `bg-card` | `#0A1725` | sampled | Cards, list rows |
| `bg-surface` | `#06233F` | derived | Sheets, inputs |
| `text-primary` | `#F8F8F5` | label | Headings, body |
| `text-secondary` | `#9AA4AF` | derived | Captions, helper text |
| `action-primary-bg` | `#09CD71` | label | Primary button fill (same as light) |
| `action-primary-text` | `#021C37` | derived | Text on green |
| `action-secondary-bg` | `#F8F8F5` | derived | Secondary button on dark |
| `action-secondary-text` | `#021C37` | derived | Text on off-white |
| `status-waitlist-bg` | `#FE6D09` | label | Waitlist chip fill |
| `status-waitlist-text` | `#021C37` | derived | Text on orange |
| `status-danger-bg` | `#F84C04` | label | Cancelled chip fill |
| `status-danger-text` | `#021C37` | derived | Text on flame |
| `border` | `#2A4560` | derived | Dividers, outlines |

## Contrast check (WCAG 2.2 AA)

Normal text needs 4.5:1; large text (24px, or 18.66px bold) needs 3:1.

| Pair | Ratio | Result |
| --- | --- | --- |
| Navy text on green | 8.16 | Pass. Use for all text on green |
| Navy text on orange | 6.05 | Pass |
| Navy text on flame | 4.92 | Pass |
| Off-white text on navy | 16.13 | Pass |
| Text secondary on light card (`#5B6675` on `#F1F6F4`) | 5.34 | Pass |
| Text secondary on dark card (`#9AA4AF` on `#0A1725`) | 7.14 | Pass |
| Green text on dark page | 8.83 | Pass (text may use green on dark only) |
| Orange text on dark card | 6.37 | Pass (dark theme only) |
| Flame text on dark card | 5.19 | Pass (dark theme only) |
| **White text on green** | **2.10** | **Fail. Do not use** |
| **White text on orange** | **2.84** | **Fail. Do not use** |
| **White text on flame** | **3.48** | **Fail for normal text.** Large text only |
| **Orange text on light page** | **2.84** | **Fail. Use navy text on orange instead** |
| **Green text on light page** | **2.10** | **Fail. Green is never text on light** |
| **Flame text on light page** | **3.48** | **Fail for normal text** |

Rules that follow:
1. Text on green, orange or flame is always navy.
2. On light surfaces, green, orange and flame are fills or icons only, never text.
3. Status is never colour alone: every chip has a label and an icon.

## Board review: items to decide before the design is final

These come from the board itself and conflict with the agreed scope or the spec. Nothing has been changed for them yet.

1. **pt-BR text on screens.** The board's screens show Portuguese copy ("Olá, João!", "Próximo treino", "Confirmado"). The agreed rule is English on screen, with pt-BR chosen in Settings. The boards need English copy (pt-BR can be shown as a translation check).
2. **Language switch on public screens.** The landing and header show a PT-BR / EN switch. The agreed rule is no language switch on public screens; the switch is in Settings.
3. **"Enviar lembrete" (send reminder)** on the admin event screen. Email and push reminders are outside v1 (see `product-spec.md`, deferred list). Keep it out of v1 or record it as an in-app notice (NOTE-01).
4. **"Marcar todos como presentes" (mark all present)** on attendance. Not in the spec. It fits the one-tap rule only if each row is still reviewable. Needs approval.
5. **Abbreviated attendance buttons** ("A", "At", "P"). Each button needs a full accessible name (Presente / Ausente / Atrasado) and visible text on the phone, not only a letter.
6. **"COURT-SIDE" tagline** and the 5x1 wordmark on the boards. The product name is 5x1 in the spec. Confirm whether COURT-SIDE is a tagline or a separate name before it appears anywhere.
7. **Typeface.** The board names Inter (or similar modern sans). The spec does not yet fix a typeface. Pick one and check it supports Portuguese accents.
8. **Dark ground.** The dark board uses `#021426` for the page, not the navy label `#021C37`. Decide whether the navy label is the page colour or only a surface colour.
