# Dependency Management Guide

## Overview
This document outlines the strategy for keeping project dependencies up-to-date and secure.

## Current Tech Stack

### Core Framework
- **Angular**: ~20.3.10
  - @angular/animations
  - @angular/common
  - @angular/compiler
  - @angular/core
  - @angular/forms
  - @angular/platform-browser
  - @angular/platform-browser-dynamic
  - @angular/router

### UI Libraries
- **Angular Material**: ~20.2.12
- **Angular CDK**: ~20.2.12
- **ngx-toastr**: ~19.1.0 (notification library)

### Core Dependencies
- **RxJS**: ~7.8.2 (reactive programming)
- **Zone.js**: ~0.15.1 (change detection)
- **TypeScript**: ~5.9.3

## Update Strategy

### Regular Updates (Monthly)
Run these commands to check for and apply minor/patch updates:

```bash
# Check for outdated packages
npm run check-updates

# Update to latest minor/patch versions
npm run update-deps

# After updating, test the application
npm install
npm run build
npm run test
npm run lint
```

### Major Version Updates (Quarterly)
For major version updates, use Angular's update guide:

```bash
# Check Angular update path
npx @angular/cli@latest update

# Update Angular CLI and core packages
ng update @angular/cli @angular/core

# Update Angular Material
ng update @angular/material

# Update other major dependencies
npm run update-deps-major
npm install
```

### Manual Review Required
Always review these updates manually due to potential breaking changes:
- Angular (major versions)
- TypeScript (check Angular compatibility)
- RxJS (major versions)
- Angular Material (major versions)

## Checking for Updates

### Using npm
```bash
# List all outdated packages
npm outdated

# Check for specific package
npm outdated <package-name>
```

### Using Angular CLI
```bash
# Check for Angular-specific updates
ng update
```

## Security Updates

### Audit Dependencies
```bash
# Check for known vulnerabilities
npm audit

# Fix vulnerabilities automatically (if possible)
npm audit fix

# Force fix (may introduce breaking changes)
npm audit fix --force
```

### Monitor Security Advisories
- Enable GitHub Dependabot alerts
- Subscribe to Angular security announcements
- Review npm security advisories regularly

## Version Compatibility Matrix

| Angular | TypeScript | RxJS   | Node.js |
|---------|------------|--------|---------|
| 20.x    | 5.6-5.9    | 7.4+   | 18.19+  |
| 19.x    | 5.4-5.5    | 7.4+   | 18.19+  |
| 18.x    | 5.2-5.4    | 7.4+   | 18.13+  |

## Migration Notes

### From TSLint to ESLint (Completed)
- **Removed**: `tslint`, `codelyzer`, `jasmine-spec-reporter`, `protractor`
- **Added**: `@angular-eslint/*`, `eslint`, `@typescript-eslint/*`
- **Configuration**: `.eslintrc.json` (replaces `tslint.json`)

### Deprecated Dependencies Removed
1. **TSLint** → ESLint (TSLint deprecated in 2020)
2. **Codelyzer** → @angular-eslint (depends on TSLint)
3. **Protractor** → Other E2E solutions (deprecated in Angular 12)

## Best Practices

### Before Updating
1. ✅ Review CHANGELOG files for breaking changes
2. ✅ Check Angular update guide: https://update.angular.io
3. ✅ Create a backup branch
4. ✅ Ensure all tests pass
5. ✅ Check for deprecated API usage

### After Updating
1. ✅ Run `npm install` to update lock file
2. ✅ Run `npm run build` to verify build succeeds
3. ✅ Run `npm run test` to verify tests pass
4. ✅ Run `npm run lint` to check code quality
5. ✅ Test the application manually
6. ✅ Update documentation if needed

### CI/CD Integration
Consider adding automated dependency checks:
```yaml
# .github/workflows/dependencies.yml
name: Check Dependencies
on:
  schedule:
    - cron: '0 0 * * 1' # Weekly on Monday
  workflow_dispatch:

jobs:
  check-updates:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '18'
      - run: npm install
      - run: npm outdated || true
      - run: npm audit
```

## Troubleshooting

### Version Conflicts
If you encounter peer dependency conflicts:
```bash
# Use legacy peer deps (temporary solution)
npm install --legacy-peer-deps

# Or force install (use with caution)
npm install --force
```

### Build Failures After Update
1. Clear cache: `npm cache clean --force`
2. Remove node_modules: `rm -rf node_modules`
3. Remove package-lock.json: `rm package-lock.json`
4. Reinstall: `npm install`
5. Check TypeScript version compatibility

### Runtime Errors After Update
1. Check browser console for errors
2. Review breaking changes in dependency CHANGELOGs
3. Update import statements if needed
4. Check for deprecated API usage

## Resources

- [Angular Update Guide](https://update.angular.io/)
- [Angular Releases](https://github.com/angular/angular/releases)
- [Angular Material Changelog](https://github.com/angular/components/releases)
- [RxJS Documentation](https://rxjs.dev/)
- [TypeScript Release Notes](https://www.typescriptlang.org/docs/handbook/release-notes/overview.html)
- [npm Documentation](https://docs.npmjs.com/)

## Support

For issues with updates:
1. Check Angular GitHub issues
2. Review Stack Overflow
3. Consult Angular Discord/Slack communities
4. Review this project's issue tracker

---

**Last Updated**: November 2025
**Current Angular Version**: 20.3.10
**Next Scheduled Review**: December 2025
