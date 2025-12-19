# Deployment Checklist

## Pre-Deployment Testing (24 hours before deployment)

### Code Quality
- [ ] Run ESLint on all JavaScript files
- [ ] Run PHP CodeSniffer on all PHP files
- [ ] Verify no console errors in browser developer tools
- [ ] Check all commits follow conventional commit format

### Functionality Testing
- [ ] Test all 5 custom blocks in Gutenberg editor:
  - [ ] Hero block renders correctly
  - [ ] Feature block displays properly
  - [ ] CTA block functions as expected
  - [ ] Testimonial block shows content
  - [ ] Gallery block loads images
- [ ] Test block controls and settings
- [ ] Verify responsive behavior at breakpoints:
  - [ ] 320px (mobile)
  - [ ] 768px (tablet)
  - [ ] 1024px (desktop)
  - [ ] 1920px (large desktop)

### Performance Testing
- [ ] Run Lighthouse audit on homepage
- [ ] Target scores: Performance >90, Accessibility >95, Best Practices >95, SEO >95
- [ ] Check bundle size
- [ ] Verify no unused CSS/JavaScript

### Accessibility Testing
- [ ] Test with keyboard navigation only
- [ ] Test with screen reader (NVDA/JAWS)
- [ ] Verify WCAG 2.1 AA compliance
- [ ] Check color contrast ratios
- [ ] Validate alt text for all images

### Cross-Browser Testing
- [ ] Chrome/Chromium (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Edge (latest)
- [ ] Mobile Safari (iOS)
- [ ] Chrome Mobile (Android)

## Deployment Steps

### Step 1: Backup
```bash
# Create database backup
wp-cli --allow-root db export backup-$(date +%Y%m%d-%H%M%S).sql

# Create theme backup
cp -r wp-content/themes/moraqmen-wp-theme wp-content/themes/moraqmen-wp-theme.backup
```

### Step 2: Deploy Code
```bash
# Pull latest code
git pull origin main

# Install dependencies
npm install
composer install

# Build assets
npm run build
```

### Step 3: Database Updates (if needed)
```bash
# Run migrations
wp-cli migrate

# Clear caches
wp-cli cache flush
wp-cli transient delete-all
```

### Step 4: Verify Deployment
```bash
# Check theme is active
wp-cli theme status

# Verify blocks are registered
wp-cli eval 'print_r(WP_Block_Type_Registry::get_instance()->get_all_registered());'
```

## Post-Deployment Verification

### Immediate Testing (30 minutes after deployment)
- [ ] Frontend displays without errors
- [ ] All blocks render correctly
- [ ] Admin panel is accessible
- [ ] Theme settings are intact
- [ ] Menu navigation works

### Extended Testing (2 hours after deployment)
- [ ] User accounts can log in
- [ ] Forms submit successfully
- [ ] Media uploads work
- [ ] Search functionality works
- [ ] Comments display properly

### Performance Monitoring
- [ ] Monitor server CPU usage
- [ ] Monitor memory usage
- [ ] Monitor database response time
- [ ] Check error logs for exceptions
- [ ] Monitor uptime/downtime

### User Feedback
- [ ] Check for support tickets
- [ ] Monitor error tracking (Sentry/etc.)
- [ ] Review server logs for errors
- [ ] Gather initial user feedback

## Rollback Procedures

### Emergency Rollback (if critical issues found)

```bash
# 1. Restore from backup
rm -rf wp-content/themes/moraqmen-wp-theme
cp -r wp-content/themes/moraqmen-wp-theme.backup wp-content/themes/moraqmen-wp-theme

# 2. Restore database
wp-cli --allow-root db import backup-YYYYMMDD-HHMMSS.sql

# 3. Clear all caches
wp-cli cache flush
wp-cli transient delete-all

# 4. Verify rollback
wp-cli theme status
```

### Notify Team
- [ ] Post in Slack
- [ ] Email stakeholders
- [ ] Document issue
- [ ] Schedule post-mortem

## Sign-Off

- [ ] Tech Lead approval
- [ ] QA approval
- [ ] Project Manager approval
- [ ] Deployment completed successfully

**Deployment Date:** _______________
**Deployed By:** _______________
**Verified By:** _______________
