# sql-library
A collection of useful SQL queries and view definitions for Panoply Data Sources

## Directory structure
Every Panoply data source should have its own directory in the top level of the repo. Each data source directory should have two subdirectories: `views` and `queries`.

For example:
```
/
    /salesforce
        /views
        /queries
    /shopify
        /views
        /queries
    /...
```

### `/views`
`views` are queries that perform significant ELT transformations and create tables to facilitate analysis and reporting. This will often be used to join and normalize tables that were created from nested JSON or create roll-up reporting tables that are easy for non-technical BI tool users to use with their BI tool of choice. Simply put: `views` are building blocks for queries.

### `/queries`
`queries` demonstrate how you can query your data in Panoply. Most `queries` will probably fall into one of two buckets: "metrics," calculations like Customer Lifetime Value that can be further segmented out; and questions, like "What is the best day of the week to post on Instagram?”

## Markdown file templates
Each Panoply data source will have its own directory modeled after the `.github/data-source-template` directory. Within that directory, there are two subdirectories: `/views` and `/queries`, each of these contains a corresponding template markdown file.

The main differences between the two templates are: `views` files have the `CREATE VIEW` syntax commented out. The `CREATE VIEW` syntax is not necessary if saving the view in Panoply. The line can also be uncommented when saving the view in other SQL workbenches. `views` files also contain on example query that could be used with the view definition. Finally, `queries` files contain a data dictionary of the columns that the query returns.
