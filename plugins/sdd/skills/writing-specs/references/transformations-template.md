# {Feature Name} — Data Transformations

## Overview

{What data is being transformed, from where to where.}

## Field Mapping

| Source Field | Target Field | Transform | Required | Default |
|-------------|-------------|-----------|----------|---------|
| source.field_a | target.field_a | Direct copy | Yes | — |
| source.field_b | target.field_b | Integer → string, zero-pad 6 | Yes | "000000" |
| source.nested.c | target.field_c | Flatten + uppercase | No | null |

## Transformation Rules

### Rule 1: {Name}

- **Input**: `source.field_b` (integer)
- **Output**: `target.field_b` (string)
- **Logic**: {conversion logic}
- **Example**: `123` → `"000123"`
- **Edge cases**: null → default, negative → {behavior}

## Validation Rules

| Field | Rule | Error Action |
|-------|------|-------------|
| source.field_a | Non-empty, max 255 | Reject record |
| source.field_b | Integer >= 0 | Use default |

## Error Handling

| Error Type | Strategy |
|------------|----------|
| Missing required field | Reject record, log error |
| Invalid format | Use default, log warning |
| Duplicate | Skip, keep existing |

## Examples

### Normal

**Input:** `{ "field_a": "hello", "field_b": 42 }`
**Output:** `{ "field_a": "hello", "field_b": "000042" }`
