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
