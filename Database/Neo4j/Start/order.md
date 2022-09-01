### Order

#### 1. Create

```cypher
CREATE (n:Person { name: 'A', born: 1960}) return n;
CREATE (n:Person { name: 'B', born: 1980 }) return n;
CREATE (n:Movie { title: 'S', released: 2001 }) return n;
CREATE (n:Worker:Person{ name: 'vic'}) return n;
                   
MATCH(a:Person{name: 'A'}), (b:Movie{title:'S'}) 
     CREATE (a)-[r:Directed]->(b) return r;
MATCH(a:Person{name: 'B'}), (b:Movie{title:'S'})
     CREATE (a)-[r:ActedIn{roles:['F']}]->(b) return r;
```

```cypher
CREATE INDEX ON :Person(firstname)
CREATE INDEX ON :Person(firstname, surname)
```

```cypher
// 节点若具有指定属性，则必须唯一
CREATE CONSTRAINT ON (b:Book) ASSERT b.isbn IS UNIQUE;
// 节点必须有指定属性
CREATE CONSTRAINT ON (b:Book) ASSERT exists(b.isbn);
// 关系必须有指定属性
CREATE CONSTRAINT ON ()-[l:LIKED]-() ASSERT exists(l.day);
// 节点指定属性必须存在，且组合唯一
CREATE CONSTRAINT ON (n:Person) ASSERT (n.firstname, n.surname) IS NODE KEY;
```

#### 2. Match

```cypher
MATCH(n) return n;
MATCH(n) where n.born < 1990 return n;
MATCH(n:Movie) return n;  // 按label 查
MATCH(n{name:'A'}) return n;  // 按属性查
         
MATCH(n)--(m:Movie) return n; // 与Movie 有关的节点
MATCH(:Person{name:'A'})-->(m) return m; //与A 有关的节点
MATCH(:Person{name:'A'})-[r]->(m) return r, type(r);
// 与A 有关的关系，返回关系和关系的type (Directed)
MATCH(n)-[:ActedIn{roles:['F']}]->(m) return n, m;
// 有ActedIn 且roles:['F']的左右节点
```

#### 3. Set

```cypher
MATCH(n) where id(n)=10 SET n.name='neo' return n;
MATCH(n) where id(n)=10 SET n:Company return n;
// (n)-[r]->(m)  SET r.team='M'
```

