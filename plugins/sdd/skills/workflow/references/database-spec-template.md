# {Feature Name} — Database Specification

## Entity-Relationship Diagram

```
┌──────────────┐       ┌──────────────┐
│   {Table A}  │───┐   │   {Table B}  │
├──────────────┤   │   ├──────────────┤
│ id (PK)      │   └──>│ id (PK)      │
│ name         │       │ table_a_id   │
│ created_at   │       │ value        │
└──────────────┘       └──────────────┘
```

## Tables

### {table_name}

{Purpose of this table.}

| Column | Type | Nullable | Default | Description |
|--------|------|----------|---------|-------------|
| id | uuid | No | gen_random_uuid() | Primary key |
| name | varchar(255) | No | — | Display name |
| status | varchar(50) | No | 'active' | Status |
| created_at | timestamptz | No | now() | Created |
| updated_at | timestamptz | No | now() | Updated |

**Constraints:** PRIMARY KEY (id), UNIQUE (name), CHECK (status IN ('active', 'inactive'))

**Indexes:**

| Name | Columns | Type | Purpose |
|------|---------|------|---------|
| idx_{table}_name | name | btree | Lookups |
| idx_{table}_status | status | btree | Filtering |

## Relationships

| From | To | Type | Description |
|------|----|------|-------------|
| {table_a} | {table_b} | one-to-many | Each A has many Bs |

## Migration Plan

### Migration 1: Create {table_name}

```sql
-- Up
CREATE TABLE {table_name} ( ... );
CREATE INDEX idx_{table}_name ON {table_name}(name);

-- Down
DROP TABLE IF EXISTS {table_name};
```

## API-to-Database Mapping

| API Field | DB Table | DB Column | Notes |
|-----------|----------|-----------|-------|
| response.id | {table} | id | Direct |
| response.name | {table} | name | Direct |
