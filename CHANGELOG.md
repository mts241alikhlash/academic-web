# academic-web

## 1.3.0

### Minor Changes

- 3a67c99: Sub-pages go back with `BackButton` from `@mts241alikhlash/ui` 1.3.1, left of the card title and labelled with where it leads, and breadcrumbs name the record a page is about instead of "Detail" or "Ubah"; long crumbs truncate. Another user's profile gets a back button and their name in the breadcrumb. The profile and address tabs use floating labels with every field tied to its label, including the birth date picker. Calendar management, the agenda form and add-student have the same back button, and the agenda form names its agenda.

### Patch Changes

- 3a67c99: Badges take `rounded-md` from `@mts241alikhlash/ui` 1.2.1. Fixes three checkboxes that never showed or changed state because they used the removed `checked` API: weekly holidays in Pengaturan and the select-all and row checkboxes in Kalender Akademik. The add-student and add-curriculum-subject dialogs and the enrollment history table use the shared `Checkbox` instead of native inputs. Skeletons keep their default radius, and the classroom picker in the teaching-assignment form uses the menu-item radius.

## 1.2.0

### Minor Changes

- 597423b: Search fields use `SearchInput` from `@mts241alikhlash/ui` 1.2.0: one icon, height and text size on every list, no zoom on iOS, and `DataTable`'s built-in filter follows it.

## 1.1.0

### Minor Changes

- 4325d84: Icons come from `@lucide/vue` (replacing the deprecated `lucide-vue-next`), with `@mts241alikhlash/ui` and `web-shared` 1.1.0.

## 1.0.1

### Patch Changes

- 4ec4f19: Update @mts241alikhlash/ui to 1.0.1.

## 1.0.0

### Major Changes

- First stable release.
