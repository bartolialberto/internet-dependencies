# web-dependencies-migration-tool

A migration tool to import in Neo4j database dns paths and web access paths defined in the framework proposed in [1].

## Requirements

Neo4j server, Python (at least 2.5).

## Migration Tool

The paper cited above provide a definition for a dependency graph that describes dependency relationships between entities of interest: websites, landing names, zones, nameservers, IP/24 address ranges and autonomous systems. In another work, the framework have been expanded also to mail domains and mail servers.
In these works, dependencies have been collected in the form of sqlite databases, with the following structure:

![Database scheme](https://github.com/BoniFederico/web-dependencies-migration-tool/blob/main/db.jpg "Sqlite database scheme")

This software is a migration tool (composed of 3 Jupyter notebook) that import a database with the above structure in the graph-based database Neo4j.

### Nodes:

Final nodes name are the one described in the paper (except from landing_name nodes, that became web_server nodes):

- AUTONOMOUS_SYSTEM
- MAIL_DOMAIN
- MAIL_SERVER
- NAME_SERVER
- NETWORK
- ZONE
- WEB_SITE
- WEB_SERVER

### Relatioships:

Relationships are the one described in the paper (except from relationship between zones, that now are present only when a zone is the parent zone of another). Relationships name are the following:

- BELONG (From a ...\_SERVER, MAIL_DOMAIN or WEB_SERVER node to a ZONE node)
- CNAME (From a ...\_SERVER node to a ...\_SERVER node)
- COMPOSED_BY (From a ZONE node to a NAME_SERVER node)
- LOCATED (From a ...\_SERVER node to a NETWORK node)
- MANAGED_BY (From a NETWORK zone to an AUTONOMOUS_SYSTEM node)
- MAPPED_IN (From a MAIL_DOMAIN node to a MAIL_SERVER node or from a WEB_SITE node to a WEB_SERVER node)
- PARENT (From a ZONE node to its parent ZONE node)

## Configuration

The 3 Jupyter notebooks in the project have to be executed in the order specified below.

1. notebook_1 (from Sqlite database to csv files)
2. notebook_2 (from csv files to Neo4j database)
3. notebook_3 (computation of path properties for each node)

Variables defined in the first configuration block of each notebook have to be configured as described in notebooks.

The notebook notebook_2 import csv files into a Neo4j database. Thus, Neo4j server is required. It is suggested to use Neo4j Desktop, that provide also Neo4j Bloom (visualization tool).

The file perspective.json contain a useful Bloom perspective with already defined cypher queries that can be used inside Neo4j Bloom. The perspective json file can be imported into Bloom by clicking on "Import Perspective" in the Perspective Gallery (The page that is displayed when Bloom is opened).

## References

<a id="1">[1]</a>
Bartoli, A. (2021).
Robustness analysis of DNS paths and web access paths in public
administration websites. Computer Communications 180 (2021), 243-258.
