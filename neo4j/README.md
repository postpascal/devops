###import data to neo4j:

```
docker exec -i composeflask_neo4j_1 bin/neo4j-import --into /data/databases/graph.db --nodes:Person /data/csv/person_node.csv --nodes:Movie /data/csv/movie_node.csv --nodes:Genre /data/csv/genre_node.csv --nodes:Keyword /data/csv/keyword_node.csv --relationships:ACTED_IN /data/csv/acted_in_rels.csv --relationships:DIRECTED /data/csv/directed_rels.csv --relationships:HAS_GENRE /data/csv/has_genre_rels.csv --relationships:HAS_KEYWORD /data/csv/has_keyword_rels.csv --relationships:PRODUCED /data/csv/produced_rels.csv --relationships:WRITER_OF /data/csv/writer_of_rels.csv --delimiter ";" --array-delimiter "|" --id-type INTEGER
```

```
LOAD CSV WITH HEADERS FROM 'file:///ratings.csv' AS line
MATCH (m:Movie {id:toInt(line.movie_id)})
MERGE (u:User {id:line.user_id, username:line.user_username}) // user ids are strings
MERGE (u)-[r:RATED]->(m)
SET r.rating = toInt(line.rating)
RETURN m.title, r.rating, u.username
```