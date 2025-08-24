# IYP-BROWSER

- **Organization:** [Internet Health Report](https://github.com/InternetHealthReport)  
- **Project:** [IYP-BROWSER](https://github.com/InternetHealthReport/iyp-browser)  
- **Mentors:** Dimitrios Giakatos, Malte Tashiro  
- **Contributor:** Ronit Jain  
- **GitHub:** [Ronit0104123](https://github.com/Ronit0104123)  
- **LinkedIn:** [Ronit0104123](https://www.linkedin.com/in/ronit-jain0104/)  
- **GSoC Proposal:** [Proposal Link](https://drive.google.com/file/d/12UTzlivJ_dvYWDyHji8hPFaZCVkcXHnt/view?usp=sharing)

---

## 1. Project Goals  

The goal of my GSoC project was to enhance the **graph exploration** and **query interface** of the Internet Health Report platform, making it more interactive and user-friendly.  

To achieve this, I worked on:  

- **Node Expansion/Unexpansion in Graph View** – Allowing users to expand/unexpand nodes dynamically after clicking, enabling deeper exploration of connected data without rerunning queries.  

- **Intelligent Cypher Query Completion** – Implementing context-aware code completion that suggests only valid node properties and relationships, helping users write accurate queries more easily.  

- **Improved Query Editor Experience** – Enhancing the Cypher editor with auto-adjusting height while maintaining readability by enabling scrolling after a fixed maximum size.  


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
UNWIND connections[..1] AS conn
RETURN conn.r AS rel, conn.m AS target
```
- Added expansion/unexpansion logic in Vue, storing state in expandedNodesMap to track which nodes were expanded and remove only those on unexpansion.

- Used emits (nodeExpanded, nodeUnexpanded) to sync changes with the parent component without re-rendering the entire graph.

---

### 2.2 Intelligent Cypher Query Completion

This feature enhances the query editor in **IYP-Browser** with **intelligent, context-aware autocompletion** for Cypher queries.  
It uses the **Monaco Editor** for code editing and relies on regex-based parsing + Neo4j schema data to provide relevant suggestions.

## Key Features
- **Monaco Editor Integration** → Rich editing with syntax highlighting and autocompletion.  
- **Regex-based Context Detection** → Determines if the user is typing a node, relationship, or property.  
- **Schema-driven Suggestions** → Only valid **labels, relationships, and properties** are suggested from the Neo4j schema.  

## Regex Patterns Used

The following regex patterns are the core of query context detection:

| Regex Pattern | Purpose |
|---------------|---------|
| `/(?=MATCH|OPTIONAL MATCH|MERGE)/gi` | Splits query text whenever a new `MATCH`, `OPTIONAL MATCH`, or `MERGE` clause starts. |
| `/\(\s*\w*\s*:\s*([A-Za-z0-9_]+)/g` | Finds **node labels** inside `(alias:Label)`. |
| `/\[\s*\w*\s*:\s*([A-Za-z0-9_]+)(?=[\s\]\-]|$)/g` | Finds **relationship types** inside `[alias:TYPE]`. |
| `/(?:\(\s*\w*\s*:\s*[A-Za-z0-9_]+\s*\{\s*([A-Za-z0-9_]*))|\b([A-Za-z][A-Za-z0-9_]*)\.\s*([A-Za-z0-9_]*)$/` | Detects when user is typing **node properties** (`{ ... }`) or alias-property access (`n.name`). |
| `/\[\s*\w*\s*:\s*[A-Za-z0-9_]+\s*\{\s*([A-Za-z0-9_]*)$/` | Matches **relationship properties** inside `{ ... }`. |
| `/\b([A-Za-z][A-Za-z0-9_]*)\.\s*([A-Za-z0-9_]*)$/` | Detects alias-property typing like `a.age`. |
| `/\(\s*(\w*)\s*:\s*([A-Za-z0-9_]+)/g` | Captures both **alias + label** for nodes `(n:Person)`. |
| `/\[\s*(\w*)\s*:\s*([A-Za-z0-9_]+)/g` | Captures both **alias + type** for relationships `[r:KNOWS]`. |

---



## 3. IYP-BROWSER VS NEO4J BROWSER
- What can be improved  
- Possible extensions  
- Long-term impact  

---

## 4. Merged Pull Requests
- [GitHub Repo](https://github.com/YourUsername/your-repo)  
- [Documentation](https://yourusername.github.io/your-repo/)  
