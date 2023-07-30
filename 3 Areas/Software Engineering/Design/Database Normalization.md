#software #sql #database
reference: [https://www.youtube.com/watch?v=GFQaEYEc8_8](https://www.youtube.com/watch?v=GFQaEYEc8_8)

_note: usually we only need to normalize database to the 3NF, unless it is necessary to normalize further._

# 1NF

- **Rows are independent to one another**, i.e. rows do not convey information. example violation: the table `beatles` store the Beatles’ members’ name from shortest to tallest.
- **Column type must be of the same type**, i.e. no column has both string and integer values. This is usually guaranteed by the database service.
- **Every table must have a primary key.**
- **Data items must not be repeated to store a list of items**. example violation: having columns `item_1`, `item_2`, …, `item_n` on a table where the row can have variable number of items.

# 2NF

_Deletion anomaly:_ deleting a particular data causes a loss of other data mapped to the same primary key (i.e. same entity), even though they are not tightly related.

_Update anomaly:_ updating data causes inconsistency of the state of an entity, e.g. duplicate non-key attribute of a primary key value, but only one attribute is altered.

_Insert anomaly:_ inserting a particular data requires the presence of other data other than the primary key, even though they are not tightly related.

- **each non-key attribute must depend only on the entire primary key** (a.k.a. _functional dependency_)

2NF is especially salient when the primary key has multiple columns instead of just 1.

# 3NF

- **Every non-key attribute in a table should depend on the key, the whole key, and nothing but the key**, i.e. no non-key attribute should depend on another non-key attribute, as it may cause update anomaly by design.

_Boyce-Codd Normal Form_: Every ~~non-key~~ attribute in a table should depend on the key, the whole key, and nothing but the key.

# 4NF

_Multivalued dependency_: a primary key can have multiple states of data, e.g. a ‘model’ primary key may have multiple available ‘color’ values.

- **Multivalued dependencies in a table must be multivalued dependencies on the key.**

# 5NF

- **A table (which must be in 4NF) cannot be describable as the logical result of joining other tables together**. This may cause multiple sources of truth for the entity state.