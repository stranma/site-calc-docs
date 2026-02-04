# Versioning Strategy

This document describes the versioning strategy for the site-calc monorepo, ensuring client-server compatibility across all packages.

## Overview

All packages in the monorepo follow **Semantic Versioning (SemVer)** with a **Minor Lock** rule for client-server compatibility.

### Packages

| Package | PyPI Name | Location |
|---------|-----------|----------|
| Core Library | `site-calc-core` | `site_calc/` |
| Server | `site-calc-server` | `server/` |
| Investment Client | `site-calc-investment` | `client-investment/` |
| Operational Client | `site-calc-operational` | `client-operational/` |

---

## Version Format

```
MAJOR.MINOR.PATCH
```

- **MAJOR**: Breaking changes, incompatible API changes
- **MINOR**: New features, backward-compatible API additions
- **PATCH**: Bug fixes, backward-compatible patches

---

## Compatibility Rule: Minor Lock

**Rule: Client MAJOR.MINOR must match Server MAJOR.MINOR. Patch can differ.**

### Compatibility Matrix

| Client | Server | Compatible? |
|--------|--------|-------------|
| 1.2.3  | 1.2.7  | YES |
| 1.2.3  | 1.3.0  | NO |
| 1.3.0  | 1.2.5  | NO |
| 2.0.0  | 1.9.9  | NO |

### Why This Approach

1. **Balances simplicity and flexibility** - Patch releases are independent
2. **Matches API versioning** - Client 1.x works with `/api/v1/`
3. **Supports submodule workflow** - Each package can have independent patches
4. **Clear upgrade path** - Minor/major bumps require coordinated release

---

## Version Sources

Each package maintains version in two locations that MUST be synchronized:

1. **`pyproject.toml`** - Package metadata version
2. **`__init__.py`** - Runtime version (`__version__`)

---

## Version Validation

### Client-Side Validation

Both clients (`InvestmentClient` and `OperationalClient`) perform automatic version checking:

```python
client = InvestmentClient(base_url="...", api_key="inv_...")
# On first API call, client fetches /health and compares versions
# Warning emitted if MAJOR.MINOR mismatch detected
```

### Server-Side Version Info

The server exposes version information:

**Health Endpoint Response:**
```json
{
  "status": "healthy",
  "version": "1.0.0",
  "api_version": "1.0",
  "environment": "production",
  "timestamp": "2025-01-31T12:00:00Z"
}
```

**Response Headers:**
```
X-API-Version: 1.0
```

---

## Version Bumping

Use the `bump_version.py` script to manage versions across all packages.

### Commands

```bash
# Check current versions
python scripts/bump_version.py --check

# Preview changes (dry run)
python scripts/bump_version.py minor --dry-run

# Bump all packages to next minor version
python scripts/bump_version.py minor       # 1.0.0 -> 1.1.0

# Bump all packages to next major version
python scripts/bump_version.py major       # 1.0.0 -> 2.0.0

# Bump specific package patch version
python scripts/bump_version.py patch server     # server: 1.0.0 -> 1.0.1
python scripts/bump_version.py patch core       # core: 1.0.0 -> 1.0.1
python scripts/bump_version.py patch investment # investment: 1.0.0 -> 1.0.1

# Bump all packages patch version
python scripts/bump_version.py patch
```

### Rules Enforced

- **Minor/Major bumps**: MUST update ALL packages (enforces MAJOR.MINOR sync)
- **Patch bumps**: Can update individual packages or all packages

---

## Version Validation Script

Use `check_versions.py` to validate version consistency:

```bash
# Check and display version status
python scripts/check_versions.py

# Quiet mode (exit code only, for CI)
python scripts/check_versions.py --quiet
```

**Exit Codes:**
- `0` - All versions valid and in sync
- `1` - Version mismatch or validation error

---

## CI/CD Integration

### Pre-Commit Hook

Add to `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: local
    hooks:
      - id: check-versions
        name: Check package versions
        entry: python scripts/check_versions.py --quiet
        language: system
        pass_filenames: false
        always_run: true
```

### GitHub Actions

```yaml
- name: Verify versions
  run: python scripts/check_versions.py

- name: Check version sync
  run: |
    if ! python scripts/check_versions.py --quiet; then
      echo "Version mismatch detected!"
      exit 1
    fi
```

---

## Release Process

### Minor/Major Release (Coordinated)

1. Decide on new version (e.g., `1.1.0`)
2. **Update changelogs:**
   - `client-investment/CHANGELOG.md`
   - `client-operational/CHANGELOG.md` (if applicable)
   - `site_calc_docs/VERSIONING.md` version history table
3. Run version bump:
   ```bash
   python scripts/bump_version.py minor
   ```
4. Run tests:
   ```bash
   cd site_calc && uv run pytest tests/
   cd client-investment && uv run pytest tests/
   cd server && uv run pytest tests/
   ```
5. Commit and tag:
   ```bash
   git add -A
   git commit -m "chore: Bump version to 1.1.0"
   git tag v1.1.0
   git push && git push --tags
   ```
6. Deploy server with new version
7. Publish client packages to PyPI

### Patch Release (Individual)

1. Apply bug fix to specific package
2. Bump patch version:
   ```bash
   python scripts/bump_version.py patch server
   ```
3. Run tests for affected package
4. Commit and deploy

---

## Troubleshooting

### Version Mismatch Warning

If you see:
```
UserWarning: Client version 1.0.0 (API 1.0) may not be compatible with server API 1.1.
Consider upgrading.
```

**Resolution:**
- Upgrade client: `pip install --upgrade site-calc-investment`
- Or downgrade server to match client version

### pyproject.toml and __init__.py Mismatch

If `check_versions.py` reports mismatch between files:

```bash
# Fix by running bump_version
python scripts/bump_version.py patch <package-name>
```

Or manually ensure both files have the same version.

---

## Current Versions

| Package | Version | Tag | Registry |
|---------|---------|-----|----------|
| Core Library | 1.2.0 | - | private |
| Server | 1.2.0 | v1.2.0 | - |
| Investment Client | 1.2.3 | v1.2.3 | PyPI |
| Operational Client | 1.2.0 | - | PyPI |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.2.3 | 2026-02-04 | save_data_file MCP tool, INVESTMENT_DATA_DIR config (investment client patch) |
| 1.2.2 | 2026-02-04 | MCP server for LLM-driven investment planning (investment client patch) |
| 1.2.0 | 2026-02-03 | Multi-environment support, API key lifecycle via PostgreSQL, infrastructure cleanup |
| 1.1.0 | 2026-02-01 | SOC anchoring, HiGHS solver default, version validation, timeout control |
| 1.0.0 | 2025-01-31 | Initial synchronized release |

---

## Related Documentation

- [Server Specification](../server/docs/SERVER_SPEC.md)
- [Investment Client](../client-investment/README.md)
- [Operational Client](../client-operational/README.md)
