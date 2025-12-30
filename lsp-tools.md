# LSP Tools Reference

Detailed usage patterns for LSP tools in duplicate detection.

## Tool Usage Patterns

### get_symbol_references

Find all locations where a symbol is used:

```
mcp__language-server__get_symbol_references({
  "file_path": "/path/to/file.ts",
  "line": 10,
  "character": 5
})
```

Use to:
- Find all calls to a function
- Identify shared utility usage patterns
- Map dependency relationships

### find_definition

Locate where a symbol is defined:

```
mcp__language-server__find_definition({
  "file_path": "/path/to/file.ts",
  "line": 10,
  "character": 5
})
```

Use to:
- Jump to function/class implementations
- Find the source code to compare
- Build a map of all definitions

### get_hover_info

Get type and signature information:

```
mcp__language-server__get_hover_info({
  "file_path": "/path/to/file.ts",
  "line": 10,
  "character": 5
})
```

Use to:
- Compare function signatures
- Identify similar type patterns
- Check parameter compatibility

### get_diagnostics

Check for code issues in a file:

```
mcp__language-server__get_diagnostics({
  "file_path": "/path/to/file.ts"
})
```

Use to:
- Validate code before analysis
- Skip files with errors
- Find related issues

## Scanning Strategy

### Directory-First Approach

1. List all source files in target directory
2. For each file, extract top-level symbols
3. Use `get_hover_info` to get signatures
4. Group by similar signatures
5. Compare implementations within groups

### Symbol-First Approach

1. Start from known entry points (main, index)
2. Use `get_symbol_references` to find usages
3. Follow reference chains
4. Compare symbols with similar names/signatures

## Comparison Heuristics

### Quick Rejection

Skip comparison if:
- Line counts differ by >50%
- Parameter counts differ
- Return types incompatible

### Deep Comparison

Perform when:
- Similar line counts (±20%)
- Matching parameter patterns
- Same structural complexity
