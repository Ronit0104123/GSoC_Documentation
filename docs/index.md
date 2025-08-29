# IYP-BROWSER

- **Organization:** [Internet Health Report](https://github.com/InternetHealthReport)
- **Project:** [IYP-BROWSER](https://github.com/InternetHealthReport/iyp-browser)
- **Mentors:** Dimitrios Giakatos, Malte Tashiro
- **Contributor:** Ronit Jain
- **GitHub:** [Ronit0104123](https://github.com/Ronit0104123)
- **LinkedIn:** [ronit-jain](https://www.linkedin.com/in/ronit-jain0104/)
- **GSoC Proposal:** [Proposal Link](https://drive.google.com/file/d/12UTzlivJ_dvYWDyHji8hPFaZCVkcXHnt/view?usp=sharing)

---

## 1. Project Goals  

The goal of my GSoC project was to enhance the graph exploration and query interface of the Internet Yellow Pages (IYP), making it more interactive and user-friendly.  

To achieve this, I worked on:  

- **Node Expansion/Unexpansion in Graph View** – Allowing users to expand/unexpand nodes dynamically after clicking, enabling deeper exploration of connected data without rerunning queries.  

- **Intelligent Cypher Query Completion** – Implementing context-aware code completion that suggests only valid nodes, relationships, and their respective properties, helping users write accurate queries more easily.  

- **Improved Query Editor Experience** – Enhancing the Cypher editor with auto-adjusting height according to the cypher query, while maintaining readability by enabling scrolling after a fixed maximum size.  


## 2. Implementation  

The project follows a **modular and incremental approach** to add new features to the IYP-Browser, ensuring maintainability and seamless integration with the existing Neo4j-based system.  

### 2.1 Node Expansion / Unexpansion in Graph View  

- Implemented a Cypher query to fetch connected nodes and relationships dynamically:  

```cypher
MATCH (n)
WHERE elementId(n) = $nodeId
MATCH (n)-[r]-(m)
WITH type(r) AS relType, r, m
WITH relType, collect({r: r, m: m}) AS connections
UNWIND connections[..1] AS conn // Samples first connection per relationship type
RETURN conn.r AS rel, conn.m AS target
```
- Added expansion/unexpansion logic in Vue, storing state in expandedNodesMap to track which nodes were expanded and remove only those on unexpansion.

- Used emits (nodeExpanded, nodeUnexpanded) to sync changes with the parent component without re-rendering the entire graph.

### 2.2 Intelligent Cypher Query Completion

This feature enhances the query editor in **IYP-Browser** with **intelligent, context-aware autocompletion** for Cypher queries.  
It uses the **Monaco Editor** for code editing and relies on regex-based parsing + Neo4j schema data to provide relevant suggestions.

#### Key Features
- **Regex-based Context Detection** → Determines if the user is typing a node, relationship, or property and other edge cases.  
- **Schema-driven Suggestions** → Only valid **Node labels, relationships, and properties** are suggested from the Neo4j schema.  

#### Key Regex Patterns Used

Below are some of the most important regex patterns implemented in this project:

`\(\s*\w*\s*:\s*([A-Za-z0-9_]+)`  
- Extracts **node labels** from a Cypher `MATCH` clause, e.g. `(a:Person)` → `Person`.

`\[\s*\w*\s*:\s*([A-Za-z0-9_]+)(?=[\s\]\-]|$)`  
- Extracts **relationship types** from patterns, e.g. `[r:FRIENDS_WITH]` → `FRIENDS_WITH`.

`\b([A-Za-z][A-Za-z0-9_]*)\.\s*([A-Za-z0-9_]*)$`  
- Detects **alias.property** being typed, e.g. `n.name` → alias = `n`, property = `name`.

`MATCH\s+(\w+)\s*=\s*\(.*?\)`  
- Captures **path aliases** in Cypher queries, e.g. `MATCH p = (a)--(b)` → `p`.

`\[\s*\{\s*([A-Za-z0-9_]*)$`  
- Matches **relationship without type but with properties**, e.g. `[{id: 123}]`.

`\b(\w+)\s+IN\s+relationships\(\s*(\w+)\s*\)`  
- Captures **relationship alias + path variable**, e.g. `r IN relationships(p)` → `r`, `p`.

`\b(\w+)\s+IN\s+nodes\(\s*(\w+)\s*\)`  
- Captures **node alias + path variable**, e.g. `n IN nodes(p)` → `n`, `p`.



## 3. IYP-BROWSER VS NEO4J BROWSER
One major advantage of **IYP-Browser** over the standard **Neo4j Browser** is the addition of **Intelligent Cypher Query Completion**, which Neo4j Browser does not provide.

### Intelligent Cypher Query Completion
- In IYP-Browser, the editor suggests **valid node labels, relationship types, and properties** while typing.  
- Suggestions are **context-aware** (e.g., inside `MATCH`, `WHERE`, or within `{ ... }`) and are fetched dynamically from the **Neo4j schema**.  
- This means users always get **accurate, schema-driven autocompletion** instead of generic keyword hints.  

### URL-based Query Execution
- Queries can be executed directly by passing them as URL parameters.  
- This allows easy sharing of query links and reproducible results.  

### Embeddable Graph Output 
- The query result graph can be exported and embedded as an **iframe component** in other websites or dashboards.  
- This enables seamless integration of IYP-Browser outputs into external tools.  
  
---

## 4. Merged Pull Requests
The pull requests that have my work and have been merged.
- [PR #3 - auto-resize editor with max height and scroll ](https://github.com/InternetHealthReport/iyp-browser/pull/3) 
- [PR #4 - Context‑aware Cypher autocompletion](https://github.com/InternetHealthReport/iyp-browser/pull/4)
- [PR #6 - Fixes in Cypher autocompletion feature after resolving merge conflicts](https://github.com/InternetHealthReport/iyp-browser/pull/6)
- [PR #8 - added node expansion feature](https://github.com/InternetHealthReport/iyp-browser/pull/8)
- [PR #9 - implemented node unexpansion after expansion](https://github.com/InternetHealthReport/iyp-browser/pull/9)
- [PR #10 - fix re-render on node unexpansion](https://github.com/InternetHealthReport/iyp-browser/pull/10)

