# Block Development Guide

## Block Folder Structure

All custom blocks should be organized in the following structure:

```
blocks/
├── hero/
│   ├── block.json
│   ├── index.js
│   ├── edit.js
│   ├── save.js
│   └── style.scss
├── feature/
│   ├── block.json
│   ├── index.js
│   ├── edit.js
│   ├── save.js
│   └── style.scss
├── cta/
├── testimonial/
└── gallery/
```

## Block Component Template

### block.json

Each block must have a `block.json` file defining its properties:

```json
{
  "$schema": "https://schemas.wp.org/trunk/block.json",
  "apiVersion": 2,
  "name": "moraqmen/hero",
  "title": "Hero Block",
  "category": "common",
  "icon": "cover-image",
  "description": "Hero section block with background image and text overlay",
  "supports": {
    "html": false,
    "align": ["full", "wide"],
    "color": {
      "background": true,
      "text": true
    },
    "spacing": {
      "padding": true,
      "margin": true
    }
  },
  "example": {},
  "render": "file:./render.php"
}
```

## Figma to Code Conversion Workflow

1. **Export Design Specifications**
   - Export component dimensions from Figma
   - Document color palette
   - Note typography specifications
   - List required spacing and breakpoints

2. **Create Block Markup**
   - Translate Figma design to semantic HTML
   - Implement responsive grid layout
   - Add accessibility attributes (aria-labels, roles)

3. **Apply Tailwind CSS Styling**
   - Use Tailwind utility classes for responsive design
   - Implement theme colors from design system
   - Ensure mobile-first approach

4. **Add Block Controls**
   - Text input fields for editable content
   - Media upload for images
   - Color pickers for customization
   - Toggle controls for optional features

## Block Testing Procedures

### Editor Testing
- Verify block renders correctly in Gutenberg editor
- Test all block controls and settings
- Check that dynamic content updates properly
- Validate keyboard navigation

### Frontend Testing
- Test rendering on frontend
- Verify responsive design at breakpoints: 320px, 768px, 1024px, 1920px
- Test with various content lengths
- Verify accessibility with screen readers

### Performance Testing
- Run Lighthouse audit
- Check render time
- Verify no console errors

## Block Registration & Initialization

Register blocks in `functions.php`:

```php
function moraqmen_theme_register_blocks() {
    register_block_type_from_metadata(
        get_template_directory() . '/blocks/hero',
        array(
            'render_callback' => 'moraqmen_render_hero_block',
        )
    );
}
add_action( 'init', 'moraqmen_theme_register_blocks' );
```

## Development Best Practices

1. Use semantic HTML markup
2. Implement proper accessibility (WCAG 2.1 AA)
3. Follow WordPress coding standards
4. Add inline documentation
5. Test across all major browsers
6. Optimize images and assets
7. Version your block changes
