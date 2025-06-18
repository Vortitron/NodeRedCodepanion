# Node-RED YAML View Plugin

[![npm version](https://badge.fury.io/js/node-red-contrib-yaml-view.svg)](https://www.npmjs.com/package/node-red-contrib-yaml-view)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node-RED](https://img.shields.io/badge/Node--RED-3.x-red)](https://nodered.org/)
[![Build Status](https://github.com/your-org/node-red-contrib-yaml-view/workflows/CI/badge.svg)](https://github.com/your-org/node-red-contrib-yaml-view/actions)
[![Downloads](https://img.shields.io/npm/dm/node-red-contrib-yaml-view.svg)](https://www.npmjs.com/package/node-red-contrib-yaml-view)

> A powerful Node-RED plugin that adds a YAML code panel alongside the visual flow editor, providing real-time bidirectional synchronisation and preserving node positions during import/export.

## 🎯 Overview

The Node-RED YAML View Plugin enhances the Node-RED editor by addressing key pain points in the default JSON-based interface:

- **Verbose JSON exports** → Clean, readable YAML format
- **Escaped code in function nodes** → Multi-line strings in YAML
- **Lost node positions on import** → Position preservation in YAML metadata
- **Poor version control compatibility** → Git-friendly YAML structure

This plugin is particularly valuable for **Home Assistant** users who prefer YAML's readability whilst retaining Node-RED's visual capabilities.

## ✨ Key Features

### 🔄 Real-Time Bidirectional Sync
- **Visual → YAML**: Changes in the visual editor update YAML instantly
- **YAML → Visual**: YAML edits are parsed and reflected in the visual editor after validation

### 📍 Node Position Preservation
- Node coordinates (x, y) stored in YAML metadata
- Positions restored during import/export, fixing Node-RED's JSON import issues

### 📝 Human-Readable YAML Format
- Structured format aligned with Home Assistant YAML conventions
- Multi-line strings for function node code (no more JSON escaping!)
- Clean, version-control-friendly output

### 🛡️ Robust Error Handling
- Real-time YAML syntax validation
- Clear error messages for invalid configurations
- Graceful handling of unknown node types

### 🎯 Focused Scope
- No autocomplete or custom visual builders (keeps complexity manageable)
- Leverages existing Node-RED editor features
- Programmatic conversion (no AI dependencies)

## 📋 YAML Format Example

Here's how a typical Home Assistant flow looks in YAML compared to JSON:

### YAML View (Clean & Readable)
```yaml
# Motion Sensor to Light Flow
flow:
  name: "Motion Light Control"
  nodes:
    motion_sensor:
      type: "ha-state-node"
      entity_id: "binary_sensor.living_room_motion"
      position: { x: 100, y: 100 }
      
    light_control:
      type: "ha-service-node"
      service: "light.turn_on"
      entity_id: "light.living_room"
      position: { x: 300, y: 100 }
      
    delay_function:
      type: "function"
      code: |
        // Wait 5 minutes before turning off
        const delay = 300000; // 5 minutes in ms
        msg.delay = delay;
        return msg;
      position: { x: 200, y: 180 }
      
  connections:
    - from: motion_sensor
      to: light_control
      port: 0
```

### JSON View (Verbose & Escaped)
```json
[
  {
    "id": "abc123",
    "type": "server-state-changed",
    "z": "flow1",
    "name": "",
    "server": "server1",
    "version": 1,
    "entityidfilter": "binary_sensor.living_room_motion",
    "x": 100,
    "y": 100,
    "wires": [["def456"]]
  },
  {
    "id": "def456", 
    "type": "function",
    "z": "flow1",
    "name": "",
    "func": "// Wait 5 minutes before turning off\nconst delay = 300000; // 5 minutes in ms\nmsg.delay = delay;\nreturn msg;",
    "x": 200,
    "y": 180
  }
]
```

## 🚀 Quick Start

### Prerequisites

- **Node.js**: 18.x or later
- **Node-RED**: 3.x or later
- **npm**: 8.x or later

### Installation

#### Via Node-RED Palette Manager (Recommended)
1. Open Node-RED in your browser
2. Go to **Menu** → **Manage palette**
3. Click **Install** tab
4. Search for `node-red-contrib-yaml-view`
5. Click **Install**

#### Via npm
```bash
cd ~/.node-red
npm install node-red-contrib-yaml-view
```

#### For Home Assistant Users
The plugin can be bundled with the Home Assistant Node-RED add-on or installed via the palette manager.

### Basic Usage

1. **Open Node-RED** in your browser
2. **Select "YAML View"** tab to see your flow as YAML
3. **Edit visually** (e.g., add a trigger-state node) → YAML updates instantly
4. **Edit YAML** (e.g., change entity_id) → Visual flow updates after validation
5. **Copy YAML** for AI analysis or version control with preserved positions

### Configuration Options

Configure the plugin via Node-RED settings.js:

```javascript
// ... existing code ...
yamlView: {
    enabled: true,
    autoSync: true,              // Auto-sync between visual and YAML
    debounceMs: 500,            // Debounce delay for YAML changes
    preserveComments: true,      // Keep YAML comments during conversion
    maxFileSize: "1MB",         // Max flow size for YAML conversion
    showLineNumbers: true,       // Show line numbers in YAML editor
    theme: "default",           // CodeMirror theme: default, dark, etc.
    indentSpaces: 2             // YAML indentation (2 or 4 spaces)
}
// ... existing code ...
```

## 🏗️ Project Structure

```
node-red-contrib-yaml-view/
├── 📄 package.json                # npm package metadata
├── 📖 README.md                   # This file
├── 📜 LICENSE                     # MIT license
├── 🗂️ src/
│   ├── 🎨 editor.js               # Frontend YAML panel integration
│   ├── 🔄 conversion.js           # JSON ↔ YAML conversion utilities
│   ├── ✅ validation.js           # YAML validation & error handling
│   ├── 🔌 plugin.js               # Node-RED plugin registration
│   └── 🏗️ architecture.js         # Core architecture components
├── 🎭 static/
│   ├── 📝 codemirror/             # CodeMirror YAML editor library
│   ├── 🎨 styles.css              # Custom CSS for YAML panel
│   └── 🖼️ icons/                  # Plugin icons and assets
├── 🧪 tests/
│   ├── 🔄 conversion.test.js      # JSON ↔ YAML conversion tests
│   ├── ✅ validation.test.js      # YAML validation tests
│   ├── 🎨 editor.test.js          # Editor integration tests
│   └── ⚡ performance.test.js     # Performance benchmarks
├── 📚 docs/
│   ├── 🛠️ development.md          # Developer setup guide
│   ├── 📋 api.md                  # Internal API documentation
│   ├── 👤 user-guide.md           # User installation & usage
│   ├── 🏗️ architecture.md         # System architecture diagram
│   └── 📊 benchmarks.md           # Performance benchmarks
└── 🔧 .github/
    ├── workflows/ci.yml           # GitHub Actions CI/CD
    └── ISSUE_TEMPLATE/            # Issue templates
```

## 🏛️ Architecture

The plugin follows a modular architecture with clear separation of concerns:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Node-RED      │    │   YAML Panel    │    │   CodeMirror    │
│   Editor        │◄──►│   Integration   │◄──►│   Editor        │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Event         │    │   Conversion    │    │   Validation    │
│   Listeners     │    │   Engine        │    │   System        │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

*See [docs/architecture.md](docs/architecture.md) for detailed system design*
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Node-RED      │    │   YAML Panel    │    │   CodeMirror    │
│   Editor        │◄──►│   Integration   │◄──►│   Editor        │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Event         │    │   Conversion    │    │   Validation    │
│   Listeners     │    │   Engine        │    │   System        │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

*See [docs/architecture.md](docs/architecture.md) for detailed system design*

## 🛠️ Development Plan

### Phase 1: Foundation & Prototype (1-2 weeks)

**🎯 Goal**: Establish project structure and basic YAML panel

**📋 Tasks**:
- [ ] Initialize npm project with `npm init node-red-node`
- [ ] Add dependencies: `js-yaml`, `codemirror`, `mocha`, `chai`
- [ ] Create minimal Node-RED plugin with "YAML View" tab
- [ ] Integrate CodeMirror for static YAML display
- [ ] Implement basic JSON → YAML conversion for debug nodes
- [ ] Set up test suite with initial unit tests
- [ ] Document developer setup
- [ ] Create CI/CD pipeline with GitHub Actions

**🏆 Milestone**: Display hardcoded YAML in Node-RED editor tab

### Phase 2: Core Functionality (3-4 weeks)

**🎯 Goal**: Implement bidirectional sync and position preservation

**📋 Tasks**:

#### Editor Integration
- [ ] Style YAML panel to match Node-RED UI
- [ ] Listen to Node-RED events (`flows:change`, `nodes:add`)
- [ ] Detect YAML changes with debouncing
- [ ] Implement real-time visual ↔ YAML sync
- [ ] Add configuration options panel

#### Conversion Engine
- [ ] Support common nodes (trigger-state, call-service, function, switch, debug)
- [ ] Store node positions in YAML metadata
- [ ] Handle function node code as multi-line strings
- [ ] Implement bidirectional wire/connection mapping
- [ ] Add support for flow metadata (labels, groups, etc.)

#### Validation System
- [ ] Real-time YAML syntax validation
- [ ] Node property validation (id, type, etc.)
- [ ] User-friendly error display in panel
- [ ] YAML schema validation for Home Assistant nodes

#### Testing
- [ ] Unit tests for all conversion functions
- [ ] Integration tests for editor synchronisation
- [ ] Position preservation round-trip tests
- [ ] Performance benchmarks for large flows

**🏆 Milestone**: Edit simple Home Assistant flow (motion sensor → light) in either interface

### Phase 3: Polish & Release (1-2 weeks)

**🎯 Goal**: Optimise performance and prepare for release

**📋 Tasks**:
- [ ] Implement JSON caching for large flows
- [ ] Optimise CodeMirror rendering performance
- [ ] Handle third-party nodes gracefully
- [ ] Add comprehensive edge case testing
- [ ] Create build pipeline for static assets
- [ ] Finalise documentation with screenshots
- [ ] Publish to npm and Node-RED library
- [ ] Create demo video and marketing materials

**🏆 Milestone**: Production-ready plugin with full Home Assistant compatibility

## 🔧 Technical Requirements

### Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `js-yaml` | `^4.1.0` | YAML parsing and serialisation |
| `codemirror` | `^6.0.0` | Code editor component |
| `mocha` | `^10.0.0` | Testing framework |
| `chai` | `^4.3.0` | Assertion library |
| `lodash` | `^4.17.21` | Utility functions |
| `ajv` | `^8.12.0` | JSON schema validation |

### Compatibility Matrix

| Component | Version | Status |
|-----------|---------|---------|
| Node-RED | 3.x | ✅ Supported |
| Node.js | 18.x+ | ✅ Required |
| Home Assistant Nodes | Latest | ✅ Tested |
| Third-party Nodes | Any | ⚠️ Generic support |
| Browsers | Chrome 90+, Firefox 88+, Safari 14+ | ✅ Supported |

### Development Environment

```bash
# Required tools
node --version    # v18.0.0+
npm --version     # v8.0.0+

# Optional for testing
docker --version  # For Home Assistant testing
git --version     # For version control
```

## 🧪 Testing Strategy

### Unit Tests (80%+ coverage target)
- JSON ↔ YAML conversion functions
- YAML validation logic
- Position preservation algorithms
- Error handling scenarios

### Integration Tests
- Editor synchronisation scenarios
- Error handling workflows
- Performance with large flows
- Plugin lifecycle events

### End-to-End Tests
- Home Assistant node compatibility
- Import/export workflows
- Multi-user scenarios
- Browser compatibility testing

### Performance Benchmarks
- Conversion speed for various flow sizes
- Memory usage monitoring
- UI responsiveness metrics

## 🐛 Troubleshooting

### Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| YAML panel not visible | Plugin not loaded | Restart Node-RED, check palette |
| Sync not working | JavaScript errors | Check browser console |
| Invalid YAML errors | Syntax mistakes | Use YAML validator, check indentation |
| Positions lost | Old JSON import | Use YAML export/import instead |
| Performance lag | Large flows | Enable caching, reduce debounce timeout |
| Third-party nodes not converting | Unknown node schema | Check logs for warnings, use generic format |

### Performance Tips

- **Large flows**: Enable JSON caching in settings
- **Frequent edits**: Increase debounce timeout to 1000ms
- **Memory usage**: Restart Node-RED periodically for very large projects
- **Browser**: Use Chrome/Firefox for best performance

### Debug Mode

Enable debug logging in Node-RED settings:

```javascript
logging: {
    console: {
        level: "debug",
        metrics: false,
        audit: false
    }
}
```

## 🤝 Contributing

We welcome contributions! Please see our [development guide](docs/development.md) for details.

### Development Setup

```bash
# Clone repository
git clone https://github.com/your-org/node-red-contrib-yaml-view.git
cd node-red-contrib-yaml-view

# Install dependencies
npm install

# Run tests
npm test

# Run tests with coverage
npm run test:coverage

# Start development Node-RED
npm run dev

# Lint code
npm run lint

# Build for production
npm run build
```

### Contribution Guidelines

1. 🐛 **Issues**: Use GitHub issues for bugs and feature requests
2. 🔧 **Pull Requests**: Include tests and clear descriptions
3. 📝 **Code Style**: Follow Node-RED JavaScript conventions (ESLint)
4. ✅ **Testing**: Maintain 80%+ test coverage
5. 📖 **Documentation**: Update relevant docs with changes
6. 🏷️ **Commits**: Use conventional commit format
7. 🔍 **Reviews**: All PRs require review before merging

### Commit Message Format

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

Examples:
- `feat(editor): add YAML syntax highlighting`
- `fix(conversion): handle nested function node code`
- `docs(readme): update installation instructions`

## 🚨 Known Limitations & Risks

### Current Limitations
- **Third-party nodes**: Generic YAML structure for unknown properties
- **Complex wiring**: Very complex node connections may need manual adjustment
- **Performance**: Large flows (1000+ nodes) may experience slower sync
- **Browser support**: Limited to modern browsers (Chrome 90+, Firefox 88+)
- **Mobile editing**: Limited mobile device support for YAML editing

### Risk Mitigation
| Risk | Impact | Mitigation |
|------|--------|------------|
| Node-RED API changes | Plugin breaks | Test with latest versions, use stable APIs |
| Complex third-party nodes | Conversion issues | Generic fallback, community feedback |
| Performance degradation | Poor UX | Caching, debouncing, profiling |
| YAML syntax errors | Flow corruption | Real-time validation, backup system |
| Browser compatibility | Limited users | Progressive enhancement, feature detection |

## 📊 Performance Benchmarks

Expected performance characteristics:

| Flow Size | Conversion Time | Memory Usage | UI Responsiveness |
|-----------|----------------|--------------|-------------------|
| Small (1-10 nodes) | <10ms | <1MB | Instant |
| Medium (11-100 nodes) | 10-50ms | 1-5MB | <100ms |
| Large (101-500 nodes) | 50-200ms | 5-20MB | <500ms |
| Very Large (500+ nodes) | 200ms+ | 20MB+ | Variable |

*See [docs/benchmarks.md](docs/benchmarks.md) for detailed performance data*

## 🎖️ Acknowledgements

Special thanks to:
- **Node-RED development team** for the excellent platform
- **Home Assistant community** for feedback and testing
- **CodeMirror team** for the excellent editor component
- **Contributors and early adopters** who made this possible

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

## 🆘 Support & Community

- 📖 **Documentation**: [docs/user-guide.md](docs/user-guide.md)
- 🐛 **Issues**: [GitHub Issues](https://github.com/your-org/node-red-contrib-yaml-view/issues)
- 💬 **Discussion**: [Node-RED Forum](https://discourse.nodered.org/)
- 🏠 **Home Assistant**: [Community Forum](https://community.home-assistant.io/)
- 📺 **Demo Video**: [YouTube Demo](https://youtube.com/watch?v=demo)
- 📧 **Email**: support@node-red-yaml-view.com

## 🔮 Roadmap

### v1.1 (Future)
- [ ] Auto-completion for Home Assistant entities
- [ ] YAML schema validation for all node types
- [ ] Custom node templates and snippets
- [ ] Improved error messages with suggested fixes
- [ ] Dark mode theme support

### v1.2 (Future)
- [ ] Flow comparison/diff view
- [ ] Collaborative editing features
- [ ] Advanced formatting options
- [ ] Export to multiple formats (JSON, XML, etc.)
- [ ] Integration with external editors (VS Code, etc.)

### v2.0 (Long-term)
- [ ] AI-powered flow suggestions
- [ ] Visual flow builder from YAML
- [ ] Advanced debugging tools
- [ ] Multi-language support

## 🎖️ Acknowledgements

Special thanks to:
- **Node-RED development team** for the excellent platform
- **Home Assistant community** for feedback and testing
- **CodeMirror team** for the excellent editor component
- **Contributors and early adopters** who made this possible

---

*Made with ❤️ for the Node-RED and Home Assistant communities*
