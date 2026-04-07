---
sidebar_position: 4
title: Migrate from TIBCO BusinessWorks
description: Migrate TIBCO BusinessWorks integrations to WSO2 Integrator with automated code generation.
---

# Migrate from TIBCO BusinessWorks

## Overview

The TIBCO migration tool converts TIBCO BusinessWorks process definitions to Ballerina code. It handles process flows, activities, transitions, shared resources, and error handling configurations.

## Run the TIBCO migration tool

```bash
# Migrate a TIBCO BusinessWorks project
bal migrate tibco -i /path/to/tibco-project/ -o migrated/

# Specify the TIBCO BusinessWorks version
bal migrate tibco -i /path/to/tibco-project/ --version 6 -o migrated/

# Generate report only
bal migrate tibco -i /path/to/tibco-project/ --report-only
```

## Component mapping

| TIBCO component | Ballerina equivalent |
|---|---|
| Process Definition | Ballerina service / function |
| HTTP Receiver | `http:Listener` + service |
| HTTP Request | `http:Client` |
| JDBC Connection | `mysql:Client` / `postgresql:Client` |
| JDBC Query / Update | Database query functions |
| XML Parse / Render | XML data binding / record types |
| Mapper Activity | Data transformation expressions |
| Choice (If/Else) | `if/else` |
| Iterate / Loop | `foreach` / `while` |
| Group (Transaction) | `transaction` block |
| Catch / Fault Handler | `do/on fail` error handler |
| Timer | `task:Listener` with scheduled job |
| JMS Receiver / Sender | JMS connector client |
| File Read / Write | File I/O functions |
| Log Activity | `log:printInfo()` / `log:printError()` |
| Sub-Process | Function call |
| Shared Variable | Module-level variable / `isolated` variable |

## Example: TIBCO process to Ballerina service

**TIBCO BusinessWorks process:**

A typical TIBCO process that receives an HTTP request, queries a database, transforms the result, and returns a response.

**Generated Ballerina code:**
```ballerina
import ballerina/http;
import ballerina/log;
import ballerina/sql;
import ballerinax/mysql;

configurable string dbHost = ?;
configurable string dbUser = ?;
configurable string dbPassword = ?;

final mysql:Client dbClient = check new (dbHost, dbUser, dbPassword, "orders_db");

service /api/orders on new http:Listener(8090) {

    resource function get [string orderId]() returns json|error {
        log:printInfo("Processing order request", orderId = orderId);

        record {|string name; decimal total;|} result = check dbClient->queryRow(
            `SELECT name, total FROM orders WHERE id = ${orderId}`
        );

        return {
            id: orderId,
            name: result.name,
            total: result.total
        };
    }
}
```

## Migration report

After migration, the tool generates a report listing successfully migrated components, items needing manual review, and unsupported features that require manual implementation.

```
Migration Report: OrderProcessing
===================================

Migrated Successfully:
  - Process: GetOrder.process -> get_order_service.bal
  - HTTP Receiver: /api/orders -> http:Listener resource
  - JDBC Query: OrderLookup -> mysql:Client query

Requires Manual Review:
  - Shared Connection: JDBC_Connection -> Needs Config.toml configuration
  - Custom Java Activity: com.example.Transform -> Rewrite or use Java interop

Unsupported (Manual Migration Required):
  - RV Transport: EventPublisher -> Replace with appropriate messaging connector
  - XSLT Transformation: OrderTransform -> Replace with Ballerina data mapping
```
