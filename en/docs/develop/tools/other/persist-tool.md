---
sidebar_position: 2
title: Persist tool
description: Generate type-safe data persistence clients for multiple data stores using bal persist.
---

# Persist tool

The `bal persist` tool generates type-safe client APIs for data persistence across multiple data stores. You define your data model using Ballerina record types, and the tool generates the client code to perform CRUD operations without writing store-specific queries.

## Supported data stores

| Data store | Module |
|---|---|
| In-memory tables | Built-in |
| MySQL | `ballerinax/persist.sql` |
| PostgreSQL | `ballerinax/persist.sql` |
| MSSQL | `ballerinax/persist.sql` |
| Google Sheets | `ballerinax/persist.googlesheets` |
| Redis | `ballerinax/persist.redis` |

## Initialize a persist project

```bash
# Add persist support to an existing project
bal persist add

# Initialize persist directory and model file
bal persist init
```

This creates a `persist` directory with a `model.bal` file where you define your data model.

## Define a data model

Define entities as Ballerina record types in `persist/model.bal`. Mark primary key fields as `readonly`:

```ballerina
type Employee record {|
    readonly int id;
    string name;
    string department;
    decimal salary;
|};

type Department record {|
    readonly string code;
    string name;
    Employee[] employees;
|};
```

## Generate the client

```bash
# Generate client API (integrates with build)
bal persist generate --datastore mysql

# Introspect an existing database to generate the model
bal persist pull --datastore mysql --host localhost --port 3306 --user root --database mydb
```

## Use the generated client

The generated client provides type-safe resource methods for CRUD operations:

```ballerina
import ballerina/persist;

configurable string dbHost = ?;
configurable string dbUser = ?;
configurable string dbPassword = ?;

final Client sClient = check new ();

// Create a record
int[]|error ids = sClient->/employees.post([{
    id: 1, name: "Alice", department: "Engineering", salary: 5000.0
}]);

// Read a record by key
Employee|error emp = sClient->/employees/1;

// Update a record
Employee|error updated = sClient->/employees/1.put({salary: 6000.0});

// Delete a record
Employee|error deleted = sClient->/employees/1.delete();

// List all records
stream<Employee, error?> employees = sClient->/employees;
```

## Command reference

| Command | Description |
|---|---|
| `bal persist init` | Initialize persist directory and model file |
| `bal persist add` | Add persist support and configure build integration |
| `bal persist generate` | One-time client generation without build integration |
| `bal persist pull` | Introspect an existing database to generate the model |
| `bal persist migrate` | Generate SQL migration scripts (experimental) |
| `--datastore <type>` | Target data store (`mysql`, `mssql`, `postgresql`, `googlesheets`, `redis`, `inmemory`) |
