# Get Element Styles Helper

This document provides a JavaScript helper function that can be run in the browser console (like Chrome DevTools) to extract all CSS styles applied to a specific DOM element. 

It automatically calculates **CSS Specificity**, orders the matching rules from highest to lowest priority, and **comments out shadowed properties** (just like the DevTools UI does).

## The Helper Function

Copy and paste the following function into your browser's DevTools console:

```javascript
function getStyles(selector, includeComputed = false) {
  const el = document.querySelector(selector);
  
  if (!el) {
    console.error(`Element not found for selector: "${selector}"`);
    return;
  }

  // 1. Specificity Calculator
  function getSpecificity(sel) {
    const id = (sel.match(/#[a-zA-Z0-9_-]+/g) || []).length;
    const cls = (sel.match(/\.[a-zA-Z0-9_-]+/g) || []).length
              + (sel.match(/\[[^\]]+\]/g) || []).length
              + (sel.match(/(?<!:):[a-zA-Z0-9_-]+/g) || []).length;
    const tag = (sel.match(/(^|[\s>+~]+)([a-zA-Z]+)/g) || []).length
              + (sel.match(/::[a-zA-Z0-9_-]+/g) || []).length;
    // Approximation mapping to a sortable integer
    return id * 10000 + cls * 100 + tag;
  }

  let matchedRules = [];
  let sheetIndex = 0;

  // 2. Collect all matched rules
  for (let sheet of document.styleSheets) {
    sheetIndex++;
    try {
      let ruleIndex = 0;
      for (let rule of sheet.cssRules) {
        ruleIndex++;
        if (rule.type === CSSRule.STYLE_RULE && el.matches(rule.selectorText)) {
          // If comma-separated selector, find the highest specificity among matching parts
          const selectors = rule.selectorText.split(',').map(s => s.trim());
          const matchingSelectors = selectors.filter(s => {
            try { return el.matches(s); } catch(e) { return false; }
          });
          if (matchingSelectors.length === 0) continue;
          
          const maxSpec = Math.max(...matchingSelectors.map(getSpecificity));
          matchedRules.push({
            source: sheet.href ? sheet.href.split('/').pop() : 'Inline/Injected <style>',
            selector: rule.selectorText,
            style: rule.style,
            specificity: maxSpec,
            sheetIndex,
            ruleIndex
          });
        }
      }
    } catch (e) {
      // Browser security prevents reading Cross-Origin stylesheets
    }
  }

  // 3. Sort Rules: Specificity (High to Low) -> Sheet Index (High to Low) -> Rule Index (High to Low)
  matchedRules.sort((a, b) => {
    if (a.specificity !== b.specificity) return b.specificity - a.specificity;
    if (a.sheetIndex !== b.sheetIndex) return b.sheetIndex - a.sheetIndex;
    return b.ruleIndex - a.ruleIndex;
  });

  let result = `/* === Style Definitions for: "${selector}" === */\n\n`;
  let seenProps = new Set();
  let importantProps = new Set();

  // Helper to format rule blocks and comment out shadowed properties
  function processStyle(styleDecl, sourceLabel, blockHeader) {
    if (!styleDecl.length) return '';
    let block = `/* Source: ${sourceLabel} */\n${blockHeader} {\n`;
    let hasProps = false;
    
    for (let i = 0; i < styleDecl.length; i++) {
      let prop = styleDecl[i];
      let value = styleDecl.getPropertyValue(prop);
      let isImportant = styleDecl.getPropertyPriority(prop) === 'important';
      
      let shadowed = false;
      if (importantProps.has(prop)) {
         shadowed = true; // Overridden by a previous !important rule
      } else if (seenProps.has(prop) && !isImportant) {
         shadowed = true; // Overridden by a higher/equal specificity rule
      }

      if (shadowed) {
        // Comment out shadowed properties
        block += `  /* ${prop}: ${value}${isImportant ? ' !important' : ''}; */\n`;
      } else {
        // Active property
        block += `  ${prop}: ${value}${isImportant ? ' !important' : ''};\n`;
        seenProps.add(prop);
        if (isImportant) importantProps.add(prop);
      }
      hasProps = true;
    }
    block += `}\n\n`;
    return hasProps ? block : '';
  }

  // 4. Output Inline Styles First (Highest Priority)
  if (el.style.length > 0) {
    result += `/* --- Inline Styles --- */\n`;
    result += processStyle(el.style, 'element.style attribute', 'element.style');
  }

  // 5. Output Matched Rules
  result += `/* --- Matched CSS Rules (Ordered by Specificity) --- */\n`;
  if (matchedRules.length === 0) {
    result += `/* (No matched rules found in accessible stylesheets) */\n\n`;
  } else {
    for (let r of matchedRules) {
      result += processStyle(r.style, `${r.source} (Specificity: ${r.specificity})`, r.selector);
    }
  }

  // 6. Output Computed Styles (if requested)
  if (includeComputed) {
    result += `/* --- Computed Styles (All final values) --- */\n${selector} {\n`;
    const computed = window.getComputedStyle(el);
    for (let i = 0; i < computed.length; i++) {
      result += `  ${computed[i]}: ${computed.getPropertyValue(computed[i])};\n`;
    }
    result += `}\n\n`;
  }

  // Auto-copy to clipboard in DevTools
  if (typeof copy === 'function') {
    copy(result);
    console.log(`%c✓ Styles copied to clipboard!`, 'color: #00e676; font-weight: bold; font-size: 14px;');
  }

  console.log(result);
  return "Done!";
}
```

## How to Use It

Once the function is defined in your console, you can call it with any valid CSS selector.

### 1. Get Written Rules (Default)
This extracts only the CSS rules that were explicitly written in your stylesheets or inline styles.

```javascript
getStyles('.my-class-name')
getStyles('#my-id')
getStyles('div > p:first-child')
```

### 2. Get Written Rules + Computed Styles
If you pass `true` as the second argument, the output will also append the browser's final **Computed Styles**. 
*Note: Computed styles contain over 300 properties including all browser defaults, exact pixel calculations, and inherited values.*

```javascript
getStyles('.my-class-name', true)
```

## Features
* **Specificity Engine:** Identifies IDs, Classes, Attributes, Pseudo-classes, and HTML Tags to accurately rank the matching rules from Highest Priority to Lowest Priority.
* **Shadowing:** Just like Chrome DevTools, if a property is overridden by a higher specificity rule or an `!important` flag, it wraps the line in a CSS comment `/* ... */`.
* **Auto-copy:** When run inside Chrome DevTools, it utilizes the native `copy()` command to automatically copy the formatted output directly to your clipboard.
* **Source Tracking:** It displays exactly which stylesheet file (`.css`) the matched rule came from.