# useCopyToClipboard Reference Guide - Implementation Plan

## Overview
Create a comprehensive reference guide for the `useCopyToClipboard` hook from `@wordpress/compose` package to replace the deprecated `ClipboardButton` component documentation.

## Phases

### Phase 1: Introduction and Basic Usage
**Timeline:** 1 week  
**Priority:** High

**Tasks:**
1. Write introduction explaining what `useCopyToClipboard` does
2. Explain why it's preferred over deprecated `ClipboardButton`
3. Provide import statement and setup instructions
4. Create simple basic usage example with a button
5. Add example showing how to handle the copy state with user feedback

**Deliverables:**
- Introduction section
- Installation/Import section
- Basic usage example with code snippet

### Phase 2: Advanced Usage and API Reference
**Timeline:** 2 weeks  
**Priority:** High

**Tasks:**
1. Create advanced examples:
   - Copying dynamic content (using function parameter)
   - Copying content from DOM elements using `useRef`
   - Implementing user feedback with success callbacks
2. Document copying different content types:
   - Plain text
   - HTML content
   - Block serialization
   - Markdown format
3. Create comprehensive API reference:
   - Parameters (text, onSuccess)
   - Return value (RefCallback)
   - Type signatures
4. Show integration with WordPress components (Button, Tooltip, ToolbarButton)

**Deliverables:**
- Advanced usage examples section
- Multiple content type examples
- Complete API reference table

### Phase 3: Security and Accessibility
**Timeline:** 1 week  
**Priority:** Medium

**Tasks:**
1. Document security best practices:
   - Content sanitization when copying user-generated content
   - XSS prevention techniques
   - Safe handling of special characters and unicode
2. Implement accessibility features:
   - Keyboard shortcut integration
   - ARIA labels and screen reader support
   - Focus management after copy action
3. Add example showing proper error handling

**Deliverables:**
- Security best practices section
- Accessibility examples
- Error handling patterns

### Phase 4: Real-World Examples and Final Documentation
**Timeline:** 1 week  
**Priority:** Medium

**Tasks:**
1. Document core blocks using `useCopyToClipboard`:
   - File block (copy URL example)
   - Color picker (copy color values)
   - Block settings (copy block as HTML)
2. Add common issues and solutions section
3. Create links to:
   - Related components (ClipboardButton)
   - Package documentation (@wordpress/compose)
   - Core blocks examples
   - Additional resources
4. Final review for accuracy and clarity
5. Update deprecated ClipboardButton documentation to link to new guide
6. Add note about screenshots for developers.wordpress.org (placeholder for now)

**Deliverables:**
- Complete reference guide
- Links to all referenced components and packages
- Updated ClipboardButton deprecation notice

### Phase 5: Sample Block Interface (Block Development Examples)
**Timeline:** 2 weeks  
**Priority:** Medium  
**Status:** Planned

**Tasks:**
1. Create a comprehensive sample block for the [block-development-examples](https://github.com/WordPress/block-development-examples) repository
2. Showcase all examples from the reference guide in a working block:
   - Basic copy button with static text
   - Copy button with user feedback (notices)
   - Dynamic content copying (counter example)
   - Copy from DOM element using `useRef`
   - Color value copying in different formats (HEX, RGB, HSL)
   - Copy block content as HTML
   - Copy with tooltip feedback
   - Keyboard shortcut integration (Ctrl+Shift+C)
   - Security examples (sanitized content)
   - Accessibility features (ARIA labels, focus management)
3. Structure the block following WordPress standards:
   - Use `@wordpress/create-block` package structure
   - Follow WordPress Coding Standards (JS, PHP)
   - Include proper file and folder organization
   - Add comprehensive inline documentation
4. Create supporting files:
   - Detailed README.md with setup instructions
   - Screenshots showing each example in action
   - CHANGELOG.md for version tracking
   - package.json with proper dependencies
5. Implement block variations or tabs to demonstrate different patterns
6. Add block inspector controls to switch between examples
7. Include proper error handling and edge cases
8. Ensure WordPress 6.0+ compatibility

**Deliverables:**
- Complete working block plugin ready for block-development-examples repo
- Block name: `copy-to-clipboard-examples` or `use-copy-to-clipboard-demo`
- README.md with:
  - Installation instructions
  - Overview of all examples included
  - Links to reference guide
  - Screenshot gallery
  - Usage instructions
- Screenshots for each example pattern
- Sample content/fixture data for testing
- Submission-ready code following repository guidelines

**Block Features to Demonstrate:**

1. **Basic Examples Tab/Section:**
   - Simple copy button
   - Copy with success notification
   - Copy with visual feedback (icon change)

2. **Advanced Examples Tab/Section:**
   - Dynamic content (counter, form inputs)
   - Copy from contentEditable div
   - Function-based text generation
   - Multiple content type copying

3. **Integration Examples Tab/Section:**
   - WordPress Button component
   - ToolbarButton in BlockControls
   - Tooltip wrapper
   - Color picker integration

4. **Accessibility Tab/Section:**
   - Keyboard shortcut demonstration
   - ARIA labels and live regions
   - Screen reader announcements
   - Focus management demo

5. **Security Tab/Section:**
   - Sanitized user input copying
   - HTML tag stripping
   - Content validation
   - Length limiting

**Technical Requirements:**
- WordPress 6.0+
- React 18+ (WordPress element)
- Use only WordPress packages (no external dependencies)
- ES6+ JavaScript
- PHP 7.4+
- Follow Gutenberg component patterns
- Include proper TypeScript types if applicable
- Pass Plugin Check Plugin (PCP) standards
- Pass WPCS standards

**Repository Submission:**
- Fork block-development-examples repository
- Create block in appropriate category folder
- Submit pull request with comprehensive description
- Include reference to gutenberg documentation
- Link back to useCopyToClipboard reference guide

## File Locations

**Reference Guide:**  
- Local: `/Users/seth/Developer/WordPress/gutenberg/docs/reference-guides/compose/use-copy-to-clipboard.md`
- GitHub: [use-copy-to-clipboard.md](https://github.com/flexseth/gutenberg/blob/docs/ref-guide_useCopyToClipboard/docs/reference-guides/compose/use-copy-to-clipboard.md)

**Implementation Files:**
- [PLAN.md](https://github.com/flexseth/gutenberg/blob/docs/ref-guide_useCopyToClipboard/packages/components/src/clipboard-button/PLAN.md)
- [PROGRESS-REPORT.md](https://github.com/flexseth/gutenberg/blob/docs/ref-guide_useCopyToClipboard/packages/components/src/clipboard-button/PROGRESS-REPORT.md)
- [CLAUDE.md](https://github.com/flexseth/gutenberg/blob/docs/ref-guide_useCopyToClipboard/packages/components/src/clipboard-button/CLAUDE.md)

**Sample Block (Planned):**  
`https://github.com/WordPress/block-development-examples/plugins/copy-to-clipboard-examples/`

**GitHub Issues:**
- [Phase 1: Review Introduction and Basic Usage](https://github.com/flexseth/gutenberg/issues/1)
- [Phase 2: Review Advanced Usage and API Reference](https://github.com/flexseth/gutenberg/issues/2)
- [Phase 3: Review Security and Accessibility](https://github.com/flexseth/gutenberg/issues/3)
- [Phase 4: Review Real-World Examples and Final Documentation](https://github.com/flexseth/gutenberg/issues/4)
- [Phase 5: Create Sample Block Interface](https://github.com/flexseth/gutenberg/issues/5)

## Success Criteria
- Comprehensive coverage of all hook features
- Clear, runnable code examples
- Follows same structure as other reference guides (like RichText)
- Properly linked to all related components and packages
- Includes security and accessibility guidance
- Easy to understand for developers of all skill levels
- Working sample block demonstrating all patterns
- Block submitted to block-development-examples repository
- Screenshots and visual documentation completed
