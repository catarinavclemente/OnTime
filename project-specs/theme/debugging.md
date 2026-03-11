# Debugging

Now I understand the full picture:

1. `.complex-filter-box__filter-box` is **absolutely positioned** (`position: absolute`) - it's a dropdown panel
2. When open, `.complex-filter-box.-open` gets `z-index: 1`
3. The filter box inside gets `z-index: -1` (`.complex-filter-box.-open .complex-filter-box__filter-box { display: block; z-index: -1; }`)
4. Filter items inside start with `opacity: .5` and become `opacity: 1` when enabled

The user's style.scss override:

The CSS selector targets the fieldgroup and form-group elements inside the open filter box and forces them to use `position: relative` with `z-index: 1`, which stacks them above the parent's negative z-index and adds a light gray background to make them visible.

The real problem emerges from nested stacking contexts: the operator select dropdown inside each fieldgroup inherits `z-index: 1` from the ECL library, so when fieldgroups stack vertically, the dropdown from one fieldgroup can visually overlap the content below it because they're all at the same stacking level. z-index 1, position relative) creates a stacking context, and within it, the `.ecl-select` (z-index 1) paints above the icon (z-index 0). But the `.ecl-select__container` has `position: relative` and the select extends to the full width of the container. The visual overlap happens because the select element (with its border, padding etc.) sits in a z-index context that, combined with the parent stacking contexts, causes it to render on top of adjacent elements.

Actually wait, I need to think more carefully. If each form group (`.fieldgroup`, `.ecl-form-group`) has `position: relative; z-index: 1`, they all have the SAME z-index. In CSS, when elements have the same z-index and position, they stack in source order (later = on top). So the LAST form group renders on top of the first.

Now I'm wondering whether the operator select is positioned to overflow outside its fieldgroup boundaries into adjacent filter elements. The fieldgroup itself has `overflow: visible`, but the parent filter-box container has `overflow: auto` with a max-height constraint, so any overflow from the select would be clipped at that level rather than spilling into neighboring form groups. So the key issue is that `<select>` elements are treated specially by browsers—they often get implicit stacking contexts even without explicit positioning, which means the `z-index: 1` on `.ecl-select` actually takes effect and can cause it to layer above adjacent form groups.

The user highlighted my comment about resetting this with `z-index: auto`, but they're pointing out that the select is still overlapping the adjacent filter elements. I need to figure out if my fix is actually being applied or if there's something else going on with how the stacking context is being established. So they're looking at my fix and saying the operator should not overlap. They might be asking me to make it more targeted to just the operator select, or verify it actually works—maybe `z-index: auto` isn't enough and I need `!important` to override other specificity issues.

Looking at the CSS, my rule has higher specificity than the ECL base rule, so it should work. But I'm realizing the real issue: each fieldgroup gets `position: relative` and `z-index: 1`, which creates separate stacking contexts. Within those contexts, the select's z-index doesn't matter for overlap between adjacent filter groups—the stacking order is determined by the fieldgroups' own z-index values and source order instead.

Since all fieldgroups have the same z-index, later elements in the DOM paint on top of earlier ones naturally. But that shouldn't cause visual overlap unless elements are actually positioned to overlap. The operator select must be using `position: relative` or have styling like box-shadow that extends beyond its box model, causing it to visually overlap with adjacent elements.

Looking at the ECL select styles, there's just a 1px border and no box-shadow, so that's not it. I think the real issue is that the z-index on the select creates a stacking context within the scrollable filter box, forcing it to paint above the next field's content even though they don't physically overlap in layout terms. This is a common issue with form controls where the visual stacking doesn't match the DOM order.

My `z-index: auto` fix should resolve this, but I'm wondering if I should also add `pointer-events: none` to the icon to prevent any click interception issues now that the select itself won't have the elevated z-index. Let me verify this approach handles both the visual stacking problem and maintains proper interaction behavior.

Analyzing...<br>

\
Let me investigate the complex-filter-box structure and how the operator select is positioned to understand the overlap issue better.

**grep -rn 'complex-filter-box' lib/themes/emn\_theme/styles/ --include="\*.scss"**

*
*

**grep -rn 'complex-filter-box' lib/ web/modules/ web/themes/ --include="\*.css" --include="\*.scss" --include="\*.js" -l 2>/dev/null | head -20**

*
*

cd /Users/catarina/REACT/just-ec-react-dev && python3 -c "\
with open('web/themes/contrib/oe\_theme/dist/ec/styles/ecl-ec.css') as f:\
css = f.read()\
blocks = css.split('}')\
for b in blocks:\
if 'ecl-select\_\_icon' in b and ('pointer' in b or 'z-index' in b or 'position' in b):\
print(b.strip() + '}')\
print('---')\
"

*
*

cd /Users/catarina/REACT/just-ec-react-dev && python3 -c "\
with open('web/themes/contrib/oe\_theme/dist/ec/styles/ecl-ec.css') as f:\
css = f.read()\
blocks = css.split('}')\
for b in blocks:\
if 'ecl-select\_\_container' in b:\
print(b.strip() + '}')\
print('---')\
"

*
*

Now I see the full picture. The `.ecl-select__container` has `position: relative` and the icon has `position: absolute; z-index: 0`. The `z-index: 1` on `.ecl-select` keeps the select above the icon. Simply setting `z-index: auto` could let the icon overlay the select and block clicks. Let me refine the fix:

Read [select.html.twig](https://vscode-file/vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html), lines 170 to 242

The icon div is a purely decorative element (just shows an arrow icon). It doesn't have any click handlers. The `<select>` element itself extends under the icon area (via padding-right/padding-inline-end to leave space for the icon). So the select should still receive clicks even if the icon is painted above it, because the select extends behind the icon.

Actually, let me re-read the ECL rule: `.ecl-select__icon` has `position: absolute; right: 0; top: 0; width: 2.75rem; height: 100%; z-index: 0`. The icon div sits absolutely positioned over the right side of the select.

The select has padding on the right side to visually accommodate the icon, but since the icon is absolutely positioned with a z-index of 0 and sits on top of the select element, it would normally intercept clicks in that area. However, native select elements in browsers have special handling that might bypass this layering issue.

To ensure the icon never blocks clicks, I could either set `pointer-events: none` on the icon, or give it a negative z-index so it renders beneath the select while remaining visible through the select's transparent padding area. Let me think through whether the select actually has a background that would hide the icon if it's positioned below. The stacking context is getting tangled here. I think the cleanest solution is to reset the select's z-index to auto and disable pointer events on the icon so it doesn't intercept clicks. I'm writing out the SCSS fix with a comment explaining why we need to neutralize ECL's z-index behavior.

Actually, let me reconsider the root cause—it might not be the select's z-index at all, but rather how the filter box creates its own stacking context when opened. The real issue is probably between different filter box instances—when one opens and drops down with absolute positioning, it can overlap the next box's header. The operator select the user mentioned is just a sibling within a single fieldgroup, so I think I've been overcomplicating this. Let me strengthen the fix to handle the stacking more robustly.

Refining the CSS approach...
