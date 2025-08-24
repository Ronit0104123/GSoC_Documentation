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

This feature enhances the query editor with context-aware Cypher code suggestions, helping users quickly write valid queries based on the Neo4j schema.

Key Features

- Monaco Editor Integration: Provides a rich code editing experience with syntax highlighting and autocompletion.

- Regex-based Context Detection: Determines what the user is currently typing (e.g., inside MATCH, WHERE, or a relationship pattern).

- Schema-driven Suggestions: Autocompletion results are fetched dynamically from the Neo4j schema to ensure only valid node labels, relationships, and properties are suggested.

Regex Patterns Used

- /((?=MATCH|OPTIONAL MATCH|MERGE))/gi → Detects Cypher clauses.

- /\(\s*([A-Za-z0-9]*)/ → Detects node variables inside parentheses ( ).

- /\[\s*([A-Za-z0-9]*)/ → Detects relationship variables inside brackets [ ].

## 3. Future Work
- What can be improved  
- Possible extensions  
- Long-term impact  

---

## 4. Resources
- [GitHub Repo](https://github.com/YourUsername/your-repo)  
- [Documentation](https://yourusername.github.io/your-repo/)  
