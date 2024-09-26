```python
import json
# for assembling the schema
from referencing import Registry, Resource

# for validating the schema. No particular reason to use Draft7
from jsonschema import Draft7Validator

# for reading the subschema files
from pathlib import Path
import os

```

### Opening file, loading the json

The validation happens not on the file-level, but rather after being able to
read the file. Any malformed JSON will not be processed and needs to be fixed.


```python
try:
    with open("Cr-Ni_Hreac.opt", "r") as f:
        json_data = json.load(f)
    print("File read successfully!")
except json.JSONDecodeError as e:
    print(f"Error parsing JSON")
    print(e)

```

    File read successfully!
    

### Assembling the schema

The schema is divided into a main schema `co-schema.json` that describes the
full structure, and separate sub schemata for each recipe type.

These are stored in the `recipes` directory, and need to be presented to the
validating instance of jsonschema, using a registry.


```python
# main schema
with open("schema/co-schema.json", "r") as main_file:
    main_schema = json.load(main_file)

# this is the directory where the subschemas are stored (in this repo)
SCHEMAS = Path(os.getcwd()) / Path("./schema")

# helper function that adds the ability to retrieve the subschemas from the filesystem
def retrieve_from_filesystem(uri: str):
    path = SCHEMAS / Path(uri)
    contents = json.loads(path.read_text())
    return Resource.from_contents(contents)

registry = Registry(retrieve=retrieve_from_filesystem)

# Create a validator class instance, with the registry
opt_file_validator = Draft7Validator(schema=main_schema, registry=registry)

```

### Perform the validation

Using the created validator, we can validate the opt file.


```python
try:
    opt_file_validator.validate(json_data)
    print("Instance is valid!")
except Exception as e:
    print(e)
```

    Instance is valid!
    
