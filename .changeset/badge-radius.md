---
'academic-web': patch
---

Badges take `rounded-md` from `@mts241alikhlash/ui` 1.2.1. Fixes three checkboxes that never showed or changed state because they used the removed `checked` API: weekly holidays in Pengaturan and the select-all and row checkboxes in Kalender Akademik. The add-student and add-curriculum-subject dialogs and the enrollment history table use the shared `Checkbox` instead of native inputs. Skeletons keep their default radius, and the classroom picker in the teaching-assignment form uses the menu-item radius.
