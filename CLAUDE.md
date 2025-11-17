# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

BETO.888 is an Obsidian vault-based creative toolkit that transforms Obsidian notes into rich web applications using the DataCore plugin. It contains 65+ modular components that enable building interactive tools within Obsidian without traditional development setup.

## Development Commands

### Component Development
- **Open Datacore Playground**: Navigate to `_RESOURCES/DATACORE/54 DatacorePlayground/` and open `D.q.datacoreplayground.viewer.md`
- **Test Components**: Use the Datacore Playground for live component testing with hot-reload
- **Component Creation**: Follow the naming pattern `D.q.{component-name}.component.md` and `D.q.{component-name}.viewer.md`

### Version Control
- **Standard Git Operations**: Use the Git Suite Manager component (`_RESOURCES/DATACORE/60 GitSuiteManager/`)
- **Terminal Access**: Use Datacore Terminal component (`_RESOURCES/DATACORE/57 DatacoreTerminal/`) for command-line operations

### Updates
- **Vault Updates**: Use the Vault Updater component (`_RESOURCES/DATACORE/46 VaultUpdater/`) for in-vault updates
- **Manual Git Pull**: For contributors with full clone: `git pull origin main`

## Architecture & Structure

### Component Architecture
- **Core Engine**: DataCore plugin v0.1.28 by Michael Brenan
- **Language**: JavaScript/JSX domain-specific language (DSL) for Obsidian
- **UI Framework**: Custom JSX components using DataCore's reactive engine
- **State Management**: useState/useEffect hooks via DataCore API
- **Runtime**: Custom JavaScript/JSX interpreter with Preact-like component system

### File Organization
```
_RESOURCES/
├── DATACORE/           # Core component library (65+ components)
│   ├── 3 BasicView/    # Numbered folders with descriptive names
│   ├── 54 DatacorePlayground/
│   └── {component}/    # Each contains:
│       ├── D.q.{name}.component.md  # Main component
│       ├── D.q.{name}.viewer.md   # Usage example
│       └── {NAME}.md               # Documentation
├── DOCS/               # Technical documentation
└── ASSETS/             # Media libraries
```

### Component Patterns
- **Naming**: `D.q.{component-name}.component.md` for components, `D.q.{component-name}.viewer.md` for viewers
- **Exports**: Use section headers (`# ViewComponent`, `# HelperFunctions`) to organize exports
- **Imports**: `const { Component } = await dc.require(dc.headerLink(dc.resolvePath("path"), "SectionName"));`
- **Styling**: Use Obsidian CSS variables (`--background-primary`, `--text-normal`)

### Key Development Tools
1. **Datacore Playground** (#54): Monaco editor with live preview for component development
2. **Plugin Development Suite** (#62): Complete IDE for Obsidian plugin development
3. **Git Suite Manager** (#60): Visual Git client within Obsidian
4. **Datacore Terminal** (#57): Multi-tabbed terminal interface

## Development Guidelines

### Component Development
- Implement error boundaries for complex components
- Use proper cleanup in useEffect hooks
- Respect Obsidian theme settings
- Follow existing file naming conventions
- Include viewer files for all components
- Add documentation files for complex components

### Code Patterns
```jsx
// Standard component structure
function ComponentName() {
  const [state, setState] = dc.useState(initialValue);

  dc.useEffect(() => {
    // Effect logic
    return () => { /* cleanup */ };
  }, [dependencies]);

  return <div>Content</div>;
}

return { ComponentName };

// Multi-export pattern
# ViewComponent
function MainComponent() { ... }

# HelperFunctions
function utilityFunction() { ... }

return { MainComponent, utilityFunction };

// Advanced import pattern
const { Component, Utils } = await dc.require(
  dc.headerLink(dc.resolvePath("D.q.component.component.md"), "ViewComponent")
);
```

### Error Handling
```jsx
// Error boundary implementation
class ErrorBoundary extends dc.preact.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  render() {
    if (this.state.hasError) {
      return <div className="datacore-error">Error: {this.state.error.message}</div>;
    }
    return this.props.children;
  }
}
```
- Wrap complex components in error boundaries
- Provide meaningful error messages
- Handle file system operations gracefully
- Implement fallback mechanisms for external dependencies

### Testing
- Use Datacore Playground for component testing
- Test components in isolation before integration
- Verify component portability (relative pathing)
- Check theme compatibility

## Advanced Development

### Script Loading & Dynamic Imports
```jsx
// Dynamic script loading with caching
async function loadScript(dc, src, onload, onerror) {
  const cacheDir = ".datacore/script_cache";
  // Implementation handles URL vs local paths
  // Automatic caching with cache invalidation
  return new Promise((resolve, reject) => {
    // Script loading logic with fallbacks
  });
}

// ESM module support
const module = await import(/* webpackIgnore: true */ url);
```

### File System Operations
```jsx
// Vault file operations
const file = await app.vault.read(filePath);
await app.vault.write(filePath, content);
const files = await app.vault.getFiles(); // Get all files in vault
```

### Component Communication
```jsx
// Parent-child communication via props
function Parent() {
  const handleCallback = (data) => {
    console.log('Child data:', data);
  };
  return <ChildComponent onAction={handleCallback} />;
}

// Context usage for path awareness
const currentPath = dc.useCurrentPath();
```

## Important Considerations

### Performance
- Components are remastered for portability with relative pathing
- Cache management is critical for script loading
- Debounce file operations and UI updates
- Consider storage footprint (optimized from 3GB to <1GB)

### Compatibility
- All components must work with DataCore v0.1.28
- Maintain compatibility with Obsidian's theming system
- Support both light and dark themes
- Ensure mobile-friendly interfaces where applicable

### Known Limitations
- Some components have version-specific features (v1, v2, v3)
- Canvas components may have stability issues
- Globe/Map components have limited functionality
- Kanban board is in preliminary state

## Community & Support
- **Discord**: Primary support channel (#feedback for bugs/ideas)
- **License**: MIT (perpetual, no time-based transitions)
- **Contributing**: Follow existing patterns, test thoroughly
- **Updates**: Check CHANGE LOG.md for latest features/fixes