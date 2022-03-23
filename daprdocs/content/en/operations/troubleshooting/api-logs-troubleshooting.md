---
type: docs
title: "Dapr API Logs"
linkTitle: "API Logs"
weight: 3000
description: "Understand how API logging works in Dapr and how to view logs"
---

API logging enables you to see the API calls from your application to the Dapr sidecar and debug issues as a result with different verbosity levels. You can also combine Dapr API logging with Dapr log events (see [configure and view Dapr Logs]({{< ref "logs-troubleshooting.md" >}}) into the output, if you want to use the logging capabilities together.

## Overview

API logs have different, configurable verbosity levels and is applicable for both HTTP and GRPC calls.

1. info
2. debug

The default level is debug, which provides a balanced amount of information for operating Dapr in normal conditions.

To set the output level, you can use the `--api-log-level` command-line option. For example:

```bash
./daprd --api-log-level info
./daprd --api-log-level debug
```

This starts the Dapr runtime with a api log level of `info` and `debug` accordingly.

## Configuring API logging in self hosted mode

To set the log level when running your app with the Dapr CLI, pass the `api-log-level` param:

```bash
dapr run --api-log-level info node myapp.js
```

### Viewing API logs in self hosted mode

When running Dapr with the Dapr CLI, both your app's log output and the Dapr runtime log output are redirected to the same session, for easy debugging.

The below example shows `info` level API logging:

```bash
dapr run --api-log-level info node myapp.js

ℹ️  Starting Dapr with id order-processor on port 56730
✅  You are up and running! Both Dapr and your app logs will appear here.
.....
INFO[0000] HTTP API Called: POST /v1.0/state/statestore  app_id=order-processor instance=QTM-SWATHIKIL-1.redmond.corp.microsoft.com scope=dapr.runtime.http type=log ver=edge
== APP == INFO:root:Saving Order: {'orderId': '483'}
INFO[0000] HTTP API Called: GET /v1.0/state/statestore/483  app_id=order-processor instance=QTM-SWATHIKIL-1.redmond.corp.microsoft.com scope=dapr.runtime.http type=log ver=edge
== APP == INFO:root:Getting Order: {'orderId': '483'}
INFO[0000] HTTP API Called: DELETE /v1.0/state/statestore  app_id=order-processor instance=QTM-SWATHIKIL-1.redmond.corp.microsoft.com scope=dapr.runtime.http type=log ver=edge
== APP == INFO:root:Deleted Order: {'orderId': '483'}
INFO[0000] HTTP API Called: PUT /v1.0/metadata/cliPID    app_id=order-processor instance=QTM-SWATHIKIL-1.redmond.corp.microsoft.com scope=dapr.runtime.http type=log ver=edge
```

The below example is for debug level API logging

```bash
dapr run --api-log-level debug node myapp.js
ℹ️  Starting Dapr with id order-processor on port 56730
✅  You are up and running! Both Dapr and your app logs will appear here.
.....
DEBU[0000] HTTP API Called: POST /v1.0/state/statestore  app_id=order-processor instance=QTM-SWATHIKIL-1.redmond.corp.microsoft.com scope=dapr.runtime.http type=log ver=edge
== APP == INFO:root:Saving Order: {'orderId': '483'}
DEBU[0000] HTTP API Called: GET /v1.0/state/statestore/483  app_id=order-processor instance=QTM-SWATHIKIL-1.redmond.corp.microsoft.com scope=dapr.runtime.http type=log ver=edge
== APP == INFO:root:Getting Order: {'orderId': '483'}
DEBU[0000] HTTP API Called: DELETE /v1.0/state/statestore  app_id=order-processor instance=QTM-SWATHIKIL-1.redmond.corp.microsoft.com scope=dapr.runtime.http type=log ver=edge
== APP == INFO:root:Deleted Order: {'orderId': '483'}
DEBU[0000] HTTP API Called: PUT /v1.0/metadata/cliPID    app_id=order-processor instance=QTM-SWATHIKIL-1.redmond.corp.microsoft.com scope=dapr.runtime.http type=log ver=edge
```

##  Configuring API logging in Kubernetes

You can set the log level individually for every sidecar by providing the following annotation in your pod spec template:

```yml
annotations:
  dapr.io/api-log-level: "info"
```

or

```yml
annotations:
  dapr.io/api-log-level: "debug"
```

### Viewing API logs on Kubernetes

Dapr API logs are written to stdout and stderr and you can view API logs on Kubernetes.

See the kubernetes API logs by executing the below command.

```bash
kubectl logs <pod_name> daprd -n <name_space>
```

The example below show `info` level API logging in Kubernetes.

```bash
time="2022-03-16T18:32:02.487041454Z" level=info msg="HTTP API Called: GET /v1.0/invoke/invoke-receiver/method/my-method" app_id=invoke-caller instance=invokecaller-f4f949886-cbnmt scope=dapr.runtime.http type=log ver=edge
time="2022-03-16T18:32:02.698387866Z" level=info msg="HTTP API Called: GET /v1.0/invoke/invoke-receiver/method/my-method" app_id=invoke-caller instance=invokecaller-f4f949886-cbnmt scope=dapr.runtime.http type=log ver=edge
time="2022-03-16T18:32:02.917629403Z" level=info msg="HTTP API Called: GET /v1.0/invoke/invoke-receiver/method/my-method" app_id=invoke-caller instance=invokecaller-f4f949886-cbnmt scope=dapr.runtime.http type=log ver=edge
time="2022-03-16T18:32:03.137830112Z" level=info msg="HTTP API Called: GET /v1.0/invoke/invoke-receiver/method/my-method" app_id=invoke-caller instance=invokecaller-f4f949886-cbnmt scope=dapr.runtime.http type=log ver=edge
time="2022-03-16T18:32:03.359097916Z" level=info msg="HTTP API Called: GET /v1.0/invoke/invoke-receiver/method/my-method" app_id=invoke-caller instance=invokecaller-f4f949886-cbnmt scope=dapr.runtime.http type=log ver=edge
```

The below example is for debug level API logging in Kubernetes. The debug level API logs are visible only when the log-level is set to debug.

```bash
time="2022-03-18T21:03:00.03969986Z" level=debug msg="HTTP API Called: GET /v1.0/invoke/invoke-receiver/method/my-method" app_id=invoke-caller instance=invokecaller-56894979cd-rt87b scope=dapr.runtime.http type=log ver=edge
time="2022-03-18T21:03:00.271463876Z" level=debug msg="HTTP API Called: GET /v1.0/invoke/invoke-receiver/method/my-method" app_id=invoke-caller instance=invokecaller-56894979cd-rt87b scope=dapr.runtime.http type=log ver=edge
time="2022-03-18T21:03:00.492570204Z" level=debug msg="HTTP API Called: GET /v1.0/invoke/invoke-receiver/method/my-method" app_id=invoke-caller instance=invokecaller-56894979cd-rt87b scope=dapr.runtime.http type=log ver=edge
time="2022-03-18T21:03:00.705486991Z" level=debug msg="HTTP API Called: GET /v1.0/invoke/invoke-receiver/method/my-method" app_id=invoke-caller instance=invokecaller-56894979cd-rt87b scope=dapr.runtime.http type=log ver=edge
time="2022-03-18T21:03:00.916868445Z" level=debug msg="HTTP API Called: GET /v1.0/invoke/invoke-receiver/method/my-method" app_id=invoke-caller instance=invokecaller-56894979cd-rt87b scope=dapr.runtime.http type=log ver=edge
```