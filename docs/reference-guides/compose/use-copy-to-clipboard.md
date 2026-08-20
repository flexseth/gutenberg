# useCopyToClipboard Hook Reference

`useCopyToClipboard` is a React hook from the [`@wordpress/compose`](/packages/compose/README.md) package that enables developers to easily add copy-to-clipboard functionality to any clickable element. When the element is clicked, the specified text is copied to the user's clipboard using the modern Clipboard API, with automatic fallback support for older browsers.

This hook is the recommended replacement for the deprecated [`ClipboardButton`](/packages/components/src/clipboard-button/README.md) component, providing a more flexible and modern approach to clipboard operations in WordPress blocks and components.

## Key Features

-   **Modern Clipboard API**: Uses the native `navigator.clipboard.writeText()` API when available for secure, reliable clipboard access
-   **Automatic Fallback**: Provides backward compatibility for older browsers using `document.execCommand('copy')`
-   **Focus Management**: Automatically restores focus to the trigger element after copying
-   **Success Callbacks**: Optional callback function for providing user feedback
-   **Dynamic Content**: Supports both static strings and function-based dynamic content
-   **Type Safe**: Full TypeScript support with proper type definitions

## Installation and Import

The `useCopyToClipboard` hook is part of the `@wordpress/compose` package:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
```

> **Note:** If you're working within the Gutenberg project, the package is already available. For external projects, install it via npm:
> ```bash
> npm install @wordpress/compose
> ```

## Basic Usage

The simplest use case involves copying a static string when a button is clicked:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { Button } from '@wordpress/components';

function CopyButton() {
	const ref = useCopyToClipboard( 'Text to copy' );
	
	return (
		<Button ref={ ref }>
			Copy to Clipboard
		</Button>
	);
}
```

### With User Feedback

Provide visual feedback when the copy operation succeeds:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { Button } from '@wordpress/components';
import { useDispatch } from '@wordpress/data';
import { store as noticesStore } from '@wordpress/notices';
import { __ } from '@wordpress/i18n';

function CopyButtonWithFeedback() {
	const { createNotice } = useDispatch( noticesStore );
	
	const ref = useCopyToClipboard(
		'Text to copy',
		() => {
			createNotice(
				'info',
				__( 'Text copied to clipboard!' ),
				{
					isDismissible: true,
					type: 'snackbar',
				}
			);
		}
	);
	
	return (
		<Button ref={ ref }>
			Copy to Clipboard
		</Button>
	);
}
```

## API Reference

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `text` | `string \| (() => string)` | Yes | The text to copy to the clipboard. Can be a static string or a function that returns a string. Use a function when the text is expensive to compute or changes dynamically. |
| `onSuccess` | `() => void` | No | Callback function invoked after the text is successfully copied. Ideal for showing user feedback, updating UI state, or triggering analytics. |

### Return Value

| Type | Description |
|------|-------------|
| `RefCallback<T extends HTMLElement>` | A ref callback to attach to the clickable element. The element will copy the text when clicked. |

### Type Signature

```typescript
function useCopyToClipboard<T extends HTMLElement>(
	text: string | (() => string),
	onSuccess?: () => void
): RefCallback<T>
```

## Advanced Usage

### Copying Dynamic Content

Use a function to compute the text at copy time rather than when the component renders:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { Button } from '@wordpress/components';
import { useState } from '@wordpress/element';

function DynamicCopyButton() {
	const [ counter, setCounter ] = useState( 0 );
	
	const ref = useCopyToClipboard(
		() => `Current count: ${ counter }`,
		() => console.log( 'Copied current count!' )
	);
	
	return (
		<div>
			<p>Count: { counter }</p>
			<Button onClick={ () => setCounter( counter + 1 ) }>
				Increment
			</Button>
			<Button ref={ ref }>
				Copy Count
			</Button>
		</div>
	);
}
```

### Copying Color Values

This example from the WordPress color picker shows copying different color format representations:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { Button } from '@wordpress/components';
import { useState } from '@wordpress/element';
import { copy, check } from '@wordpress/icons';

function ColorCopyButton( { color, colorType } ) {
	const [ copied, setCopied ] = useState( false );
	
	const ref = useCopyToClipboard(
		() => {
			switch ( colorType ) {
				case 'hsl':
					return color.toHslString();
				case 'rgb':
					return color.toRgbString();
				case 'hex':
				default:
					return color.toHex();
			}
		},
		() => {
			setCopied( true );
			setTimeout( () => setCopied( false ), 3000 );
		}
	);
	
	return (
		<Button
			ref={ ref }
			icon={ copied ? check : copy }
			label={ copied ? 'Copied!' : 'Copy' }
		/>
	);
}
```

### Copying Content from DOM Elements

Use `useRef` to copy text content from a specific DOM element:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { useRef } from '@wordpress/element';
import { Button } from '@wordpress/components';

function CopyFromElement() {
	const textRef = useRef( null );
	const copyRef = useCopyToClipboard(
		() => textRef.current?.textContent || '',
		() => alert( 'Content copied!' )
	);
	
	return (
		<div>
			<div ref={ textRef } contentEditable>
				Edit this text and copy it!
			</div>
			<Button ref={ copyRef }>
				Copy Content
			</Button>
		</div>
	);
}
```

### Copying Different Content Types

#### Plain Text

The default use case - copy simple text strings:

```jsx
const ref = useCopyToClipboard( 'Hello, World!' );
```

#### HTML Content

To copy HTML as plain text (useful for sharing formatted content):

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { serialize } from '@wordpress/blocks';

function CopyBlockHTML( { blocks } ) {
	const ref = useCopyToClipboard(
		() => serialize( blocks ),
		() => console.log( 'Block HTML copied!' )
	);
	
	return <Button ref={ ref }>Copy as HTML</Button>;
}
```

#### Block Serialization

Copy blocks as WordPress block markup:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { serialize } from '@wordpress/blocks';
import { useSelect } from '@wordpress/data';
import { store as blockEditorStore } from '@wordpress/block-editor';

function CopySelectedBlocks() {
	const selectedBlocks = useSelect(
		( select ) => select( blockEditorStore ).getSelectedBlocks(),
		[]
	);
	
	const ref = useCopyToClipboard(
		() => serialize( selectedBlocks ),
		() => console.log( 'Blocks copied!' )
	);
	
	return <Button ref={ ref }>Copy Selected Blocks</Button>;
}
```

#### Unicode and Special Characters

The hook safely handles unicode characters, emojis, and special characters:

```jsx
const ref = useCopyToClipboard( 
	'Hello 👋 World 🌍 with émojis and spëcial çharacters!' 
);
```

### Integration with WordPress Components

#### ToolbarButton Integration

Used in the File block to copy file URLs:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { ToolbarButton } from '@wordpress/components';
import { BlockControls } from '@wordpress/block-editor';
import { useDispatch } from '@wordpress/data';
import { store as noticesStore } from '@wordpress/notices';
import { __ } from '@wordpress/i18n';

function CopyURLToolbarButton( { url } ) {
	const { createNotice } = useDispatch( noticesStore );
	
	const ref = useCopyToClipboard( url, () => {
		createNotice( 'info', __( 'Copied URL to clipboard.' ), {
			isDismissible: true,
			type: 'snackbar',
		} );
	} );
	
	return (
		<BlockControls>
			<ToolbarButton ref={ ref }>
				{ __( 'Copy URL' ) }
			</ToolbarButton>
		</BlockControls>
	);
}
```

#### Tooltip Integration

Add tooltips for better user experience:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { Button } from '@wordpress/components';
import Tooltip from '@wordpress/components';
import { useState } from '@wordpress/element';
import { copy, check } from '@wordpress/icons';
import { __ } from '@wordpress/i18n';

function CopyButtonWithTooltip( { textToCopy } ) {
	const [ copied, setCopied ] = useState( false );
	
	const ref = useCopyToClipboard( textToCopy, () => {
		setCopied( true );
		setTimeout( () => setCopied( false ), 2000 );
	} );
	
	const label = copied ? __( 'Copied!' ) : __( 'Copy' );
	
	return (
		<Tooltip text={ label }>
			<Button
				ref={ ref }
				icon={ copied ? check : copy }
				label={ label }
			/>
		</Tooltip>
	);
}
```

## Security Best Practices

When copying user-generated content or dynamic data, always sanitize and validate the content to prevent XSS attacks:

### Sanitize User Input

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import DOMPurify from 'dompurify';

function SecureCopyButton( { userContent } ) {
	const ref = useCopyToClipboard(
		() => {
			// Strip HTML tags and only copy plain text
			const div = document.createElement( 'div' );
			div.innerHTML = DOMPurify.sanitize( userContent );
			return div.textContent || div.innerText || '';
		}
	);
	
	return <Button ref={ ref }>Copy Content</Button>;
}
```

### Escape Special Characters

For content that might contain executable code:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';

function SafeCopyButton( { rawContent } ) {
	const ref = useCopyToClipboard(
		() => {
			// Ensure content is treated as plain text
			return String( rawContent ).replace( /[<>]/g, '' );
		}
	);
	
	return <Button ref={ ref }>Copy Safely</Button>;
}
```

### Validate Content Before Copying

```jsx
import { useCopyToClipboard } from '@wordpress/compose';

function ValidatedCopyButton( { content } ) {
	const ref = useCopyToClipboard(
		() => {
			// Only copy if content is valid
			if ( typeof content !== 'string' || content.length === 0 ) {
				return '';
			}
			// Limit length to prevent clipboard abuse
			return content.substring( 0, 10000 );
		}
	);
	
	return <Button ref={ ref }>Copy Content</Button>;
}
```

## Accessibility

### Keyboard Shortcuts

Enhance accessibility by adding keyboard shortcuts:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { useKeyboardShortcut } from '@wordpress/compose';
import { Button } from '@wordpress/components';
import { useDispatch } from '@wordpress/data';
import { store as noticesStore } from '@wordpress/notices';
import { __ } from '@wordpress/i18n';

function KeyboardAccessibleCopy( { textToCopy } ) {
	const { createNotice } = useDispatch( noticesStore );
	const ref = useCopyToClipboard( textToCopy, () => {
		createNotice( 'info', __( 'Copied to clipboard!' ), {
			type: 'snackbar',
		} );
	} );
	
	// Add Ctrl+Shift+C / Cmd+Shift+C keyboard shortcut
	useKeyboardShortcut(
		'mod+shift+c',
		() => {
			ref.current?.click();
		},
		{ bindGlobal: true }
	);
	
	return (
		<Button ref={ ref }>
			{ __( 'Copy (Ctrl+Shift+C)' ) }
		</Button>
	);
}
```

### ARIA Labels

Provide clear labels for screen readers:

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { Button } from '@wordpress/components';
import { useState } from '@wordpress/element';
import { __ } from '@wordpress/i18n';

function AccessibleCopyButton( { content, label } ) {
	const [ copied, setCopied ] = useState( false );
	
	const ref = useCopyToClipboard( content, () => {
		setCopied( true );
		setTimeout( () => setCopied( false ), 2000 );
	} );
	
	const ariaLabel = copied
		? __( 'Content copied to clipboard' )
		: __( 'Copy content to clipboard' );
	
	return (
		<Button
			ref={ ref }
			aria-label={ ariaLabel }
			aria-live="polite"
		>
			{ copied ? __( 'Copied!' ) : label }
		</Button>
	);
}
```

### Focus Management

The hook automatically handles focus restoration after the copy operation completes. This ensures that keyboard users maintain their position in the document:

```jsx
// Focus is automatically restored to the button after copying
const ref = useCopyToClipboard( 'Content', () => {
	// This callback runs AFTER focus is restored
	console.log( 'Focus has been restored to the button' );
} );
```

## Core Blocks Using useCopyToClipboard

Several core blocks and components use `useCopyToClipboard`. These implementations serve as best practice references:

-   **[File Block](https://github.com/WordPress/gutenberg/blob/HEAD/packages/block-library/src/file/edit.js)**: Copies file URLs via a toolbar button
-   **[Color Picker](https://github.com/WordPress/gutenberg/blob/HEAD/packages/components/src/color-picker/color-copy-button.tsx)**: Copies color values in different formats (HEX, RGB, HSL)
-   **[Block Settings](https://github.com/WordPress/gutenberg/blob/HEAD/packages/block-editor/src/components/block-settings-menu/block-settings-dropdown.js)**: Copies blocks as HTML
-   **[Error Boundary](https://github.com/WordPress/gutenberg/blob/HEAD/packages/edit-widgets/src/components/error-boundary/index.js)**: Copies error stack traces for debugging

## Common Issues and Solutions

### The onSuccess Callback Runs Even After Unmounting

This is expected behavior. The `onSuccess` callback will execute even if the component unmounts before the copy operation completes, allowing you to update external state or show notifications. However, focus restoration only occurs if the component is still mounted.

```jsx
// ✅ Safe - can update external state
const ref = useCopyToClipboard( text, () => {
	dispatch( { type: 'COPY_SUCCESS' } );
} );

// ❌ Avoid - component state updates after unmount
const ref = useCopyToClipboard( text, () => {
	setCopied( true ); // May cause React warnings if component unmounted
} );
```

### Copy Operation Fails Silently

The hook will fail silently if:
-   The trigger element is null
-   The browser doesn't support the Clipboard API and the fallback also fails
-   The user denies clipboard permissions

To handle failures, you can use the lower-level `copyToClipboard` function:

```jsx
import { copyToClipboard } from '@wordpress/compose';

async function handleCopy( text, element ) {
	const success = await copyToClipboard( text, element );
	
	if ( ! success ) {
		console.error( 'Failed to copy to clipboard' );
	}
}
```

### Empty String Copies Successfully

Copying an empty string is considered a success. Validate your content before copying:

```jsx
const ref = useCopyToClipboard(
	() => {
		const content = getContent();
		return content.length > 0 ? content : 'No content available';
	}
);
```

### Browser Clipboard Permissions

In some browsers, clipboard access requires a secure context (HTTPS). The hook provides a fallback for non-secure contexts, but this fallback may not work in all browsers. For the best user experience, always use HTTPS in production.

## Migration from ClipboardButton

If you're migrating from the deprecated `ClipboardButton` component:

### Before (ClipboardButton)

```jsx
import { ClipboardButton } from '@wordpress/components';

<ClipboardButton
	text="Text to copy"
	onFinish={ () => console.log( 'Copied!' ) }
>
	Copy
</ClipboardButton>
```

### After (useCopyToClipboard)

```jsx
import { useCopyToClipboard } from '@wordpress/compose';
import { Button } from '@wordpress/components';

function CopyButton() {
	const ref = useCopyToClipboard(
		'Text to copy',
		() => console.log( 'Copied!' )
	);
	
	return <Button ref={ ref }>Copy</Button>;
}
```

## Related Resources

-   [`@wordpress/compose` Package Reference](/packages/compose/README.md)
-   [`ClipboardButton` Component (Deprecated)](/packages/components/src/clipboard-button/README.md)
-   [`Button` Component](/packages/components/src/button/README.md)
-   [`useRefEffect` Hook](/packages/compose/src/hooks/use-ref-effect/README.md)
-   [Clipboard API Documentation (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)
-   [WordPress Block Editor Handbook](https://developer.wordpress.org/block-editor/)

## Additional Notes

> **Screenshots**: For detailed visual examples and screenshots, visit the [useCopyToClipboard documentation on developers.wordpress.org](https://developer.wordpress.org/block-editor/reference-guides/packages/packages-compose/#usecopytoclipboard).

> **Browser Compatibility**: The hook automatically detects browser capabilities and uses the appropriate method (Clipboard API or fallback). No additional configuration is required.

> **TypeScript Support**: Full type definitions are included with the package. The hook is generic and can be typed for specific HTML elements: `useCopyToClipboard<HTMLButtonElement>( ... )`
