# Mongo JSONSchema

Reverse engineer JSONSchemas from a MongoDB database.

## Installation

`pip install mongo-jsonschema`

## Usage

### As CLI Tool

`python -m mongo_jsonschema [-h] [-c COLLECTIONS] [-p PORT] [-s SAMPLE_SIZE] [-r {PRIMARY,PRIMARY_PREFERRED,SECONDARY,SECONDARY_PREFERRED,NEAREST}] [-d DESTINATION] [-e] host db`

#### Positional Arguments
| argument | description |
| :-: | :-- |
| `host` | A MongoDB hostname or connection string. |
| `db` | The name of the database in which the collections reside. |

#### Optional Arguments
|flags|example argument|description
|--:| :-: |:--|
| `-h`, `--help` | n/a | show the help message |
| `-c`, `--collections` | collectionOne,collectionTwo | A comma separated list of collections to generate schemas from. If blank, schemas will be generated for all collections. |
| `-p`, `--port` | 27017 | The port to use when connecting to the MongoDB server. Required if host is not a connectionstring. |
| `-s`, `--sample_size` | .15 | A decimal representation of the percentage of total documents in the collection to sample when deriving the schema. Default is .33. |
| `-r, --read_preference` | Any of PRIMARY, PRIMARY_PREFERRED, SECONDARY, SECONDARY_PREFERRED, NEAREST | The read preference to use when querying the MongoDB database. Default is 'SECONDARY'. |
| `-d`, `--destination` | some/path/value | The directory to output the generated schemas to. If none is specified, will be output to the current working directory. |
| `-e`, `--external_sorting` | n/a | Enables external sorting, using the disk on the mongodb server, for aggregations that exceed the memory limit. If false, the aggregation fails due to an exceeded memory limit, the schema will be skipped. |


### As A Library

```python
from mongo_jsonschema import SchemaGenerator

# Initialize with your mongodb hostname and port
schema_generator = SchemaGenerator('localhost', 27017)
# Or use a connection string
schema_generator = SchemaGenerator('mongodb+srv://username:password@localhost/mydatabase')

#Generate schema for a multiple collections
schemas = schema_generator.get_schemas(
    db='mydatabase', 
    collections=['foo','bar'],
    sample_percent=.05
)

# Generate schema for a single collection.
schema = schema_generator.get_schemas(
    db='dbname',
    collection='baz',
    sample_percent=.05
)
