# NodeRedCodepanion
Below is a detailed README.md for your Node-RED plugin project, which adds a YAML code panel to the Node-RED editor with real-time bidirectional conversion and preserves node positions during import/export. The README explains the project’s purpose, functionality, project structure, and development plan, providing enough detail for developers to implement it without including example code. The scope includes addressing your concerns about Node-RED’s JSON import/export (e.g., losing node positions, escaped code in function nodes) and your preference for a structured, YAML-based code view alongside the visual editor.
Node-RED YAML View Plugin
Overview
The Node-RED YAML View Plugin enhances the Node-RED editor by adding a YAML code panel alongside the visual flow editor. This panel displays a human-readable YAML representation of the current flow, with real-time bidirectional synchronization between the visual editor and YAML code. The plugin addresses key pain points in Node-RED’s default JSON-based interface, including verbose JSON exports, escaped code in function nodes, and loss of node positions during import/export. It provides a structured, AI-friendly, and version-control-compatible alternative for users who prefer code-based editing while retaining Node-RED’s visual capabilities.
The plugin is designed for Node-RED users, particularly those in the Home Assistant community, who want a more readable and editable flow representation without replacing the visual editor. It focuses on simplicity, avoiding features like autocomplete or custom visual builders, and uses programmatic conversion (no AI) to ensure reliability.
Features
YAML Code Panel: A sidebar or tab in the Node-RED editor displaying the YAML representation of the selected flow.
Real-Time Bidirectional Sync:
Visual to YAML: Changes in the visual editor (e.g., adding nodes, wiring) update the YAML instantly.
YAML to Visual: Edits to the YAML code are parsed and reflected in the visual editor after validation.
Node Position Preservation: Node coordinates (x, y) are stored in YAML and restored during import/export, fixing Node-RED’s JSON import issue where positions are lost.
Human-Readable YAML:
Structured format for nodes, wires, and properties, aligned with Home Assistant’s YAML conventions.
Multi-line strings for function node code to avoid JSON escaping issues.
Error Handling: Displays warnings for invalid YAML syntax or unmappable node properties, preventing broken flows.
Minimal Scope: No autocomplete, dropdowns, or custom visual editor; users rely on Node-RED’s existing editor for advanced features.
Import/Export Improvement: YAML serves as a cleaner alternative to JSON, with preserved node positions and readable function node code.
Use Case
This plugin is ideal for:
Home Assistant users who prefer YAML’s readability (e.g., similar to automations.yaml) but want Node-RED’s power.
Developers needing to copy-paste flow logic into AI tools or version control systems.
Users frustrated by Node-RED’s JSON exports, especially lost node positions and escaped code in function nodes.
Example Workflow:
Open Node-RED in Home Assistant.
Select the “YAML View” tab to see the current flow as YAML.
Modify the visual editor (e.g., add a trigger-state node); the YAML updates instantly.
Edit the YAML (e.g., change a service call’s entity_id); the visual flow updates after validation.
Copy the YAML for AI analysis or export it for version control, with node positions preserved.
Project Structure
The project is a Node-RED plugin, built as an npm module, installable via Node-RED’s palette manager or bundled with the Home Assistant Node-RED add-on. Below is the planned directory structure:
node-red-contrib-yaml-view/
├── package.json                # npm package metadata and dependencies
├── README.md                   # Project documentation
├── LICENSE                     # MIT license
├── src/
│   ├── editor.js               # Frontend logic for YAML panel and editor integration
│   ├── conversion.js           # JSON ↔ YAML conversion utilities
│   ├── validation.js           # YAML validation and error handling
│   └── plugin.js               # Node-RED plugin registration
├── static/
│   ├── codemirror/             # CodeMirror library for YAML editor
│   └── styles.css              # Custom CSS for YAML panel
├── tests/
│   ├── conversion.test.js      # Unit tests for JSON ↔ YAML conversion
│   ├── validation.test.js      # Unit tests for YAML validation
│   └── editor.test.js          # Integration tests for editor behavior
└── docs/
    ├── development.md          # Developer setup and contribution guide
    ├── api.md                  # Internal API documentation
    └── user-guide.md           # User installation and usage instructions
File Descriptions
package.json: Defines the plugin as a Node-RED module, with dependencies (js-yaml, codemirror) and scripts for build/test.
src/editor.js: Injects the YAML panel into Node-RED’s editor, handles UI rendering, and binds to editor events.
src/conversion.js: Implements bidirectional JSON ↔ YAML conversion, including node position handling.
src/validation.js: Validates YAML syntax and Node-RED compatibility, generating user-friendly errors.
src/plugin.js: Registers the plugin with Node-RED’s plugin system.
static/: Contains CodeMirror for the YAML editor and custom styles for the panel.
tests/: Mocha/Chai tests for conversion, validation, and editor integration.
docs/: Detailed guides for developers and users.
Development Plan
The project will be developed in three phases: Setup and Prototype, Core Functionality, and Polish and Release. The estimated timeline is 6–8 weeks for an MVP, assuming 10–20 hours/week per developer with JavaScript and Node-RED experience.
Phase 1: Setup and Prototype (1–2 Weeks)
Goal: Establish the project structure, set up the development environment, and create a basic YAML panel prototype.
Tasks:
Initialize an npm project with npm init node-red-node.
Add dependencies: js-yaml (YAML parsing), codemirror (code editor), mocha/chai (testing).
Set up the directory structure as described above.
Create a minimal Node-RED plugin that injects a “YAML View” tab into the editor.
Integrate CodeMirror to display static YAML in the panel.
Write a basic JSON → YAML conversion function for a single node (e.g., debug node).
Set up a test suite with one unit test for conversion.
Document developer setup in docs/development.md (e.g., Node.js version, npm install, running Node-RED locally).
Deliverables:
Working plugin scaffold with a static YAML panel.
Basic JSON → YAML conversion for one node type.
Initial test suite and documentation.
Milestone: Display a hardcoded YAML string in the Node-RED editor’s new tab.
Phase 2: Core Functionality (3–4 Weeks)
Goal: Implement bidirectional sync, node position preservation, and support for common Node-RED nodes.
Tasks:
Editor Integration:
Add the YAML panel as a sidebar or tab, styled to match Node-RED’s UI.
Listen to Node-RED editor events (e.g., flows:change, nodes:add) to trigger JSON → YAML updates.
Detect YAML changes in CodeMirror and trigger YAML → JSON updates with debouncing.
Conversion Logic:
Implement JSON → YAML conversion for common nodes (e.g., trigger-state, call-service, function, switch, debug).
Include node positions (x, y) in YAML (e.g., under position: { x: 100, y: 200 }).
Handle function node code as multi-line YAML strings to avoid JSON escaping.
Implement YAML → JSON conversion, mapping back to Node-RED’s JSON schema.
Support node connections (wires) in both directions.
Validation:
Validate YAML syntax using js-yaml.
Check for required node properties (e.g., id, type) and valid node types.
Display errors in the YAML panel (e.g., “Invalid syntax at line 5” or “Unknown node type”).
Node Position Handling:
Store x, y coordinates in YAML and restore them during YAML → JSON import.
Ensure visual editor updates node positions correctly after YAML edits.
Testing:
Write unit tests for JSON ↔ YAML conversion of common nodes.
Add integration tests for editor sync (e.g., add node visually, edit YAML, verify consistency).
Test node position preservation during round-trip import/export.
Documentation:
Update docs/api.md with conversion and validation function details.
Draft docs/user-guide.md with installation and basic usage instructions.
Deliverables:
Fully functional YAML panel with bidirectional sync.
Support for common Home Assistant nodes and node positions.
Robust error handling and test coverage.
Initial user and API documentation.
Milestone: Edit a simple flow (e.g., motion sensor → light) visually or in YAML, with positions preserved and no errors.
Phase 3: Polish and Release (1–2 Weeks)
Goal: Optimize performance, handle edge cases, and prepare for community release.
Tasks:
Optimization:
Cache parsed JSON to reduce conversion overhead for large flows.
Debounce YAML edits to prevent excessive updates.
Optimize CodeMirror rendering for large YAML files.
Edge Cases:
Support third-party nodes with a generic YAML structure (e.g., key-value pairs for unknown properties).
Handle invalid or missing wires gracefully.
Preserve flow metadata (e.g., label, disabled) in YAML.
Testing:
Add tests for edge cases (e.g., third-party nodes, large flows, invalid YAML).
Test with Home Assistant’s Node-RED add-on to ensure compatibility.
Packaging:
Update package.json with metadata (e.g., description, keywords, version).
Create a build script to bundle static assets (CodeMirror, CSS).
Test installation via Node-RED’s palette manager.
Documentation:
Finalize docs/user-guide.md with screenshots and examples.
Add contribution guidelines to docs/development.md.
Include a troubleshooting section for common issues.
Release:
Publish to npm as node-red-contrib-yaml-view.
Submit to Node-RED’s plugin library.
Share on Home Assistant Community Forum and r/nodered for feedback.
Create a GitHub release with changelog.
Deliverables:
Production-ready plugin with optimized performance.
Comprehensive test suite and documentation.
Published npm package and community announcement.
Milestone: Install the plugin in Node-RED, create/edit a complex Home Assistant flow, and export/import YAML with positions intact.
Technical Requirements
Dependencies:
js-yaml: For YAML parsing and serialization.
codemirror: For the YAML code editor.
mocha/chai: For unit and integration testing.
Node-RED Compatibility: Target Node-RED 3.x (latest stable as of June 2025).
Home Assistant Compatibility: Test with node-red-contrib-home-assistant-websocket nodes.
Development Environment:
Node.js 18.x or later.
npm 8.x or later.
Local Node-RED instance for testing.
Optional: Home Assistant instance for integration testing.
Development Guidelines
Code Style: Follow Node-RED’s JavaScript conventions (e.g., ESLint, 2-space indentation).
Testing: Aim for 80%+ test coverage, with unit tests for conversion/validation and integration tests for editor sync.
Commits: Use conventional commits (e.g., feat: add YAML panel, fix: handle function node code).
Documentation: Keep docs/ updated with every change; use clear, concise language.
Community: Engage with Node-RED and Home Assistant communities for feedback during development.
Risks and Mitigations
Complex Nodes: Third-party nodes may have undocumented properties.
Mitigation: Use a generic YAML structure for unknown nodes; log warnings for unmappable properties.
Performance: Large flows may slow conversion or UI updates.
Mitigation: Cache JSON, debounce edits, and profile CodeMirror performance.
Node-RED Updates: Future Node-RED releases may break editor APIs.
Mitigation: Test with latest Node-RED versions; use stable APIs where possible.
User Adoption: Visual editor users may not see the value.
Mitigation: Market as a complementary tool for coders; highlight YAML’s benefits in documentation.
Contributing
Contributions are welcome! See docs/development.md for setup instructions and docs/api.md for internal API details. Please follow the contribution guidelines, including:
Submit issues for bugs or feature requests.
Open pull requests with clear descriptions and tests.
Join discussions on the Node-RED or Home Assistant community forums.
License
MIT License. See LICENSE for details.
Contact
For questions or feedback, open an issue on GitHub or post in the Node-RED or Home Assistant community forums.
Notes for Developers
This README.md provides a clear roadmap for implementation, with a focus on:
Functionality: YAML panel, bidirectional sync, node position preservation.
Scope Limits: No autocomplete or custom visual editor to keep effort manageable.
Home Assistant Context: Emphasis on compatibility with Home Assistant nodes.
User Pain Points: Addresses JSON verbosity, position loss, and function node escaping.
The development plan is structured to deliver an MVP in 6–8 weeks, with milestones to track progress. Developers should:
Start with Node-RED’s plugin documentation (https://nodered.org/docs/creating-nodes/plugins).
Use js-yaml and CodeMirror for core functionality.
Test early with Home Assistant flows (e.g., trigger-state → call-service).
Engage the community for feedback to refine the plugin.
If you need further refinements to the README (e.g., adding specific YAML examples, expanding testing details), or want a separate document (e.g., development.md), let me know!
