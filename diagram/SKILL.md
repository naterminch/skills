---
name: diagram
description: Convert long, dense, or complex text into a Mermaid diagram so the idea is seen instead of read. Use when the user asks to visualize, diagram, draw, map, or "show as a diagram", or when a long explanation/process/architecture/relationship would be clearer as a picture. Renders Mermaid to SVG via pele when available.
---

# Diagram (text to Mermaid)

Turn boring walls of text into a picture. Output a single mermaid code fence the user can read at a glance (and render with pele or any Mermaid renderer).

## When to use

- The user asks to "visualize", "diagram", "draw", "map", "flowchart", "show as a diagram", or "make this a picture".
- A long explanation of a process, system, architecture, relationship, hierarchy, timeline, decision tree, state machine, or comparison would be clearer visually.
- The user is skimming and wants to skip reading.

Do not diagram trivial one-liners or pure prose/narrative with no structure.

## Step 1 - Find the shape of the content

Before writing syntax, identify what the text actually is:

| Content shape | Diagram type |
|---|---|
| Steps / process / decision tree / branching logic | flowchart |
| Who talks to whom, request/response, protocol | sequenceDiagram |
| Hierarchy, taxonomy, brainstorm, concept map | mindmap |
| Object model, inheritance, domain model | classDiagram |
| Lifecycle, states, transitions, status flow | stateDiagram-v2 |
| Data model, entities and relations | erDiagram |
| Schedule, project plan, milestones | gantt |
| Proportions, distribution, share | pie |
| 2-axis positioning, prioritization matrix | quadrantChart |
| User experience over time | journey |
| Branching history, releases | gitGraph |
| Layout blocks, components in a grid | block |
| Flow/volume between stages | sankey-beta |
| Numeric series over categories | xychart-beta |
| Kanban board / work items by status | kanban |
| Nested proportions / budget breakdown | treemap-beta |
| Overlapping sets, Venn | venn-beta |
| System context / C4 architecture | C4Context |
| Decision-making domains (Cynefin) | cynefin-beta |
| Root-cause / fishbone | ishikawa-beta |
| Tree / file structure | treeView-beta |
| Packet / byte layout | packet-beta |
| Requirements and verification | requirementDiagram |
| Radar / skill matrix | radar-beta |
| Agent/workflow orchestration | agentflow-beta |

If two shapes fit, prefer the one with fewer nodes and clearer labels.

## Step 2 - Authoring rules (read before writing)

1. Short labels. 4-6 words per node max. Move detail to a caption or the original text.
2. Limit size. 20 nodes total max. If larger, split into 2-3 diagrams or aggregate.
3. One idea per diagram. Do not cram unrelated things together.
4. Quote labels with spaces/special chars in double quotes: A["User (admin)"].
5. Avoid Mermaid reserved words as bare node ids (end, graph, etc.); use ids like n1, a1.
6. Group with subgraphs when there are clear clusters.
7. Keep edge labels short (-- yes -->), or omit.
8. Validate by rendering with pele when available (see below) before finalizing.

## Step 3 - Syntax cheat sheet

### flowchart
```mermaid
flowchart TD
  A[Start] --> B{Decision?}
  B -- Yes --> C[Do X]
  B -- No --> D[Do Y]
  C --> E([End])
  D --> E
  subgraph Group
    C
    D
  end
```
Shapes: [] rect, () rounded, ([]) stadium, {} diamond, [()] cylinder, (( )) circle.

### mindmap
```mermaid
mindmap
  root((Topic))
    Branch A
      Leaf 1
      Leaf 2
    Branch B
```

### sequenceDiagram
```mermaid
sequenceDiagram
  participant U as User
  participant S as Server
  U->>S: Request
  S-->>U: Response
  alt ok
    S->>U: 200
  else fail
    S->>U: 500
  end
```

### timeline
```mermaid
timeline
  title History
  2020 : Event A
        : Event B
  2021 : Event C
```

### classDiagram
```mermaid
classDiagram
  Animal <|-- Duck
  Animal : +int age
  Animal : +grow()
  class Duck {
    +String beakColor
    +quack()
  }
```

### stateDiagram-v2
```mermaid
stateDiagram-v2
  [*] --> Idle
  Idle --> Running : start
  Running --> Idle : stop
  Running --> [*] : done
```

### erDiagram
```mermaid
erDiagram
  CUSTOMER ||--o{ ORDER : places
  ORDER ||--|{ LINE_ITEM : contains
  CUSTOMER {
    string name
    int id
  }
```

### gantt
```mermaid
gantt
  title Plan
  dateFormat YYYY-MM-DD
  section Build
  Design :a1, 2024-01-01, 3d
  Code   :a2, after a1, 5d
```

### pie
```mermaid
pie title Pets
  "Dogs" : 40
  "Cats" : 60
```

### quadrantChart
```mermaid
quadrantChart
  title Reach vs Impact
  x-axis Low Reach --> High Reach
  y-axis Low Impact --> High Impact
  quadrant-1 Quick wins
  A: [0.8, 0.9]
  B: [0.2, 0.3]
```

### journey
```mermaid
journey
  title Checkout
  section Browse
    View product: 5: User
  section Pay
    Enter card: 3: User
```

### gitGraph
```mermaid
gitGraph
  commit
  branch feature
  commit
  checkout main
  merge feature
```

### block
```mermaid
block-beta
  columns 2
  a["Frontend"] b["API"]
  c["DB"] d["Cache"]
  a --> b
  b --> c
```

### sankey-beta
```mermaid
sankey-beta
  Source,Stage1,10
  Stage1,Stage2,6
  Stage1,Loss,4
```

### xychart-beta
```mermaid
xychart-beta
  title Sales
  x-axis [Jan, Feb, Mar]
  y-axis 0 --> 100
  bar [30, 50, 80]
  line [25, 45, 75]
```

### kanban
```mermaid
kanban
  Todo
    Task A
    Task B
  Done
    Task C
```

### treemap-beta
```mermaid
treemap-beta
  "Budget"
    "Eng" : 60
    "Mkt" : 40
```

### venn-beta
```mermaid
venn-beta
  set A
  set B
  union A,B
```

### C4Context
```mermaid
C4Context
  title System Context
  Person(user, "User")
  System(sys, "System")
  Rel(user, sys, "Uses")
```

### agentflow-beta
```mermaid
agentflow-beta
  A[Trigger] --> B[Agent]
  B --> C{Tool?}
  C -- yes --> D[Call tool]
  D --> B
```

### cynefin-beta
```mermaid
cynefin-beta
  title Domains
```

### ishikawa-beta
```mermaid
ishikawa-beta
  title Problem
  Cause A
  Cause B
```

### treeView-beta
```mermaid
treeView-beta
  Root
    Child A
    Child B
```

### packet-beta
```mermaid
packet-beta
  0-7: "Header"
  8-15: "Type"
```

### requirementDiagram
```mermaid
requirementDiagram
  requirement r1 {
    id: 1
    text: the system shall X
    risk: high
    verifymethod: test
  }
```

### radar-beta
```mermaid
radar-beta
  title Skills
  axis A, B, C
  curve c1{1, 2, 3}
```

## Step 4 - Render with pele (optional, when available)

If the project uses pele (or you can install it), render the diagram to an SVG file so the user sees the picture immediately:

```js
// render.mjs
import { writeFileSync } from 'node:fs';
import { render } from 'pele';
const src = `flowchart LR
  A[Mermaid text] --> B[SVG]`;
const { svg } = render(src);
writeFileSync('diagram.svg', svg);
console.log('written');
```

Run with: node render.mjs. pele is synchronous and has no runtime deps.

If pele is not available, the mermaid code fence alone is enough - Obsidian, GitHub, and most markdown viewers render it.

## Output format

Always deliver:

1. A one-line caption describing what the diagram shows.
2. The diagram in a fenced block:

   ```markdown
   ```mermaid
   <diagram>
   ```
   ```

3. (Optional) the rendered .svg if pele is available and the user wants a file.

Keep the diagram the star - do not re-explain the text you just diagrammed. A short caption plus the diagram is the whole answer.