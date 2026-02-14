# {Page Name}

## Purpose

{What the user accomplishes on this page.}

## URL

`/path/to/page`

## Wireframe

```
┌─────────────────────────────────────┐
│  Header / Navigation                │
├─────────────────────────────────────┤
│  ┌─────────┐  ┌──────────────────┐  │
│  │ Sidebar │  │  Main Content    │  │
│  │ - Nav 1 │  │  ┌────────────┐  │  │
│  │ - Nav 2 │  │  │  Card      │  │  │
│  │ - Nav 3 │  │  │  {data}    │  │  │
│  │         │  │  └────────────┘  │  │
│  └─────────┘  └──────────────────┘  │
├─────────────────────────────────────┤
│  Footer                             │
└─────────────────────────────────────┘
```

## Components

| Component | Description | Data Needed |
|-----------|-------------|-------------|
| {component} | {what it renders} | {API fields} |

## State Matrix

| State | Condition | What User Sees |
|-------|-----------|----------------|
| Loading | Data fetching | Skeleton/spinner |
| Empty | No data | Empty state + CTA |
| Error | API error | Error message + retry |
| Populated | Data available | Normal content |

## User Interactions

| Action | Trigger | Result | API Call |
|--------|---------|--------|----------|
| {action} | Click button | {result} | POST /api/... |

## Data Requirements

| UI Element | API Endpoint | Response Field |
|------------|-------------|----------------|
| {element} | GET /api/... | response.field |
