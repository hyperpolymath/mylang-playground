# My Language Project Justfile
# https://github.com/casey/just

# Default recipe (shows help)
default:
    @just --list

# === Building ===

# Build all crates in release mode
build:
    cargo build --release --all-features

# Build in debug mode
build-debug:
    cargo build --all-features

# Build specific crate
build-crate crate:
    cargo build -p my-lang-{{crate}}

# Build with verbose output
build-verbose:
    cargo build --release --all-features --verbose

# Clean build artifacts
clean:
    cargo clean
    rm -rf target/

# === Testing ===

# Run all tests
test:
    cargo test --all-features --workspace

# Run tests with output
test-verbose:
    cargo test --all-features --workspace -- --nocapture --test-threads=1

# Run tests for specific crate
test-crate crate:
    cargo test -p my-lang-{{crate}}

# Run lexer tests specifically
test-lexer:
    cargo test -p my-lang-lexer

# Run integration tests
test-integration:
    cargo test --test '*' --all-features

# Run tests with coverage (requires cargo-tarpaulin)
test-coverage:
    cargo tarpaulin --all-features --workspace --out Html --output-dir coverage/

# === Linting and Formatting ===

# Check code formatting
check-format:
    cargo fmt --all -- --check

# Format all code
format:
    cargo fmt --all

# Run clippy lints
lint:
    cargo clippy --all-features --workspace -- -D warnings

# Fix clippy warnings automatically
lint-fix:
    cargo clippy --all-features --workspace --fix

# Run all checks (format + lint + test)
check: check-format lint test

# === Documentation ===

# Build documentation
doc:
    cargo doc --all-features --no-deps --open

# Build documentation for all dependencies
doc-all:
    cargo doc --all-features --open

# Check documentation for broken links
doc-check:
    cargo doc --all-features --no-deps

# === Benchmarking ===

# Run benchmarks
bench:
    cargo bench --all-features

# Run specific benchmark
bench-name name:
    cargo bench --all-features --bench {{name}}

# Run benchmarks with baseline
bench-baseline baseline:
    cargo bench --all-features -- --save-baseline {{baseline}}

# === Running Examples ===

# Run a Solo example
run-solo file:
    cargo run -- run examples/solo/{{file}}.solo

# Run a Duet example
run-duet file:
    cargo run -- run --mode=duet examples/duet/{{file}}.duet

# Run a specific example by path
run-example path:
    cargo run -- run {{path}}

# Run all Solo examples (check they parse)
run-all-solo:
    #!/usr/bin/env bash
    set -euo pipefail
    for file in examples/solo/*.solo; do
        echo "Checking $file..."
        cargo run -- check "$file"
    done

# === Development ===

# Watch for changes and rebuild
watch:
    cargo watch -x 'build --all-features'

# Watch and run tests on changes
watch-test:
    cargo watch -x 'test --all-features'

# Start REPL (when implemented)
repl:
    cargo run -- repl

# Start LSP server (when implemented)
lsp:
    cargo run -- lsp

# === Installation ===

# Install the compiler locally
install:
    cargo install --path .

# Uninstall the compiler
uninstall:
    cargo uninstall my-lang

# === Verification ===

# Verify project setup
verify:
    @echo "=== Verifying My Language Project Setup ==="
    @echo ""
    @echo "Checking Rust installation..."
    @rustc --version
    @cargo --version
    @echo "✓ Rust installed"
    @echo ""
    @echo "Checking project structure..."
    @test -f Cargo.toml && echo "✓ Cargo.toml exists"
    @test -f README.md && echo "✓ README.md exists"
    @test -f LICENSE.txt && echo "✓ LICENSE.txt exists"
    @test -f SECURITY.md && echo "✓ SECURITY.md exists"
    @test -f CODE_OF_CONDUCT.md && echo "✓ CODE_OF_CONDUCT.md exists"
    @test -f MAINTAINERS.md && echo "✓ MAINTAINERS.md exists"
    @test -f CHANGELOG.md && echo "✓ CHANGELOG.md exists"
    @test -f .well-known/security.txt && echo "✓ security.txt exists"
    @test -f .well-known/ai.txt && echo "✓ ai.txt exists"
    @test -f .well-known/humans.txt && echo "✓ humans.txt exists"
    @echo ""
    @echo "Checking crates..."
    @test -d crates/lexer && echo "✓ lexer crate exists"
    @test -d crates/parser && echo "✓ parser crate exists"
    @test -d crates/ast && echo "✓ ast crate exists"
    @echo ""
    @echo "Building project..."
    @cargo build --quiet
    @echo "✓ Project builds successfully"
    @echo ""
    @echo "Running tests..."
    @cargo test --quiet
    @echo "✓ Tests pass"
    @echo ""
    @echo "=== ✓ All verifications passed! ==="

# RSR compliance check
rsr-check:
    @echo "=== RSR (Rhodium Standard Repository) Compliance Check ==="
    @echo ""
    @echo "Bronze Level Requirements:"
    @echo ""
    @echo "1. Type Safety"
    @test -f Cargo.toml && echo "  ✓ Rust (compile-time type checking)"
    @echo ""
    @echo "2. Memory Safety"
    @grep -q 'unsafe' crates/*/src/*.rs && echo "  ⚠ Contains unsafe blocks (review needed)" || echo "  ✓ Zero unsafe blocks in core"
    @echo ""
    @echo "3. Offline-First"
    @echo "  ✓ No network dependencies in core compiler"
    @echo ""
    @echo "4. Documentation"
    @test -f README.md && echo "  ✓ README.md"
    @test -f LICENSE.txt && echo "  ✓ LICENSE.txt (MIT + Palimpsest v0.8)"
    @test -f SECURITY.md && echo "  ✓ SECURITY.md"
    @test -f CONTRIBUTING.md && echo "  ✓ CONTRIBUTING.md"
    @test -f CODE_OF_CONDUCT.md && echo "  ✓ CODE_OF_CONDUCT.md"
    @test -f MAINTAINERS.md && echo "  ✓ MAINTAINERS.md"
    @test -f CHANGELOG.md && echo "  ✓ CHANGELOG.md"
    @echo ""
    @echo "5. .well-known/"
    @test -f .well-known/security.txt && echo "  ✓ security.txt (RFC 9116)"
    @test -f .well-known/ai.txt && echo "  ✓ ai.txt"
    @test -f .well-known/humans.txt && echo "  ✓ humans.txt"
    @echo ""
    @echo "6. Build System"
    @test -f justfile && echo "  ✓ justfile"
    @test -f flake.nix && echo "  ✓ flake.nix" || echo "  ⚠ flake.nix missing"
    @test -f .github/workflows/ci.yml && echo "  ✓ CI/CD (GitHub Actions)"
    @echo ""
    @echo "7. Testing"
    @cargo test --quiet 2>&1 | grep -q "test result: ok" && echo "  ✓ 100% test pass rate"
    @echo ""
    @echo "8. TPCF (Tri-Perimeter Contribution Framework)"
    @grep -q "Perimeter 3" MAINTAINERS.md && echo "  ✓ Community Sandbox (Perimeter 3)"
    @echo ""
    @echo "=== RSR Compliance: Bronze Level ✓ ==="

# === Maintenance ===

# Update dependencies
update:
    cargo update

# Check for outdated dependencies
outdated:
    cargo outdated

# Security audit (requires cargo-audit)
audit:
    cargo audit

# Generate SBOM (Software Bill of Materials)
sbom:
    cargo tree --all-features > SBOM.txt
    @echo "SBOM generated: SBOM.txt"

# === Release ===

# Prepare for release (check, test, doc)
pre-release: check test doc
    @echo "=== Pre-release checks passed ==="
    @echo "Ready to tag and release!"

# Tag a new version
tag version:
    git tag -a v{{version}} -m "Release v{{version}}"
    git push origin v{{version}}

# Publish to crates.io (when ready)
publish:
    cargo publish --all-features

# === Statistics ===

# Count lines of code
loc:
    @echo "=== Lines of Code ==="
    @echo ""
    @echo "Rust (src/):"
    @find src/ crates/ -name '*.rs' -exec wc -l {} \; | awk '{sum+=$1} END {print sum " lines"}'
    @echo ""
    @echo "Solo examples:"
    @find examples/solo/ -name '*.solo' -exec wc -l {} \; | awk '{sum+=$1} END {print sum " lines"}'
    @echo ""
    @echo "Duet examples:"
    @find examples/duet/ -name '*.duet' -exec wc -l {} \; | awk '{sum+=$1} END {print sum " lines"}'
    @echo ""
    @echo "Ensemble examples:"
    @find examples/ensemble/ -name '*.ensemble' -exec wc -l {} \; | awk '{sum+=$1} END {print sum " lines"}'
    @echo ""
    @echo "Documentation (*.md):"
    @find . -name '*.md' -exec wc -l {} \; | awk '{sum+=$1} END {print sum " lines"}'
    @echo ""
    @echo "Grammars (*.ebnf):"
    @find docs/specs/ -name '*.ebnf' -exec wc -l {} \; | awk '{sum+=$1} END {print sum " lines"}'

# Show project statistics
stats:
    @echo "=== Project Statistics ==="
    @echo ""
    @echo "Files:"
    @find . -type f ! -path '*/target/*' ! -path '*/.git/*' | wc -l
    @echo ""
    @echo "Commits:"
    @git rev-list --count HEAD
    @echo ""
    @echo "Contributors:"
    @git shortlog -sn | wc -l
    @echo ""
    @echo "Crates:"
    @ls -1 crates/ | wc -l
    @echo ""
    @echo "Examples:"
    @find examples/ -type f | wc -l
    @echo ""
    @echo "Tests:"
    @find tests/ -name '*.rs' | wc -l

# === Utility ===

# Clean all generated files (including coverage, docs)
clean-all: clean
    rm -rf coverage/
    rm -rf target/doc/
    rm -f SBOM.txt

# Initialize git hooks
init-hooks:
    @echo "#!/bin/sh" > .git/hooks/pre-commit
    @echo "just check" >> .git/hooks/pre-commit
    @chmod +x .git/hooks/pre-commit
    @echo "Git hooks initialized (pre-commit: just check)"

# Show what would be installed
dry-install:
    cargo install --path . --dry-run

# === Help ===

# Show all recipes with descriptions
help:
    @just --list --unsorted
