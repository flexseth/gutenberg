# purpose

Currently the `ClipboardButton` component is used to copy text to the clipboard when clicked. It provides a simple interface for users to copy content without needing to manually select and copy it. The button can be customized with different styles and icons to fit the design of the application.

However, the component is deprecated and it is recommended to use the `useCopyToClipboard` hook from the `@wordpress/compose` package instead. This hook provides a more flexible and modern approach to copying text to the clipboard, allowing developers to manage the copy state and handle copy events more effectively.

There is no reference guide for this component.

[Reference guides](./docs/reference-guides/) showcase how to use components, with incomplete examples and code snippets. They are not meant to be comprehensive documentation, but rather a starting point for developers to understand how to use the component in their own projects.

## delivered work product 

I want to create a reference guide for `useCopyToClipboard` that includes examples of how to use the hook, its API, and best practices for implementing it in a React application. The guide should cover the following topics:

1. **Introduction to `useCopyToClipboard`**: Explain what the hook does, its purpose, and why it is preferred over the deprecated `ClipboardButton` component.
2. **Installation and Setup**: Provide instructions on how to install the `@wordpress/compose` package and set up the hook in a React application. 
> **NOTE:** This should be an import.
3. **Basic Usage**: Show a simple example of how to use the `useCopyToClipboard` hook in a functional component, including how to trigger the copy action and handle the copy state.
4. **Advanced Usage**: Demonstrate more complex scenarios, such as copying dynamic content, handling errors, and providing user feedback after a successful copy.
5. **API Reference**: List the available functions and properties provided by the `useCopyToClipboard` hook, along with their types and descriptions.
6. Find other elements in common reference guides, provided in the link above.
7. Show how `useRef` can be used in conjunction with `useCopyToClipboard` to copy content from a specific DOM element.
8. Show how to copy as blocks, HTML, plain text, and Markdown. The button should be able to copy unicode characters, emojis, and other special characters without issues.
9. The implementation should securely handle data, using escaping, sanitization, and validation techniques to prevent XSS attacks or other security vulnerabilities when copying content to the clipboard. 
10. Show how to add a keyboard shortcut to trigger the copy action, enhancing accessibility for users who prefer keyboard navigation.
11. The reference guide should provide screenshots, which will need to be uploaded to developers.wordpress.org.
12. Relevant reference links should be provided at the end of the guide
13. As a final cleanup task, it should be noted that the deprecated component should be linked to this reference guide.

## plan
Create a `PLAN.md` file that works in phases, addressed in 1-10 above. Sort them by relevancy and priority, and provide a timeline for each phase. The plan should include the following sections:

- **Phase 1: Introduction and Setup**
   - Timeline: 1 week
   - Tasks:
     - Write an introduction to the `useCopyToClipboard` hook.
     - Provide installation instructions for the `@wordpress/compose` package.
     - Include a simple example of basic usage.
- **Phase 2: Advanced Usage and API Reference**
   - Timeline: 2 weeks
   - Tasks:
     - Demonstrate advanced usage scenarios.
     - Provide a comprehensive API reference for the hook.
     - Include examples of copying dynamic content and handling errors.
- **Phase 3: Security and Accessibility**
   - Timeline: 1 week
   - Tasks:
     - Implement security best practices for copying content to the clipboard.
     - Show how to add keyboard shortcuts for accessibility.    
- **Phase 4: Final Review and Documentation**
   - Timeline: 1 week
   - Tasks:
     - Review the entire reference guide for accuracy and clarity.
     - Ensure all code snippets are functional and well-documented.
     - Add links to additional resources and related components/hooks.

The doc should land in the `reference-guides` folder in the relevant subfolder.
The doc should follow the same structure as other reference guides, with clear headings, code snippets, and explanations. It should be written in a way that is easy to understand for developers of all skill levels, and should include links to additional resources where appropriate.

Always link directly to components, packages, etc, when they are mentioned in the reference guide, for quick lookup. 

The guide should be comprehensive enough to serve as a go-to resource for developers looking to implement copy-to-clipboard functionality in their applications using the `useCopyToClipboard` hook.

## resources

### Deprecated Component
<ClipboardButton> component from `@wordpress/components` package.

**Deprecated but relevant code sample:** `<ClipboardButton>` component from `@wordpress/components` package.
**Relevant hook:** [`useCopyToClipboard`](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-compose/#usecopytoclipboard) from `@wordpress/compose` package.

### GitHub search for `<ClipboardButton>` code
https://github.com/search?q=repo%3AWordPress%2Fgutenberg+ClipboardButton&type=code

### GitHub search for `useCopyToClipboard` code
https://github.com/search?q=repo%3AWordPress%2Fgutenberg+useCopyToClipboard&type=code
