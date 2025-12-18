# Repository Separation Analysis & Corrections

## Executive Summary

**Issue Identified:** The NEXT_STEPS.md document in the moraqmen-wp-theme repository contains incorrect instructions for npm package installation and Tailwind CSS setup. These tasks belong exclusively in the moraqmen-wp-blocks repository, not in the theme.

**Root Cause:** Misunderstanding of the hybrid repository separation architecture during documentation creation.

**Impact:** HIGH - Team members following the theme documentation will waste time on incorrect setup tasks that should only occur in the blocks repository.

**Status:** CRITICAL - Requires immediate correction before sprint kickoff (tomorrow, Dec 19, 2025)

---

## Architecture Decision Review

### What Was Decided (Correct Understanding)

The Moraqmen WordPress ecosystem uses a **Hybrid Separated Repository Model**:

#### Repository 1: moraqmen-wp-blocks (Plugin Repository)
- **Purpose:** Custom Gutenberg blocks plugin development
- **Ownership:** Block developers
- **Content:**
  - Block component code (React/JSX)
  - Tailwind CSS configuration (tailwind.config.js)
  - npm dependencies (package.json, node_modules/)
  - Build pipeline (webpack, npm scripts)
  - Block registration (plugin.php)
  - Block styling
  - Copilot instructions (.copilot-instructions)
- **Development:** npm install, npm run build, npm run start
- **Tech Stack:** Node.js, npm, React, Tailwind CSS, webpack
- **Deployment:** As WordPress plugin to wp-content/plugins/

#### Repository 2: moraqmen-wp-theme (FSE Theme Repository)
- **Purpose:** WordPress Full Site Editing theme that USES the blocks from moraqmen-wp-blocks
- **Ownership:** Theme developers
- **Content:**
  - theme.json configuration (colors, typography, spacing)
  - Template files (header, footer, templates)
  - Template parts for FSE
  - Documentation (setup, workflow)
  - CI/CD configuration for theme
  - Theme styles (CSS only, NO npm build)
  - NO block development code
  - NO npm dependencies
  - NO Tailwind configuration (inherits from blocks via theme.json)
- **Development:** Direct theme editing, no build process needed
- **Tech Stack:** PHP, CSS, JSON (theme.json)
- **Installation:** Direct theme folder in wp-content/themes/
- **Dependency:** Requires moraqmen-wp-blocks plugin to be installed and activated

---

## Problems Identified

### Issue #1: npm Installation Instructions in Theme Documentation

**Location:** docs/NEXT_STEPS.md (moraqmen-wp-theme)

**Problematic Section:**
```
### 2. **Environment Setup Verification** [URGENT]
**For Each Team Member:**
- [ ] Verify Local by Flywheel installation
- [ ] Clone moraqmen-wordpress-theme repository locally
- [ ] Install required npm dependencies
  ```bash
  npm install
  # Install Tailwind CSS
  npm install -D tailwindcss postcss autoprefixer
  ```
```

**Why This Is Wrong:**
1. The moraqmen-wp-theme repository has NO package.json
2. There are NO npm dependencies to install for the theme
3. Tailwind CSS setup is done in moraqmen-wp-blocks, NOT the theme
4. The theme has NO build process
5. Team members will waste time trying to install packages that don't exist
6. This creates confusion about the separation of concerns

**Correct Behavior:**
- moraqmen-wp-theme developers should NOT run npm install
- Tailwind CSS setup is handled by block developers in moraqmen-wp-blocks
- Theme developers work directly with theme.json and PHP templates

### Issue #2: Block Development Instructions in Theme Repository

**Location:** docs/NEXT_STEPS.md - Section 2 (Environment Setup)

**Problematic Content:**
- "Clone moraqmen-wordpress-theme repository locally"
- "Install required npm dependencies"
- Implying block structure setup in theme repo

**Why This Is Wrong:**
- Block folder structure (src/, blocks/) belongs in moraqmen-wp-blocks
- Block development happens in moraqmen-wp-blocks
- The theme repository should never have a blocks/ folder
- This conflates two distinct repositories

### Issue #3: Missing Block Development Instructions in Block Repository

**Location:** moraqmen-wp-blocks README.md

**Current State:** The README correctly shows npm setup, but NEXT_STEPS should NOT exist there or should be different

**What's Missing:**
- No clear guidance that block developers work in this repo
- No setup steps for block developers to clone THIS repo
- No npm setup instructions specific to block development

---

## Corrected Understanding

### For Theme Repository (moraqmen-wp-theme)

**What Theme Developers Do:**
```
1. Clone moraqmen-wp-theme repository
   git clone https://github.com/Moraqmen-LLC/moraqmen-wp-theme.git
   
2. Setup Local by Flywheel with WordPress 6.x+ (FSE enabled)
   - Create new Local site
   - Ensure WordPress 6.x is installed
   
3. Install moraqmen-blocks plugin
   - Clone moraqmen-wp-blocks to a separate folder
   - OR download from releases
   - Copy to wp-content/plugins/
   - Activate in WordPress Admin
   
4. Install moraqmen-wp-theme
   - Copy theme folder to wp-content/themes/
   - Activate in WordPress Admin
   
5. Edit theme in WordPress Block Editor
   - Use FSE to edit templates
   - Configure theme.json
   - NO command line, NO npm
```

### For Block Repository (moraqmen-wp-blocks)

**What Block Developers Do:**
```
1. Clone moraqmen-wp-blocks repository
   git clone https://github.com/Moraqmen-LLC/moraqmen-wp-blocks.git
   cd moraqmen-wp-blocks
   
2. Install Node.js 16+ and npm 8+
   (System-level installation, not project-specific)
   
3. Install project dependencies
   npm install
   
4. Setup Local by Flywheel WordPress instance
   - Create new Local site
   - Ensure WordPress 6.x is installed with FSE
   
5. Link plugin for development
   - Symlink or copy moraqmen-wp-blocks to wp-content/plugins/
   - Activate in WordPress Admin
   
6. Start development
   npm run start    # Watch mode
   npm run build    # Production build
   
7. Test in WordPress Block Editor
   - Create posts/pages
   - Use blocks to verify they work
   - Check styling with moraqmen-wp-theme
```

---

## Specific Corrections Needed

### Correction #1: Update NEXT_STEPS.md in moraqmen-wp-theme

**Remove These Sections:**
- All npm installation instructions
- "Install required npm dependencies" 
- Tailwind CSS installation steps
- Block folder structure creation
- "Install Tailwind CSS" code block

**Replace With:**
- Instructions to clone moraqmen-wp-blocks separately
- Instructions to copy moraqmen-wp-blocks to wp-content/plugins/
- Instructions to activate moraqmen-blocks in WordPress Admin
- Clear statement: "NO npm setup needed for theme development"
- Setup workflow for theme.json editing

**Key Addition:**
```
### 2. **Environment Setup Verification** [URGENT]

**For Theme Developers:**
- [ ] Verify Local by Flywheel installation
- [ ] Create new WordPress site with WordPress 6.x+ (FSE enabled)
- [ ] Clone moraqmen-wp-blocks repository (SEPARATE from theme)
  ```bash
  # In a different folder, not inside the theme
  git clone https://github.com/Moraqmen-LLC/moraqmen-wp-blocks.git
  ```
- [ ] Copy moraqmen-wp-blocks to WordPress plugins folder
  ```bash
  cp -r moraqmen-wp-blocks /path/to/local/site/wp-content/plugins/
  ```
- [ ] Clone moraqmen-wp-theme repository
  ```bash
  git clone https://github.com/Moraqmen-LLC/moraqmen-wp-theme.git
  ```
- [ ] Copy moraqmen-wp-theme to WordPress themes folder
  ```bash
  cp -r moraqmen-wp-theme /path/to/local/site/wp-content/themes/
  ```
- [ ] Activate both in WordPress Admin:
  - Plugins → Activate "Moraqmen-Blocks"
  - Themes → Activate "Moraqmen WordPress Theme"
- [ ] Verify GitHub SSH key is configured
- [ ] Pull latest code from main branch
```

### Correction #2: Create Proper Block Developer Documentation

**Location:** moraqmen-wp-blocks/docs/BLOCK_DEVELOPER_SETUP.md (NEW)

**Content Should Include:**
- Cloning moraqmen-wp-blocks repository
- Node.js/npm installation requirements
- npm install and dependency setup
- Tailwind CSS configuration
- Local WordPress setup for testing blocks
- npm scripts explanation (start, build, lint, format)
- Block development workflow
- Testing blocks in WordPress
- Contributing guidelines

### Correction #3: Update Repository Descriptions

**moraqmen-wp-theme Description:**
Change from: "Production WordPress theme with Moraqmen-Blocks plugin and Full Site Editing. Includes complete block development workflow and CI/CD pipeline."

Change to: "Production WordPress FSE theme that works with moraqmen-wp-blocks plugin. Complete theme development with Full Site Editing support."

Reason: Remove "block development workflow" - that belongs in moraqmen-wp-blocks repo.

**moraqmen-wp-blocks Description:**
Should read: "Custom Gutenberg blocks plugin for WordPress FSE with Tailwind CSS integration. Complete block development and build pipeline."

---

## Impact on Next Steps

### For Tomorrow's Sprint Kickoff

**BEFORE team sync meeting at 9:00 AM (Dec 19):**

1. [ ] Update NEXT_STEPS.md to remove npm instructions from theme
2. [ ] Add instructions for installing moraqmen-wp-blocks plugin
3. [ ] Create clear separation: "Theme Dev Tasks" vs "Block Dev Tasks"
4. [ ] Update team documentation references
5. [ ] Create separate setup guides for theme vs blocks

**During team sync meeting:**
- [ ] Clarify which repo each team member should clone
- [ ] Explain that theme developers DON'T need npm
- [ ] Explain that block developers work in separate repo
- [ ] Assign team members to correct repositories

### Team Member Assignments

**Theme Developers (moraqmen-wp-theme):**
- Clone moraqmen-wp-theme only
- NO npm setup
- Focus: theme.json, templates, FSE editing
- Local setup: Copy plugin + theme to WordPress

**Block Developers (moraqmen-wp-blocks):**
- Clone moraqmen-wp-blocks only
- npm install required
- Tailwind CSS setup required
- Focus: Block components, styling, build process
- Local setup: npm + WordPress with both plugin and theme

**Full Stack Developers (Both):**
- Clone both repositories in separate folders
- Setup npm for blocks repo
- Setup WordPress for both repos
- Can work on blocks and theme integration

---

## Prevention for Future

### Documentation Standards

1. **Repository-Specific Docs Only**
   - Each repo has docs folder
   - Document only what's in that repo
   - Cross-reference other repos when needed
   - NEVER duplicate setup instructions

2. **Clear Role-Based Sections**
   - "For Block Developers" sections in moraqmen-wp-blocks/docs/
   - "For Theme Developers" sections in moraqmen-wp-theme/docs/
   - "For Full Stack" sections in main README

3. **Repository Separation Checklist**
   - Does this code belong in this repo?
   - Are there npm scripts? → blocks repo only
   - Are there block components? → blocks repo only
   - Are there templates/theme.json? → theme repo only
   - Is it documentation for setup? → repo-specific docs folder only

### Testing Documentation Accuracy

- [ ] New developers should be able to follow ONE set of instructions
- [ ] Theme dev setup should NOT mention npm
- [ ] Block dev setup should mention npm
- [ ] Instructions should be testable (run them yourself first)
- [ ] Peer review all documentation before committing

---

## Summary of Required Changes

| Document | Issue | Fix | Priority |
|----------|-------|-----|----------|
| NEXT_STEPS.md (theme) | npm instructions | Remove/replace with plugin instructions | CRITICAL |
| NEXT_STEPS.md (theme) | Block folder setup | Remove (blocks in different repo) | CRITICAL |
| Repository description | Mentions block dev | Update to theme-only | HIGH |
| Team onboarding docs | Role confusion | Add "For Theme Devs" vs "For Block Devs" | HIGH |
| NEW: Block setup guide | Missing | Create in moraqmen-wp-blocks/docs/ | HIGH |

---

## Approval Needed

- [ ] Technical Lead review
- [ ] Repository maintainers approval
- [ ] Before sprint kickoff (Dec 19, 9:00 AM EET)

---

## Files Modified/Created

1. **moraqmen-wp-theme/docs/NEXT_STEPS.md** - NEEDS UPDATE
2. **moraqmen-wp-blocks/docs/BLOCK_DEVELOPER_SETUP.md** - NEEDS CREATION
3. **Repository descriptions** - NEEDS UPDATE

---

*Document created: December 18, 2025, 7:30 PM EET*  
*Status: Analysis Complete - Awaiting Implementation*
