---
sidebar_position: 10
title: "Quick Start: Build an Automation"
description: Create a scheduled automation that runs tasks on a timer.
---

import ThemedImage from '@theme/ThemedImage';

# Quick Start: Build an Automation

**Time:** Under 10 minutes | **What you'll build:** A scheduled automation that runs tasks on a timer or manual trigger.

Automations are ideal for data synchronization, report generation, and routine maintenance jobs.

## Prerequisites

- [WSO2 Integrator extension installed](install.md)

## Step 1: Create the project

1. Open WSO2 Integrator.
2. Select **Create**.
3. Set **Integration Name** to `Integration`.
4. Select **Browse**.
5. Select the project location and select **Open**.
6. Select **Create Integration**.

<ThemedImage
    alt="Create the project in WSO2 Integrator"
    sources={{
        light: '/img/get-started/quick-start-automation/create-the-project-light.gif',
        dark: '/img/get-started/quick-start-automation/create-the-project-dark.gif',
    }}
/>

## Step 2: Add an automation artifact

1. In the design view, select **+ Add Artifact**.
2. Select **Automation** artifact.
3. Select **Create** to create an automation. This directs you to the automation diagram view.

<ThemedImage
    alt="Add an automation artifact"
    sources={{
        light: '/img/get-started/quick-start-automation/add-an-automation-artifact-light.gif',
        dark: '/img/get-started/quick-start-automation/add-an-automation-artifact-dark.gif',
    }}
/>

## Step 3: Add logic

1. Select **+** after the **Start** node to open the node panel.
2. Select **Call Function** node to the flow.
3. Select **Println** from the node panel.
4. Select **Initialize Array** from the node panel.
5. Set **Values** to `"Hello World"` and select **Save**.

<ThemedImage
    alt="Add automation logic"
    sources={{
        light: '/img/get-started/quick-start-automation/add-logic-light.gif',
        dark: '/img/get-started/quick-start-automation/add-logic-dark.gif',
    }}
/>

## Step 4: Run and test

1. Select **Run**.
2. The automation executes immediately and prints output to the terminal.
3. Check the terminal output for `Hello World`.

<ThemedImage
    alt="Run and test the automation"
    sources={{
        light: '/img/get-started/quick-start-automation/run-and-test-light.gif',
        dark: '/img/get-started/quick-start-automation/run-and-test-dark.gif',
    }}
/>

## Scheduling Automations

For production use, configure a cron schedule to trigger the automation periodically:

```ballerina
import ballerina/task;

listener task:Listener timer = new ({
    intervalInMillis: 60000  // Run every 60 seconds
});

service on timer {
    remote function onTrigger() {
        // Your automation logic here
    }
}
```

## What's Next

- [Quick Start: Integration as API](quick-start-api.md) -- Build an HTTP service
- [Quick Start: Event Integration](quick-start-event.md) -- React to messages from brokers
- [Quick Start: AI Agent](quick-start-ai-agent.md) -- Build an intelligent agent
