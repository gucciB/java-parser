# AGENTS.md

This file provides guidance to agents when working with code in this repository.

## Non-Obvious Project Details

### Multiple Main Classes (MUST configure before running)
- Three main classes exist: `App.java` (Excel only), `AppWithCsv.java` (CSV+Excel), `MethodExtractor.java` (console)
- ALL have hardcoded paths that MUST be manually updated before execution
- Default main class in build.gradle is `javaparser.test.App` (line 46)

### Critical Gotchas
- **Performance**: Excel auto-resizes columns on EVERY row insertion (lines 98 in App.java, 99 in AppWithCsv.java) - major bottleneck for large codebases
- **Row numbering inconsistency**: AppWithCsv.java starts at row 1 (line 44), App.java starts at row 0 (line 42)
- **Mixed POI versions**: Uses both Apache POI 5.3.0 and 5.5.1 (poi vs poi-ooxml)
- **Silent error handling**: Exceptions caught and printed to console only, execution continues
- **CSV escaping**: Parameters field doubles quotes (line 84 in AppWithCsv.java)

### Code Style
- No code style configuration files (no checkstyle, spotless, or formatter configs)
- Exception handling pattern: catch, print to console, continue (no throws or proper error propagation)
- Typo in variable name: `accesModifiers` (missing 's') used consistently across codebase

### Testing
- Only one test exists: verifies greeting message in App.java
- No tests for actual parsing/extraction functionality
- Run specific test: `./gradlew test --tests javaparser.test.AppTest`