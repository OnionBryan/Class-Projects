# Quick Fixes - Priority Action Items

This document provides actionable fixes for the most critical issues found in the audit.

## 🔴 P0: CRITICAL - Fix Immediately (2-4 hours)

### 1. Security: Fix External Links
**Affected files:** All HTML files

**Find:** All instances of `target="_blank"` without security attributes
**Replace with:**
```html
<!-- OLD -->
<a href="https://github.com/onionbryan" target="_blank">GitHub Profile</a>

<!-- NEW -->
<a href="https://github.com/onionbryan" target="_blank" rel="noopener noreferrer">GitHub Profile</a>
```

**Locations to fix:**
- index.html: lines ~1117-1119
- septindex.html: lines 1117-1119
- Other external links throughout

### 2. Security: Add Content Security Policy
**Affected files:** All HTML files

**Add to `<head>` section:**
```html
<meta http-equiv="Content-Security-Policy"
      content="default-src 'self'; style-src 'self' 'unsafe-inline'; script-src 'self' 'unsafe-inline'; frame-src https://onionbryan.github.io;">
```

### 3. Security: Secure the iframe
**Location:** index.html line 1178

**Change from:**
```html
<iframe src="https://onionbryan.github.io/Class-Projects/index2.html"
        style="width: 100%; height: calc(100vh - 80px); border: none;"></iframe>
```

**To:**
```html
<iframe src="https://onionbryan.github.io/Class-Projects/index2.html"
        style="width: 100%; height: calc(100vh - 80px); border: none;"
        sandbox="allow-scripts allow-same-origin"
        title="Additional Presentation Content"></iframe>
```

---

## 🟠 P1: HIGH Priority - Fix This Week (8-12 hours)

### 4. Accessibility: Add ARIA Labels to Tab Buttons
**Location:** All files with tab navigation

**Change from:**
```html
<button class="tab-btn active" onclick="switchTab('presentation1', this)">
    Judge & Piccolo (2004)
</button>
```

**To:**
```html
<button class="tab-btn active"
        onclick="switchTab('presentation1', this)"
        role="tab"
        aria-selected="true"
        aria-controls="presentation1"
        id="tab-presentation1">
    Judge & Piccolo (2004)
</button>
```

Also add to tab containers:
```html
<div class="tab-nav" role="tablist" aria-label="Presentation navigation">
    <!-- buttons here -->
</div>

<div id="presentation1" class="tab-pane active" role="tabpanel" aria-labelledby="tab-presentation1">
    <!-- content here -->
</div>
```

### 5. Accessibility: Fix Form Labels
**Location:** All calculator sliders

**Change from:**
```html
<label>Transformational Behaviors (0-100%): <span id="transValue">50</span>%</label>
<input type="range" class="slider" id="transSlider" min="0" max="100" value="50" oninput="updateCalculator()">
```

**To:**
```html
<label for="transSlider">Transformational Behaviors (0-100%): <span id="transValue">50</span>%</label>
<input type="range"
       class="slider"
       id="transSlider"
       name="transSlider"
       min="0"
       max="100"
       value="50"
       oninput="updateCalculator()"
       aria-valuenow="50"
       aria-valuemin="0"
       aria-valuemax="100"
       aria-label="Transformational leadership behavior percentage">
```

### 6. Accessibility: Add Table Captions
**Location:** All tables throughout files

**Change from:**
```html
<table>
    <tr>
        <th>Leadership Type</th>
        <th>Mean Reliability</th>
    </tr>
```

**To:**
```html
<table>
    <caption>Reliability Values Used in Meta-Analysis</caption>
    <thead>
        <tr>
            <th scope="col">Leadership Type</th>
            <th scope="col">Mean Reliability</th>
        </tr>
    </thead>
    <tbody>
        <!-- data rows -->
    </tbody>
</table>
```

### 7. Accessibility: Add Skip Link
**Location:** All HTML files, immediately after `<body>` tag

**Add:**
```html
<body>
    <a href="#main-content" class="skip-link">Skip to main content</a>
    <!-- rest of content -->
```

**Add CSS:**
```css
.skip-link {
    position: absolute;
    top: -40px;
    left: 0;
    background: #000;
    color: white;
    padding: 8px;
    text-decoration: none;
    z-index: 100;
}

.skip-link:focus {
    top: 0;
}
```

**Add id to main content:**
```html
<div class="container" id="main-content">
```

### 8. Semantic HTML: Use HTML5 Elements
**Location:** Throughout all files

**Common replacements:**
```html
<!-- OLD -->
<div class="header">
    <h1>Title</h1>
</div>

<!-- NEW -->
<header class="header">
    <h1>Title</h1>
</header>

<!-- OLD -->
<div class="container">
    <div class="section">Content</div>
</div>

<!-- NEW -->
<main class="container">
    <section class="section">Content</section>
</main>
```

---

## 🟡 P2: MEDIUM Priority - Next Sprint (16-24 hours)

### 9. Performance: Consolidate Files
**Action:**
1. Keep only `index.html` as the main file
2. Create `/archive` directory
3. Move old versions:
   ```bash
   mkdir archive
   git mv septindex.html archive/
   git mv septindex2.html archive/
   git mv Bryan/2004 archive/
   ```

### 10. Code Quality: Extract External CSS
**Steps:**
1. Create `css/styles.css`
2. Copy all CSS from `<style>` tags
3. Replace `<style>` section with:
   ```html
   <link rel="stylesheet" href="css/styles.css">
   ```

### 11. Code Quality: Extract External JS
**Steps:**
1. Create `js/tabs.js`
2. Create `js/calculator.js`
3. Move functions to respective files
4. Replace `<script>` section with:
   ```html
   <script src="js/tabs.js"></script>
   <script src="js/calculator.js"></script>
   ```

### 12. Code Quality: Remove Inline Event Handlers
**Find all:**
- `onclick="..."`
- `oninput="..."`
- `onchange="..."`

**Replace with event listeners in JS:**
```javascript
// In your external JS file
document.addEventListener('DOMContentLoaded', function() {
    // Tab buttons
    document.querySelectorAll('.tab-btn').forEach(btn => {
        btn.addEventListener('click', function() {
            switchTab(this.dataset.tab, this);
        });
    });

    // Sliders
    document.getElementById('transSlider')?.addEventListener('input', updateCalculator);
    document.getElementById('rewardSlider')?.addEventListener('input', updateCalculator);
    document.getElementById('laissezSlider')?.addEventListener('input', updateCalculator);

    // Select
    document.getElementById('criterionSelect')?.addEventListener('change', updateComparison);
});
```

---

## 🟢 P3: LOW Priority - Future Improvements

### 13. Documentation: Add README
**Create:** `README.md`

**Template:**
```markdown
# Leadership Meta-Analysis Interactive Presentation

Interactive visualization of Judge & Piccolo (2004) meta-analysis on transformational vs. transactional leadership.

## Features
- Interactive data visualizations
- Leadership effectiveness calculator
- Comprehensive meta-analysis breakdown

## Setup
1. Clone repository
2. Open `index.html` in browser
3. No build process required

## Technologies
- HTML5
- CSS3
- Vanilla JavaScript

## Author
Bryan - [GitHub](https://github.com/onionbryan)

## License
MIT License © 2025 Bryan
```

### 14. Add Build Process (Optional)
**Setup:**
```bash
npm init -y
npm install --save-dev clean-css-cli terser html-minifier
```

**Create:** `package.json` scripts:
```json
{
  "scripts": {
    "build:css": "cleancss -o dist/styles.min.css css/styles.css",
    "build:js": "terser js/*.js -o dist/bundle.min.js",
    "build:html": "html-minifier --collapse-whitespace --remove-comments -o dist/index.html index.html",
    "build": "npm run build:css && npm run build:js && npm run build:html"
  }
}
```

---

## Testing Commands

### Accessibility Testing
```bash
# Install tools
npm install -g @axe-core/cli pa11y

# Run tests
axe https://onionbryan.github.io/Class-Projects/
pa11y https://onionbryan.github.io/Class-Projects/
```

### Lighthouse Audit
```bash
# Install
npm install -g lighthouse

# Run
lighthouse https://onionbryan.github.io/Class-Projects/ --view
```

### HTML Validation
Visit: https://validator.w3.org/

---

## Quick Checklist

Use this checklist to track your progress:

### P0 - Critical (Do First)
- [ ] Add `rel="noopener noreferrer"` to external links
- [ ] Add Content Security Policy meta tag
- [ ] Add `sandbox` and `title` to iframe

### P1 - High Priority
- [ ] Add ARIA labels to all interactive elements
- [ ] Fix form input labels with `for` attribute
- [ ] Add table captions and scope attributes
- [ ] Add skip navigation link
- [ ] Convert divs to semantic HTML5 elements

### P2 - Medium Priority
- [ ] Move old files to archive
- [ ] Extract CSS to external file
- [ ] Extract JavaScript to external files
- [ ] Remove inline event handlers
- [ ] Add error handling to JS functions

### P3 - Low Priority
- [ ] Create README.md
- [ ] Add code comments
- [ ] Set up build process
- [ ] Add print styles
- [ ] Implement structured data (schema.org)

---

## Support

For questions about fixes:
1. Check the full AUDIT_REPORT.md
2. Review WCAG 2.1 guidelines: https://www.w3.org/WAI/WCAG21/quickref/
3. Test with browser DevTools

---

**Last Updated:** 2025-11-07
**Next Review:** After P0-P1 fixes implemented
