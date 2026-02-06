# Language Support Reference

## Supported Languages

| Language | Linter | Formatter | File Extensions |
|----------|--------|-----------|-----------------|
| C++ | cpplint, clang-tidy | clang-format | .cpp, .cc, .cxx, .h, .hpp |
| Python | ruff, pylint, flake8 | ruff, black, autopep8 | .py |
| JavaScript | eslint | prettier | .js, .jsx, .mjs |
| TypeScript | eslint | prettier | .ts, .tsx |
| Go | golangci-lint | gofmt | .go |
| Rust | clippy | rustfmt | .rs |
| Java | checkstyle | google-java-format | .java |
| Kotlin | ktlint | ktlint | .kt, .kts |
| Swift | swiftlint | swiftformat | .swift |

## C++ Configuration

```toml
[languages.cpp]
enabled = true
standard = "c++17"  # c++11, c++14, c++17, c++20
linter = "cpplint"  # cpplint or clang-tidy
formatter = "clang-format"
include_dirs = ["include", "src"]
extra_args = [
    "--filter=-whitespace/line_length",
    "--filter=-build/include_subdir",
    "--filter=-readability/todo"
]
```

### cpplint Filters

Common filters to disable:
- `-whitespace/line_length` - Line length warnings
- `-build/include_subdir` - Include path warnings
- `-readability/todo` - TODO comment warnings
- `-build/header_guard` - Header guard format

## Python Configuration

```toml
[languages.python]
enabled = true
linter = "ruff"  # ruff (recommended), pylint, flake8
formatter = "ruff"  # ruff (recommended), black, autopep8
line_length = 100
target_version = "py311"
```

### Ruff vs Pylint

| Feature | Ruff | Pylint |
|---------|------|--------|
| Speed | 10-100x faster | Slower |
| Rules | 700+ rules | 300+ rules |
| Auto-fix | Extensive | Limited |
| Config | pyproject.toml | .pylintrc |

## JavaScript/TypeScript Configuration

```toml
[languages.javascript]
enabled = true
linter = "eslint"
formatter = "prettier"
config_file = ".eslintrc.js"

[languages.typescript]
enabled = true
linter = "eslint"
formatter = "prettier"
config_file = ".eslintrc.js"
```

## Go Configuration

```toml
[languages.go]
enabled = true
linter = "golangci-lint"
formatter = "gofmt"
extra_args = ["--timeout=5m"]
```

### golangci-lint Linters

Default enabled linters:
- errcheck
- gosimple
- govet
- ineffassign
- staticcheck
- unused

## Rust Configuration

```toml
[languages.rust]
enabled = true
linter = "clippy"
formatter = "rustfmt"
extra_args = ["--", "-W", "clippy::pedantic"]
```

## Language Detection

Linthis automatically detects languages based on:
1. File extensions
2. Project configuration files (Cargo.toml, package.json, etc.)
3. Shebang lines (#!)

To override detection:
```toml
[general]
# Force specific languages only
languages = ["cpp", "python"]
```
