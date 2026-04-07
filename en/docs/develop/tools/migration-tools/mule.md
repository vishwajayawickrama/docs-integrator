---
sidebar_position: 3
title: Migrate from MuleSoft
description: Migrate MuleSoft integrations to WSO2 Integrator with automated code generation.
---

# Migrate from MuleSoft

## Overview

The MuleSoft migration tool converts MuleSoft Anypoint flows (XML configurations) to Ballerina code. It handles HTTP listeners, request connectors, DataWeave transformations, routers, and error handling patterns.

## Run the MuleSoft migration tool

```bash
# Migrate a MuleSoft project
bal migrate mule -i /path/to/mule-project/ -o migrated/

# Specify the MuleSoft version
bal migrate mule -i /path/to/mule-project/ --version 4 -o migrated/

# Generate report only
bal migrate mule -i /path/to/mule-project/ --report-only
```

## Component mapping

| MuleSoft component | Ballerina equivalent |
|---|---|
| HTTP Listener | `http:Listener` + service |
| HTTP Request | `http:Client` |
| Database Connector | `mysql:Client` / `postgresql:Client` |
| Transform (DataWeave) | Ballerina query expressions + data mapper |
| Flow Reference | Function call |
| Choice Router | `if/else` or `match` |
| For Each | `foreach` |
| Scatter-Gather | Workers (parallel execution) |
| Try | `do/on fail` |
| Object Store | Configurable state management |
| Scheduler | `task:Listener` |

## Example: MuleSoft flow to Ballerina

**MuleSoft flow (XML):**
```xml
<flow name="getOrderFlow">
    <http:listener config-ref="HTTP_Listener" path="/orders/{orderId}" method="GET"/>
    <logger message="Received order request for #[attributes.uriParams.orderId]" level="INFO"/>
    <http:request config-ref="Backend_Request" method="GET"
                  path="/orders/#[attributes.uriParams.orderId]"/>
    <ee:transform>
        <ee:message>
            <ee:set-payload><![CDATA[%dw 2.0
                output application/json
                ---
                {
                    id: payload.orderId,
                    customer: payload.customerName,
                    total: payload.amount
                }]]>
            </ee:set-payload>
        </ee:message>
    </ee:transform>
</flow>
```

**Generated Ballerina code:**
```ballerina
import ballerina/http;
import ballerina/log;

configurable string backendUrl = ?;

final http:Client backendClient = check new (backendUrl);

service /orders on new http:Listener(8090) {

    resource function get [string orderId]() returns json|error {
        log:printInfo("Received order request", orderId = orderId);

        json payload = check backendClient->get("/orders/" + orderId);

        // Transformed from DataWeave
        return {
            id: check payload.orderId,
            customer: check payload.customerName,
            total: check payload.amount
        };
    }
}
```
