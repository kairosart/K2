 

### _Full Edition — PDF Version_

---

## **Table of Contents**

1. Introduction
    
2. BloodHound v8 UI Changes
    
3. Importing Data
    
4. Graph Navigation
    
5. Legacy 4.x → v8 Query Mapping
    
6. Cypher Query Library
    
7. v8 Analyst Workflows
    
8. Neo4j Reset & Maintenance
    
9. Troubleshooting
    
10. Appendix A — Sidebar JSON Toolbox
    
11. Appendix B — Cypher Query Pack
    

---

# ----------------------------------------------------------

## 1. **Introduction**

# ----------------------------------------------------------

BloodHound v8+ is a modern graph-based Active Directory analysis platform built on Neo4j.  
This toolkit provides a complete, safe, defensive workflow for auditors, defenders, and administrators.

This guide contains:

- v8 UI navigation
    
- Full legacy → v8 query migrations
    
- Full Cypher Query Pack
    
- Sidebar toolbox data
    
- Analyst workflows
    
- Maintenance & troubleshooting
    

---

# ----------------------------------------------------------

## 2. **BloodHound v8 UI Changes**

# ----------------------------------------------------------

### **Removed from v8**

- “Analysis → Pre-Built Queries”
    
- Old graph engine
    
- Legacy collectors
    
- Sidebar query lists
    

### **Added in v8**

- New Explore Panel
    
- Object Inspector
    
- Pathfinding panel
    
- Search-centric interface
    
- Faster ingestion
    
- Improved relationship classification
    

---

# ----------------------------------------------------------

## 3. **Importing Data in BloodHound v8**

# ----------------------------------------------------------

BloodHound v8 supports:

✔ ZIP files from SharpHound  
✔ Individual JSON data files  
✔ Batch upload of mixed files

**Steps**

1. Start Neo4j
    
2. Open BloodHound
    
3. Click **Upload** (cloud icon)
    
4. Drop JSON or ZIP files
    

---

# ----------------------------------------------------------

## 4. **Graph Navigation**

# ----------------------------------------------------------

**Search Bar**

Find any:

- user
    
- computer
    
- group
    
- domain
    
- GPO
    
- high-value node
    

**Explore Panel**

Includes:

- Principals
    
- Assets
    
- Domains
    
- Edges
    
- Attack Paths
    

**Pathfinding Panel**

Used for:

- Shortest paths
    
- Inbound/outbound privilege influence
    

**Cypher Console**

For running direct Neo4j queries.

---

# ----------------------------------------------------------

# 5. **Legacy 4.x → v8 Query Mapping**

# ----------------------------------------------------------

### **A. Privilege Escalation**

#### Shortest Path to Domain Admins

**v8 Workflow:**  
Search “Domain Admins” → Open group → _Paths → Shortest paths to here_

**Cypher:**

```
MATCH (g:Group {name:"DOMAIN ADMINS@YOURDOMAIN.COM"})
CALL shortestPath(g) YIELD path
RETURN path;
```

### Shortest Path to High-Value Objects

**v8 Workflow:** Search “highvalue”

**Cypher:**

```
MATCH (n {highvalue:true})
CALL shortestPath(n) YIELD path
RETURN path;
```

## **B. Account Discovery**

### Kerberoastable Users

**v8 Workflow:** Explore → Principals → Users → Filter

**Cypher:**

```
MATCH (u:User {haskrb:true}) RETURN u;
```

### AS-REP Roastable Users

**Cypher:**

```
MATCH (u:User {dontreqpreauth:true}) RETURN u;
```

### Accounts With SPNs

**Cypher:**

```
MATCH (u:User) WHERE u.spn IS NOT NULL RETURN u;
```

## **C. Lateral Movement**

### Local Admin Rights

**Cypher:**

```
MATCH (n)-[:AdminTo]->(c:Computer)
RETURN n,c
```

### RDP Rights

**Cypher:**

```
MATCH (n)-[:CanRDP]->(c:Computer)
RETURN n,c;
```

### Active Sessions

**Cypher:**

```
MATCH (u)-[:HasSession]->(c:Computer) RETURN u,c;
```

## **D. Delegation**

### Unconstrained Delegation Computers

**Cypher:**

```
MATCH (c:Computer {unconstraineddelegation:true})
RETURN c;
```

### Resource-Based Constrained Delegation (RBCD)

**Cypher:**

```
MATCH (u)-[:AllowedToAct]->(c)
RETURN u,c;
```

## **E. GPO & ACL Analysis**

### Writable GPOs

**Cypher:**

```
MATCH (u)-[:Owns|:GenericAll|:WriteProperty]->(g:GPO)
RETURN u,g;
```

### Objects with Writable ACLs

**Cypher:**

```
MATCH (u)-[:WriteProperty]->(n)
RETURN u,n;
```

# ----------------------------------------------------------

# 6. **Cypher Query Library (Full Pack)**

# ----------------------------------------------------------

### High-Value Objects

```
MATCH (n {highvalue:true}) RETURN n;
```

Protected Users (adminCount=true)

```
MATCH (u:User {admincount:true}) RETURN u;
```

Forest / Domain Trust Map

```
MATCH (d:Domain)-[r:TrustedBy]->(t:Domain)
RETURN d,r,t;
```

Nested Groups

```
MATCH (g:Group)-[:MemberOf*2..]->(p)
RETURN g,p;
```

# ----------------------------------------------------------

# 7. **v8 Analyst Workflow Guide**

# ----------------------------------------------------------

### Step 1 — Identify high-value assets

Search “highvalue” → open → review paths.

### Step 2 — Review lateral movement

Explore → Edges → AdminTo, CanRDP, HasSession.

### Step 3 — Review delegation

Explore → Assets → Computers → Filters:

- Unconstrained delegation
    
- RBCD
    

### Step 4 — Review GPOs

Explore → Objects → GPOs → Writable GPOs.

---

# ----------------------------------------------------------

# 8. **Neo4j Reset / Maintenance**

# ----------------------------------------------------------

To clear database safely:

```
cypher-shell "MATCH (n) DETACH DELETE n;"
sudo systemctl restart neo4j
```

# ----------------------------------------------------------

# 9. **Troubleshooting**

# ----------------------------------------------------------

### Missing edges

Upload all JSON files together.

### Authentication failed

Reset password:

```
cypher-shell "ALTER USER neo4j SET PASSWORD 'NewPassword123!';"
```

### Graph freezes

Enable “reduce relationship visibility” toggle.

---

# ----------------------------------------------------------

# 10. **Appendix A — Sidebar JSON Toolbox**

# ----------------------------------------------------------

```
{
  "toolbox": [
    {
      "category": "Privilege Escalation",
      "queries": [
        {
          "name": "Shortest Path to DA",
          "cypher": "MATCH (g:Group {name:\"DOMAIN ADMINS@YOURDOMAIN.COM\"}) CALL shortestPath(g) YIELD path RETURN path;"
        },
        {
          "name": "Shortest Path to High Value",
          "cypher": "MATCH (n {highvalue:true}) CALL shortestPath(n) YIELD path RETURN path;"
        }
      ]
    },
    {
      "category": "User Discovery",
      "queries": [
        {
          "name": "Kerberoastable Users",
          "cypher": "MATCH (u:User {haskrb:true}) RETURN u;"
        },
        {
          "name": "AS-REP Roastable Users",
          "cypher": "MATCH (u:User {dontreqpreauth:true}) RETURN u;"
        }
      ]
    },
    {
      "category": "Lateral Movement",
      "queries": [
        {
          "name": "Local Admin Rights",
          "cypher": "MATCH (u)-[:AdminTo]->(c:Computer) RETURN u,c;"
        },
        {
          "name": "RDP Rights",
          "cypher": "MATCH (u)-[:CanRDP]->(c:Computer) RETURN u,c;"
        }
      ]
    }
  ]
}

```

# ----------------------------------------------------------

# 11. **Appendix B — Cypher Query Pack**

# ----------------------------------------------------------

Copy all queries from Sections 5 and 6 into a `.cypher` file for batch import.

---

# ----------------------------------------------------------

# **End of Document**

# ----------------------------------------------------------

