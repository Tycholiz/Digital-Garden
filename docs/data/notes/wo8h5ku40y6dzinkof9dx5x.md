
Document-oriented databases are inherently a subclass of the key-value store
- The difference lies in the way the data is processed: 
    - in a key-value store, the data is considered to be inherently opaque to the database, 
        - this means that while a value *can* be a complex data type (like an object), we cannot do intelligent querying based on its fields. The database only sees the object as a string and it doesn’t really care about the structure of the content it represents. 
    - a document-oriented system relies on internal structure in the document in order to extract metadata that the database engine uses for further optimization.

Document databases store all information for a given object in a single instance in the database, and every stored object can be different from every other. This eliminates the need for object-relational mapping while loading data into the database.

Document databases typically provide for additional metadata to be associated with and stored along with the document content. That metadata may be related to facilities the datastore provides for organizing documents, providing security, or other implementation specific features.

