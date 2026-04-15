---
sidebar_position: 3
title: Key Concepts
description: The vocabulary you need to understand WSO2 Integrator.
---

# Key Concepts

This page introduces every major product component in 2–3 sentences. Think of it as your vocabulary guide—it gives you the map. The [Develop](/docs/develop/integration-artifacts/overview) section is the territory.

## Integration

A reusable piece of business logic that connects systems, transforms data, or orchestrates workflows. Integrations are the core building blocks in WSO2 Integrator and are composed using [Services](#services), [Automations](#automations), [Event handlers](#event-handlers), and more.

## Project

A workspace that contains your integration code, dependencies, configuration, and deployment artifacts. Each project is self-contained with its own `Ballerina.toml` file. For more information, see [Project structure](/docs/develop/project-views/overview).

## Library

A shareable collection of reusable components, functions, and connectors packaged for distribution. Libraries let you build once and use across multiple projects or share with your team. For more information, see [Organize code](/docs/develop/organize-code/).

## Services

Expose your integrations over HTTP, GraphQL, gRPC, WebSocket, TCP, etc. Services are the most common artifact and define endpoints that external systems call.

## Automations

Scheduled or manually triggered integrations that run without an external request. Use automations for periodic data synchronization, cleanup tasks, or report generation.

## Event handlers

Reactive integrations triggered by messages from Kafka, RabbitMQ, NATS, or MQTT. Event handlers process streaming data in real time.

## File processors

Integrations triggered by file arrival on FTP, SFTP, or local directories. Process CSV, JSON, XML, or fixed-width files in batch or one at a time.

## AI agents

Intelligent artifacts backed by large language models (LLMs). Agents can reason, use tools, maintain conversation memory, and orchestrate multi-step workflows.t

## Connectors

Pre-built modules for connecting to external systems such as SaaS applications, databases, messaging systems, and AI services. Each connector handles authentication, serialization, and error handling. For more information, see [Connectors](/docs/connectors/overview).

## Visual data mapper

A visual data transformation tool in the WSO2 Integrator design view. Map fields between source and target schemas using the design interface, with AI-assisted suggestions. For more information, see [Visual data mapper](/docs/develop/transform/data-mapper).

## Natural functions

LLM calls treated as typed function calls in your integration code. Define an input type and output type, and the platform handles the prompt, API call, and response parsing.

## Configurations

The `Config.toml` is the file where you define environment-specific configuration such as database URLs, API keys. Different environments (dev, test, prod) have different `Config.toml` files.

## Integration control plane (ICP)

A dashboard for monitoring, managing, and troubleshooting running integrations in production. View logs, metrics, and trace requests across services. For more information, see [Integration control plane](/docs/deploy-operate/observe/icp).

## Ballerina

The programming language that powers the platform. You do not need prior experience to use the WSO2 Integrator design view, but understanding the basics of [Ballerina](https://ballerina.io/) enables advanced, code-driven capabilities.

## Low-code and pro-code

Seamless switching between the design view and the code editor in WSO2 Integrator. Changes in one view instantly appear in the other. This is not a code generation tool and it is a bidirectional synchronization.
