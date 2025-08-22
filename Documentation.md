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

- **Node Expansion in Graph View** – Allowing users to expand nodes dynamically after clicking, enabling deeper exploration of connected data without rerunning queries.  

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

---

## 3. Future Work
- What can be improved  
- Possible extensions  
- Long-term impact  

---

## 4. Resources
- [GitHub Repo](https://github.com/YourUsername/your-repo)  
- [Documentation](https://yourusername.github.io/your-repo/)  
