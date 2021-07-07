## Index Operations

| Operation | In Place | Rebuilds Tables | Permits Concurrent DML | Only Modifies Metadata |
| --- | --- | --- | --- | --- |
| Creating or adding a secondary index | Yes| No| Yes| No |
| Dropping an index | Yes | No | Yes | Yes |
| Renaming an index | Yes | No | Yes | Yes |
| Adding a `Fulltext` Index |  Yes\* | No\* | No | No |
| Adding a `Spatial` index |  Yes | No | No | No |
| Changing the index type | Yes | No | Yes | Yes |

## Primary Key Operations

| Operation | In Place | Rebuilds Tables | Permits Concurrent DML | Only Modifies Metadata |
| --- | --- | --- | --- | --- |
| Adding a primary key | Yes\* | Yes\* | Yes | No |
| Dropping a primary key | No | Yes | No | No |
| Dropping a primary key and adding another | Yes | Yes | Yes | No |

## Column Operations

| Operation | In Place | Rebuilds Tables | Permits Concurrent DML | Only Modifies Metadata |
| --- | --- | --- | --- | --- |
| Adding a column | Yes | Yes | Yes\* | No |
| Dropping a column | Yes | Yes | Yes\* | No |
| Renaming a column | Yes | No | Yes\* | Yes |
| Reordering columns | Yes | Yes | Yes | No |
| Setting a column default value | Yes | No | Yes | Yes |
| Changing the column data type | No | Yes | No | No |
| Extending `Varchar` column size | Yes | No | Yes | Yes |
| Dropping the column default value | Yes | No | Yes | Yes |
| Changing the `auto-increment` value | Yes | No | Yes | No\* |
| Making a column `NULL` | Yes | Yes\* | Yes | No |
| Making a column `NOT NULL` | Yes\* | Yes\* | Yes | No |
| Modifying the definition of an `ENUM` or `SET` column | Yes | No | Yes | Yes |

## Table Operations

| Operation | In Place | Rebuilds Tables | Permits Concurrent DML | Only Modifies Metadata |
| --- | --- | --- | --- | --- |
| Changing the ROW_FORMAT |  Yes |  Yes |  Yes | No |
| Changing the KEY_BLOCK_SIZE | Yes | Yes | Yes | No |
| Setting persistent table statistics | Yes | No | Yes | Yes |
| Specifying a character set | Yes | Yes\* | No | No |
| Converting a character set | No | Yes\* | No | No |
| Optimizing a table | Yes\* | Yes | Yes | No |
| Rebuilding with the FORCE option | Yes\* | Yes | Yes | No |
| Performing a null rebuild | Yes\* | Yes | Yes | No |
| Renaming a table | Yes | No | Yes | Yes |