# Live Code Editor Implementation Plan

## Overview
Replace the static HTML code block in Week 0 with an interactive, live code editor that allows students to write HTML and see real-time results.

## Current Implementation Target
**Location**: `/src/pages/weeks/week-0.astro`
**Section**: Part 3: Your First HTML (11:50-12:15)

**Current static code**:
```html
<!DOCTYPE html>
<html>
<head>
    <title>My First Page</title>
</head>
<body>
    <h1>Hello, I'm [your name]</h1>
    <p>This is my first HTML file in DESN368!</p>
</body>
</html>
```

## Implementation Options

### Option 1: CodeMirror-based Editor (Full Featured)
**Pros:**
- Professional code editor experience
- Advanced features (line numbers, code folding, search/replace)
- Excellent syntax highlighting and error detection
- Widely used in developer tools

**Cons:**
- Larger bundle size (~200KB)
- More complex setup and configuration
- Might be overkill for beginner HTML

**Implementation:**
- Install CodeMirror 6
- Configure HTML mode with syntax highlighting
- Add real-time preview iframe
- Implement resizable panels

### Option 2: Simple Textarea + Live Preview (Recommended)
**Pros:**
- Matches site's clean, minimalist design
- Lightweight and fast loading
- Easy to customize and style
- Focus on learning fundamentals
- Better for beginners

**Cons:**
- Less advanced editor features
- Basic syntax highlighting only

**Implementation:**
- Custom textarea with monospace font
- Prism.js for lightweight syntax highlighting
- Real-time iframe preview
- Clean, side-by-side or stacked layout

### Option 3: CodePen Embed
**Pros:**
- Zero implementation time
- Professional platform
- Students can save and share work
- Built-in community features

**Cons:**
- External dependency
- Requires internet connection
- Less control over user experience
- Doesn't match site design

## Recommended Implementation: Option 2

### Design Specification
Based on the site's current design language:

**Layout:**
```
┌─────────────────────────────────────────┐
│ Part 3: Your First HTML                 │
│ We'll create this together in class:    │
├─────────────────┬───────────────────────┤
│ HTML Editor     │ Live Preview          │
│                 │                       │
│ <!DOCTYPE html> │ ┌─────────────────┐   │
│ <html>          │ │ Hello, I'm...   │   │
│ <head>          │ │                 │   │
│   <title>...    │ │ This is my...   │   │
│ </head>         │ │                 │   │
│ <body>          │ └─────────────────┘   │
│   <h1>Hello...  │                       │
│   <p>This is... │                       │
│ </body>         │                       │
│ </html>         │                       │
└─────────────────┴───────────────────────┘
```

### Technical Implementation

#### 1. HTML Structure
```html
<div class="live-code-editor">
  <div class="live-code-header">
    <h3>Interactive HTML Editor</h3>
    <div class="live-code-actions">
      <button class="btn-copy">Copy Code</button>
      <button class="btn-reset">Reset</button>
    </div>
  </div>

  <div class="live-code-container">
    <div class="code-editor-panel">
      <label for="html-editor">HTML Code</label>
      <textarea
        id="html-editor"
        class="code-textarea"
        spellcheck="false"
        placeholder="Write your HTML here...">
      </textarea>
    </div>

    <div class="preview-panel">
      <label>Live Preview</label>
      <iframe
        id="preview-frame"
        class="preview-iframe"
        sandbox="allow-same-origin">
      </iframe>
    </div>
  </div>
</div>
```

#### 2. CSS Styling
```css
.live-code-editor {
  border: 1px solid var(--border-light);
  border-radius: 8px;
  background: var(--bg-card);
  margin: 2rem 0;
}

.live-code-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  border-bottom: 1px solid var(--border-light);
  background: var(--bg-tertiary);
}

.live-code-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1px;
  background: var(--border-light);
}

.code-editor-panel,
.preview-panel {
  background: var(--bg-card);
  padding: 1rem;
}

.code-textarea {
  width: 100%;
  height: 300px;
  font-family: var(--font-mono);
  font-size: 14px;
  line-height: 1.5;
  border: 1px solid var(--border-light);
  border-radius: 4px;
  padding: 1rem;
  resize: vertical;
  background: #f8f9fa;
}

.preview-iframe {
  width: 100%;
  height: 300px;
  border: 1px solid var(--border-light);
  border-radius: 4px;
  background: white;
}

/* Responsive */
@media (max-width: 768px) {
  .live-code-container {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto;
  }
}
```

#### 3. JavaScript Functionality
```javascript
class LiveCodeEditor {
  constructor(editorId, previewId) {
    this.editor = document.getElementById(editorId);
    this.preview = document.getElementById(previewId);
    this.defaultCode = this.getDefaultHTML();

    this.init();
  }

  init() {
    // Set default code
    this.editor.value = this.defaultCode;

    // Update preview on input
    this.editor.addEventListener('input', () => {
      this.updatePreview();
    });

    // Initial preview
    this.updatePreview();
  }

  updatePreview() {
    const html = this.editor.value;
    const previewDoc = this.preview.contentDocument;

    previewDoc.open();
    previewDoc.write(html);
    previewDoc.close();
  }

  getDefaultHTML() {
    return `<!DOCTYPE html>
<html>
<head>
    <title>My First Page</title>
</head>
<body>
    <h1>Hello, I'm [your name]</h1>
    <p>This is my first HTML file in DESN368!</p>
</body>
</html>`;
  }

  copyCode() {
    navigator.clipboard.writeText(this.editor.value);
  }

  reset() {
    this.editor.value = this.defaultCode;
    this.updatePreview();
  }
}

// Initialize when page loads
document.addEventListener('DOMContentLoaded', () => {
  const editor = new LiveCodeEditor('html-editor', 'preview-frame');

  // Add button functionality
  document.querySelector('.btn-copy')?.addEventListener('click', () => {
    editor.copyCode();
  });

  document.querySelector('.btn-reset')?.addEventListener('click', () => {
    editor.reset();
  });
});
```

#### 4. Enhanced Features (Optional)

**Syntax Highlighting with Prism.js:**
```html
<!-- Add to BaseLayout.astro -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism.min.css" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/components/prism-markup.min.js"></script>
```

**Line Numbers:**
```css
.code-textarea {
  background-image: linear-gradient(
    transparent 0px,
    transparent 19px,
    var(--border-light) 20px
  );
  background-size: 100% 20px;
  line-height: 20px;
}
```

### Integration Steps

1. **Replace static code block** in `/src/pages/weeks/week-0.astro`
2. **Add CSS** to `/src/styles/styles.css`
3. **Create JavaScript module** `/src/scripts/live-code-editor.js`
4. **Import script** in the week-0 page
5. **Test responsive behavior** on mobile devices

### Benefits for Students

- **Immediate feedback** - See results as they type
- **Experimentation** - Safe environment to try changes
- **Error learning** - Broken HTML shows immediate visual feedback
- **Engagement** - Interactive vs. passive reading
- **Accessibility** - Copy/paste code for their own use

### Accessibility Considerations

- **Keyboard navigation** support
- **Screen reader** compatible labels
- **High contrast** code editor theme option
- **Adjustable font sizes**
- **Focus indicators** for all interactive elements

### Performance Considerations

- **Lazy loading** - Only initialize when section is visible
- **Debounced updates** - Prevent excessive preview updates
- **Lightweight dependencies** - Minimal external libraries
- **Efficient DOM updates** - Use iframe sandboxing for security

## Implementation Priority

1. **Phase 1**: Basic textarea + live preview
2. **Phase 2**: Add copy/reset functionality
3. **Phase 3**: Implement syntax highlighting
4. **Phase 4**: Enhanced UX features (line numbers, themes)
5. **Phase 5**: Mobile optimization and testing

## Success Metrics

- **Student engagement** - Time spent interacting with editor
- **Learning outcomes** - Understanding of HTML structure
- **Technical performance** - Page load times remain fast
- **Accessibility compliance** - WCAG 2.1 AA standards met

---

*This plan aligns with the site's minimalist design philosophy while providing maximum educational value for beginning HTML students.*