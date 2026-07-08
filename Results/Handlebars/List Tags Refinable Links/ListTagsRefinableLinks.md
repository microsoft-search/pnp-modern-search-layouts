# List Tags Refinable Links #

This sample is a new variant of the list tag layouts. It keeps the tag-pill presentation, but turns each pill into a link that opens a Search Results page with a Search Filters refiner already selected.

It does not replace the existing **List Tags Advanced options** sample. That sample should remain as-is. This variant is an additional option for scenarios where the tags should act as refiner links.

<img width="608" height="380" alt="Skærmbillede 2026-05-03 152312" src="https://github.com/user-attachments/assets/876a4993-f7e1-492e-ba78-128c4b47d49c" />


## What this variant does

- renders tags as pills
- makes each pill clickable
- opens a target search page with a preselected refiner
- can also preserve a vertical by including a query parameter such as `v=Cards`

This is possible because PnP Modern Search supports Search Filters deep links and provides the `stringToHex` Handlebars helper to build the encoded filter value.

## Important limitation

This sample is still not fully generic.

In practice, you must still specify:

- the page URL to open
- the Search Filters web part instance ID used on that page

The dynamic `f_{{@root.filters.instanceId}}` pattern can work for reusable templates when the Search Results template receives the connected Filters instance ID. However, that is not reliable in every page setup. For this sample, treat the layout as page-specific and replace both the page URL and the Search Filters instance ID with values from your own page.

## How the deep link works

The Search Filters web part stores selected filters in a query string parameter named `f_<Search Filters instance ID>`.

For plain string refiners, each selected value uses the following structure:

- `name`: the label displayed in the filter UI
- `value`: the same label encoded as UTF-8 hex, prefixed with `ǂǂ`, then wrapped in escaped quotes
- `operator`: typically `0`

The safest way to produce a working URL is:

1. Select the filter once in the UI.
1. Copy the generated URL.
1. Keep the page URL, the `f_<instanceId>` parameter name, and the JSON payload structure.
1. Replace only the `name` and encoded `value` parts in the template.

## Example

The sample layout file is [List Tags Refinable Links](./List%20Tags%20Refinable%20Links.html).

Before using it, replace all occurrences of these placeholders:

- `__TARGET_PAGE_URL__` with your target page URL
- `__FILTERS_INSTANCE_ID__` with your Search Filters web part instance ID
- `RefinableString109` with the managed property used by your filter

If your page depends on a selected vertical, keep or adjust the example `v=Cards` query parameter.

## Notes

- This example intentionally uses a page-specific link.
- If you use taxonomy refiners instead of plain string refiners, the filter `value` format is different and must use the taxonomy token format from a generated Search Filters URL.
- When possible, copy a working URL from the UI instead of hand-authoring the full payload.
