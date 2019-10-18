# sql-library
A collection of useful view definitions and SQL queries for Panoply Data Sources

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
`queries` will probably fall into one of two buckets: "metrics," a single calculation like Customer Lifetime Value, and questions, eg. "What is the best day of the week to post on Instagram?”

## Markdown file templates
`views` and `queries` files each have their own template. The main differences are: `views` files have the `CREATE VIEW` syntax commented out, if the view is to be saved directly in Panoply, or to be uncommented for use in other SQL workbenches. `views` files contain on example query that could be used with the view definition. And `queries` files contain a data dictionary of the columns that are returned by the query.

Each template has a front matter section (shown below) so that Jekyll can incorporate these pages into the marketing site.

```
---
layout: view
title: Hubspot Contacts Base View
---
```
