# 📊 ENHANCED WORKFLOW ANALYSIS
## Based on Project Requirements & Constraints

**Date:** December 18, 2025  
**Status:** Production-ready enhanced workflow  
**Version:** 2.0 (Updated for dual-repo + Tailwind + Figma-ready approach)

---

## 1. 🗓️ SPRINT TIMELINE ADJUSTMENT

### Original vs Adjusted

**Original Plan:**
- Thursday Dec 18 → Sunday Dec 21 (4 days)
- Friday evening = Day 2 ends

**ADJUSTED (Per Your Request):**
- Thursday Dec 18 → Monday Dec 22 (4 full days)
- Benefit: Extra day for validation & Monday launch prep

### Adjusted Daily Timeline

```
THURSDAY (Dec 18) - Day 1: Infrastructure Setup (6 hours)
├─ 9 AM-11:15 AM: Setup GitHub repos (theme + blocks)
├─ 11:15 AM-1:15 PM: Local environment
├─ 1:30-2:30 PM: Linear workspace
└─ Result: Dual repos configured ✅

FRIDAY (Dec 19) - Day 2: Design System & Blocks (8 hours)
├─ 9-10 AM: Figma design review + export
├─ 10 AM-1 PM: Design tokens JSON + Tailwind config + theme.json sync
├─ 1-3 PM: Hero block templates (Tailwind, no SCSS)
├─ 3-4 PM: Create issues in Linear
└─ Result: Ready for development ✅

SATURDAY (Dec 20) - Day 3: Hero Block Development (7 hours)
├─ 10 AM-2 PM: Hero block development (Tailwind CSS)
├─ 2-4 PM: Code review + testing
├─ 4-5 PM: Merge to production
└─ Result: Production-ready block ✅

SUNDAY (Dec 21) - Day 4: Validation & Refinement (6 hours)
├─ 10 AM-12 PM: End-to-end verification
├─ 12-1:30 PM: Documentation finalization
├─ 1:30-3 PM: Team training
└─ Result: System validated ✅

MONDAY (Dec 22) - Day 5: Launch & Client Project Start
├─ 10 AM: Team standup + sprint retrospective
├─ 11 AM: Client Project #1 Kickoff (with Figma)
├─ 1 PM: Designer begins implementation
├─ 2 PM: Developer begins first block from Figma
└─ Result: First project live with new workflow 🚀
```

---

## 2. 🗓️ REPOSITORY SEPARATION ARCHITECTURE

### Problem with Single Repo

```
Single Repo: moraqmen-wordpress-theme/
├─ blocks/
│  ├─ hero/
│  ├─ feature/
│  ├─ cta/
│  └─ gallery/        ← Grows with every block
├─ theme files
└─ config

❌ Issues:
- Blocks and theme coupled
- Hard to reuse blocks across projects
- Difficult to version blocks independently
- Large repository over time
```

### Solution: Dual Repository Architecture

```
📄 REPOSITORY 1: moraqmen-blocks (NPM Package)
├─ What: Reusable block library
├─ Package: Published to NPM
├─ Versioning: Semantic versioning (v1.0.0, v1.1.0, etc.)
├─ Contents:
│  ├─ blocks/
│  │  ├─ hero/
│  │  ├─ feature/
│  │  ├─ cta/
│  │  ├─ gallery/
│  │  └─ testimonials/
│  ├─ design-tokens.json
│  ├─ tailwind.config.js (core config)
│  └─ package.json
├─ Benefits:
│  ✅ Reusable across all themes
│  ✅ Versioned independently
│  ✅ Easy to update
│  ✅ Share across team

📄 REPOSITORY 2: moraqmen-wordpress-theme (WordPress Theme)
├─ What: Project-specific WordPress theme
├─ Per-Client: One theme per client project
├─ Versioning: Dev, staging, production branches
├─ Contents:
│  ├─ blocks/ (empty - uses npm packages)
│  ├─ src/ (theme-specific code)
│  ├─ theme.json (synced with Tailwind)
│  ├─ tailwind.config.js (extends core + project tweaks)
│  ├─ functions.php
│  └─ package.json (dependencies)
├─ Benefits:
│  ✅ Client-specific customization
│  ✅ Independent deployment
│  ✅ Clean separation of concerns
│  ✅ Easy to clone for new clients
```

### File Structure for Both Repos

```
📄 moraqmen-blocks/ (NPM Package - Monorepo)
├─ blocks/
│  ├─ hero/
│  │  ├─ block.json
│  │  ├─ index.js
│  │  ├─ edit.js
│  │  ├─ render.twig
│  │  └─ style.module.css (Tailwind classes)
│  ├─ feature/
│  ├─ cta/
│  └─ gallery/
├─ design-tokens.json (master tokens)
├─ tailwind.config.js (base config - shared)
├─ package.json
│  ├─ name: @moraqmen/blocks
│  ├─ version: 1.0.0
│  ├─ main: ./blocks/index.js
│  └─ scripts: build, test, publish
├─ .github/workflows/ (CI/CD for blocks)
├─ README.md
└─ CHANGELOG.md

📄 moraqmen-wordpress-theme/ (Client Theme - Per Client)
├─ .github/
│  ├─ workflows/
│  │  ├─ lint-and-test.yml
│  │  ├─ deploy-staging.yml
│  │  └─ deploy-production.yml
│  └─ ISSUE_TEMPLATE/
├─ src/
│  ├─ functions.php (theme setup)
│  ├─ template-parts/ (custom templates)
│  └─ scss/ (theme-specific styling - optional)
├─ theme.json (synced with Tailwind)
├─ tailwind.config.js (extends @moraqmen/blocks config)
├─ package.json
│  ├─ dependencies: { "@moraqmen/blocks": "^1.0.0" }
│  ├─ devDependencies: tailwindcss, webpack, etc.
└─ README.md
```

### Package.json Integration

**moraqmen-blocks/package.json:**
```json
{
  "name": "@moraqmen/blocks",
  "version": "1.0.0",
  "description": "Reusable WordPress blocks with Tailwind CSS",
  "main": "./blocks/index.js",
  "scripts": {
    "dev": "webpack serve --mode development",
    "build": "webpack --mode production",
    "test": "jest",
    "publish": "npm publish --access public"
  }
}
```

**moraqmen-wordpress-theme/package.json:**
```json
{
  "name": "moraqmen-theme-client-name",
  "version": "1.0.0",
  "dependencies": {
    "@moraqmen/blocks": "^1.0.0",
    "tailwindcss": "^3.3.0"
  }
}
```

---

## 3. 🗓️ NEW CLIENT PROJECT WORKFLOW

### Complete Steps for Starting New Project

#### Phase 1: Project Setup (1 hour)

**Step 1.1: Create Client Project Issue in Linear**
```
Title: [CLIENT] Project Setup: {Client Name}
Description:

Client: {Client Name}
Project URL: {Domain or staging URL}
Deadline: {Date}
Scope: {Brief description}

Tasks:
- [ ] Create theme repository
- [ ] Setup GitHub Actions
- [ ] Create Figma workspace access
- [ ] Setup Linear issues
- [ ] Configure Tailwind for client
```

**Step 1.2: Clone Theme Template**
```bash
# From GitHub
git clone https://github.com/khmoraqmen/moraqmen-wordpress-theme.git
cd moraqmen-wordpress-theme

# Rename for client
git remote set-url origin https://github.com/khmoraqmen/moraqmen-theme-{client-name}.git

# Update package.json
name: "moraqmen-theme-{client-name}"
version: "1.0.0"

# Install dependencies
npm install

# Add @moraqmen/blocks
npm install @moraqmen/blocks@latest

# Create new branch
git checkout -b setup/client-theme-init
git add .
git commit -m "chore: Setup theme for {Client Name}"
git push origin setup/client-theme-init
```

**Step 1.3: Configure Tailwind for Client Project**
```javascript
// tailwind.config.js (PROJECT-SPECIFIC)
const baseConfig = require('@moraqmen/blocks/tailwind.config');

module.exports = {
  ...baseConfig,
  // Client-specific extensions
  theme: {
    extend: {
      colors: {
        'client-primary': '#YOUR_COLOR',
        'client-secondary': '#YOUR_COLOR',
      },
      // Client-specific changes
    }
  },
  // Rest of config
};
```

**Step 1.4: Sync theme.json with Tailwind**
```bash
# Generate theme.json from Tailwind config
npm run generate:theme-json

# Script in package.json:
"scripts": {
  "generate:theme-json": "node scripts/sync-theme-json.js"
}
```

#### Phase 2: Design System Setup (2-3 hours)

**Step 2.1: Export Figma Design Tokens**
```
In Figma:
1. Open design file
2. Right-click on component library
3. Export → Design Tokens (JSON)
4. Save as design-tokens.json in project root
```

**Step 2.2: Convert Figma Tokens to Tailwind Config**
```bash
# Script converts Figma design-tokens.json to tailwind.config.js
npm run tokens:convert

# Script in package.json:
"scripts": {
  "tokens:convert": "node scripts/convert-tokens.js",
  "tokens:sync": "npm run tokens:convert && npm run generate:theme-json"
}
```

**Step 2.3: Generate theme.json from Tailwind**
```
Automatically synced:
- Colors → Color palette
- Typography → Font sizes
- Spacing → Spacing scale
- Border radius → Border radius
- Shadows → Shadow styles
```

**Step 2.4: Create Linear Issues from Figma Components**
```
For each component in Figma:

Title: [BLOCK] {Component Name}
Description:
Figma Link: [URL]
Design Status: Ready
Estimate: 6 hours
Priority: {Based on project}

Dependencies:
- Design tokens exported
- Tailwind configured
- theme.json synced

Acceptance Criteria:
- Matches Figma design
- Responsive (mobile/tablet/desktop)
- Lighthouse > 90
- Accessibility > 90
```

#### Phase 3: Block Development (Per Block)

**Step 3.1: Developer Picks Issue from Linear**
```
Linear:
1. Select issue: [BLOCK] {Component Name}
2. Click: "Create branch"
3. Local: git fetch && git checkout feature/LINEAR-{N}-component-name
```

**Step 3.2: Convert Figma Component to Block**
```bash
# Create block directory
mkdir blocks/{component-name}
cd blocks/{component-name}

# Create files
touch block.json index.js edit.js render.twig

# Copy from @moraqmen/blocks template
cp ../../node_modules/@moraqmen/blocks/blocks/hero/* .

# Modify for new component
# - Update block.json with new controls
# - Modify edit.js for Gutenberg UI
# - Update render.twig for frontend output
# - Use Tailwind classes in render.twig (NO SCSS)
```

**Step 3.3: Use Tailwind CSS (Not SCSS)**
```twig
{# render.twig - Use Tailwind classes directly #}
<div class="block-component max-w-6xl mx-auto px-4 py-16">
  <h2 class="text-4xl font-bold text-gray-900 mb-8">
    {{ title }}
  </h2>
  
  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
    {# Content here #}
  </div>
</div>
```

**Step 3.4: Test & Verify**
```bash
# Start dev server
npm run dev

# In WordPress:
1. Create new page
2. Add block
3. Test controls in Gutenberg
4. Check responsive design
5. Verify in browser

# Run tests
npm test
npm run lighthouse
npm run accessibility:audit
```

**Step 3.5: Create GitHub PR**
```bash
git add blocks/{component}
git commit -m "feat(LINEAR-{N}): Develop {Component} block

- Created block.json with all controls
- Implemented Gutenberg editor component
- Created responsive Twig template with Tailwind
- Tested in local WordPress
- Lighthouse score: 95
- Accessibility: WCAG 2.1 AA compliant

Closes LINEAR-{N}"

git push origin feature/LINEAR-{N}-component-name
```

#### Phase 4: Deployment (Per Project)

**Step 4.1: Staging Deployment**
```bash
# Trigger GitHub Actions
git push origin feature/... → Creates PR → Copilot reviews → Team approves

# GitHub Actions:
1. Lint code
2. Run tests
3. Build assets
4. Deploy to staging
5. Run Lighthouse audit
```

**Step 4.2: Production Deployment**
```bash
# After client approval on staging
git merge to main

# GitHub Actions:
1. All checks pass
2. Build for production
3. Deploy to production
4. Monitor errors
5. Client notified
```

---

## 4. 🗓️ TAILWIND CSS IMPLEMENTATION

### Why Tailwind (Not SCSS)

**Benefits:**
- ✅ Faster development (no separate CSS files)
- ✅ Utility-first approach (easier to maintain)
- ✅ Smaller CSS output (only used classes)
- ✅ Consistent design system
- ✅ theme.json auto-sync possible
- ✅ Easier responsive design
- ✅ No naming conflicts

### Setup

**Configuration Files:**

```javascript
// Base config in @moraqmen/blocks/tailwind.config.js
module.exports = {
  content: [
    './blocks/**/*.js',
    './blocks/**/*.twig',
  ],
  theme: {
    extend: {
      colors: {
        primary: 'var(--color-primary)',
        secondary: 'var(--color-secondary)',
      },
      fontSize: {
        xs: ['12px', '1.2'],
        sm: ['14px', '1.5'],
        base: ['16px', '1.5'],
        lg: ['18px', '1.5'],
        xl: ['20px', '1.2'],
        '2xl': ['24px', '1.2'],
        '3xl': ['30px', '1.2'],
      },
      spacing: {
        xs: '4px',
        sm: '8px',
        md: '16px',
        lg: '24px',
        xl: '32px',
        '2xl': '48px',
      },
    },
  },
  plugins: [],
};
```

**Project Override (tailwind.config.js):**
```javascript
const baseConfig = require('@moraqmen/blocks/tailwind.config');

module.exports = {
  ...baseConfig,
  theme: {
    extend: {
      ...baseConfig.theme.extend,
      colors: {
        ...baseConfig.theme.extend.colors,
        'brand-primary': '#0E7C86',
        'brand-secondary': '#E74C3C',
      },
    },
  },
};
```

### Tailwind Classes in Templates

```twig
{# Hero Block Example #}
<section class="relative overflow-hidden">
  {# Background with overlay #}
  <div class="absolute inset-0 z-0">
    {% if imageUrl %}
      <img 
        src="{{ imageUrl }}" 
        alt="{{ imageAlt }}" 
        class="w-full h-full object-cover"
      />
    {% endif %}
    <div 
      class="absolute inset-0 bg-black transition-opacity" 
      style="opacity: {{ overlayOpacity }}"
    ></div>
  </div>

  {# Content #}
  <div class="relative z-10 container mx-auto px-4 py-16 sm:py-24 lg:py-32">
    <div 
      class="max-w-2xl flex flex-col {{ contentAlignment == 'left' ? 'items-start text-left' : contentAlignment == 'center' ? 'items-center text-center' : 'items-end text-right' }}"
    >
      {% if title %}
        <h1 class="text-4xl sm:text-5xl lg:text-6xl font-bold text-white mb-4 leading-tight">
          {{ title }}
        </h1>
      {% endif %}

      {% if subtitle %}
        <p class="text-lg sm:text-xl text-gray-100 mb-8 leading-relaxed">
          {{ subtitle }}
        </p>
      {% endif %}

      {% if buttonText and buttonUrl %}
        <a 
          href="{{ buttonUrl }}" 
          class="inline-block bg-primary hover:bg-primary-700 text-white font-semibold py-3 px-6 rounded-lg transition-colors duration-200"
        >
          {{ buttonText }}
        </a>
      {% endif %}
    </div>
  </div>
</section>
```

---

## 5. 🗓️ DESIGN TOKENS TO THEME.JSON SYNC

### Architecture

```
Figma Design File
       ⬇️
 Export design-tokens.json
       ⬇️
 Convert to Tailwind config
       ⬇️
 Generate theme.json (WordPress)
       ⬇️
 Sync Tailwind classes
```

### Scripts for Conversion

**Script 1: scripts/convert-tokens.js**
```javascript
// Converts Figma design-tokens.json to tailwind.config.js
const fs = require('fs');
const designTokens = require('../design-tokens.json');

const tailwindConfig = {
  colors: {},
  fontSize: {},
  spacing: {},
  borderRadius: {},
  boxShadow: {},
};

// Convert colors
if (designTokens.colors) {
  Object.entries(designTokens.colors).forEach(([name, value]) => {
    tailwindConfig.colors[name] = value;
  });
}

// Convert typography
if (designTokens.typography) {
  Object.entries(designTokens.typography.fontSize).forEach(([size, value]) => {
    tailwindConfig.fontSize[size] = value;
  });
}

// Convert spacing
if (designTokens.spacing) {
  Object.entries(designTokens.spacing).forEach(([size, value]) => {
    tailwindConfig.spacing[size] = value;
  });
}

// Write tailwind.config.js
const configContent = `module.exports = { theme: { extend: ${JSON.stringify(tailwindConfig, null, 2)} } }`;
fs.writeFileSync('./tailwind.config.js', configContent);
console.log('✅ Tailwind config generated');
```

**Script 2: scripts/generate-theme-json.js**
```javascript
// Generates WordPress theme.json from Tailwind config
const tailwindConfig = require('../tailwind.config.js');
const fs = require('fs');

const themeJson = {
  version: 2,
  settings: {
    color: {
      palette: Object.entries(tailwindConfig.theme.extend.colors || {})
        .map(([name, value]) => ({
          color: value,
          name: name.charAt(0).toUpperCase() + name.slice(1),
          slug: name,
        })),
    },
    typography: {
      fontSizes: Object.entries(tailwindConfig.theme.extend.fontSize || {})
        .map(([size, value]) => ({
          size: value,
          slug: size,
        })),
    },
  },
};

fs.writeFileSync('./theme.json', JSON.stringify(themeJson, null, 2));
console.log('✅ theme.json generated from Tailwind config');
```

### Automated Sync on Build

**In package.json:**
```json
{
  "scripts": {
    "dev": "npm run tokens:sync && webpack serve --mode development",
    "build": "npm run tokens:sync && webpack --mode production",
    "tokens:sync": "npm run tokens:convert && npm run generate:theme-json"
  }
}
```

**GitHub Action for Auto-Sync:**
```yaml
name: Sync Tokens to Theme

on:
  pull_request:
    paths:
      - 'design-tokens.json'

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install
      - run: npm run tokens:sync
      - name: Commit changes
        run: |
          git config user.name "Moraqmen Bot"
          git add .
          git commit -m "chore: Sync design tokens to Tailwind and theme.json"
          git push
```

---

## 6. 🗓️ COMPLETE NEW PROJECT CHECKLIST

### When Figma Design is Ready

```
PREPARATION (Before Developer Starts)
☐ Figma design complete and approved by client
☐ Design tokens exported from Figma (JSON file)
☐ Design system documented in Figma
☐ Component library created in Figma
☐ Handoff document prepared (links, fonts, colors)

REPOSITORY SETUP (30 min)
☐ Clone moraqmen-wordpress-theme
☐ Create new GitHub repo for client
☐ Update package.json with client name
☐ Create dev/staging/main branches
☐ Setup GitHub Actions workflows
☐ Add Linear integration
☐ Invite team members to repo

DESIGN SYSTEM (1 hour)
☐ Export design-tokens.json from Figma
☐ Place design-tokens.json in project root
☐ Run: npm run tokens:sync
☐ Verify Tailwind config generated
☐ Verify theme.json generated
☐ Test: npm run dev (should start without errors)
☐ Review generated theme.json in Gutenberg
☐ Approve with design lead

ISSUE CREATION (1 hour)
☐ Create Linear Epic: "Project: {Client Name}"
☐ For each Figma component, create Linear issue:
  ☐ [BLOCK] Hero Section
  ☐ [BLOCK] Feature Cards
  ☐ [BLOCK] Call to Action
  ☐ [BLOCK] Gallery
  ☐ [BLOCK] Testimonials
☐ Link Figma component URL to each issue
☐ Estimate hours (6h for most blocks)
☐ Set priorities
☐ Assign to developers
☐ Create sub-issues if needed

DEVELOPMENT ASSIGNMENT (30 min)
☐ Prioritize component list (most important first)
☐ Assign blocks to developers
☐ Create sprint timeline
☐ Set daily standup time
☐ Team readiness check

READY TO START
✅ All developers can clone repo
✅ npm install works
✅ npm run dev starts successfully
✅ Design system visible in Gutenberg
✅ Figma links in Linear
✅ Team trained on workflow
✅ Ready to develop first block!
```

---

## 7. 🗓️ WORKFLOW SUMMARY

### Timeline for New Project

```
MONDAY 10 AM
└─ Team standup
    └─ Review sprint process
    └─ Introduce new project

MONDAY 11 AM
└─ Repository Setup (30 min)
    └─ Clone template
    └─ Create GitHub repo
    └─ Setup CI/CD

MONDAY 12 PM
└─ Design System Setup (1 hour)
    └─ Import Figma tokens
    └─ Run sync scripts
    └─ Verify theme.json

MONDAY 1 PM
└─ Issue Creation (1 hour)
    └─ Create Linear epic
    └─ Break down Figma components
    └─ Assign to team

MONDAY 2 PM
└─ First Block Development Begins
    └─ Developer picks first issue
    └─ Creates branch from Figma
    └─ Uses Tailwind CSS (not SCSS)
    └─ Should complete by EOD or Tuesday

TUESDAY-FRIDAY
└─ Parallel Development
    └─ Developer 1: Building blocks
    └─ Designer: Creating next components
    └─ Both: Synced via Figma links
```

---

## 🎉 ENHANCED WORKFLOW READY

✅ **Sprint adjusted:** Thursday-Monday (4 full days)  
✅ **Dual repos:** moraqmen-blocks (NPM) + moraqmen-wordpress-theme (per-client)  
✅ **Tailwind CSS:** No SCSS, utilities only  
✅ **Auto-sync:** Design tokens → Tailwind config → theme.json  
✅ **Figma-ready:** Can start projects with Figma designs immediately  
✅ **Client workflow:** Complete checklist from design to deployment  

**Ready to launch enhanced workflow Monday! 🚀**