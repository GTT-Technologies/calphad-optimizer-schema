# Calphad Optimizer Schema
This is the repository where the JSON schemata for Calphad Optimizer are kept.

The current release can be found on the right.

The structure of the schema is simple: co-schema.json is the schema to validate
an OPT file against, whereas there are several subschemata to validate the
experiments again.

Please note that the first version does not implement proper schema version
referencing - therefore it is not designated a 1.0 version yet.

Have a look at the [example](Example.md) to figure out how to validate, or use
[the example Jupyter Notebook](Example.ipynb).

Issues and Bugreports are welcome. Pull requests are only considered after
discussions in issues.