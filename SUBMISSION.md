<div align="center">

# PA4 Submission: TaskFlow Pipeline

<img alt="GitHub only" src="https://img.shields.io/badge/Submit-GitHub%20URL%20Only-10b981?style=for-the-badge">
<img alt="Total points" src="https://img.shields.io/badge/Total-100%20points-7c3aed?style=for-the-badge">

</div>

<div style="background:#f5f3ff;color:#111827;border-left:6px solid #6330bc;padding:14px 18px;border-radius:10px;margin:18px 0;">
Copy this file to <code style="color:#111827;background:#ddd6fe;padding:2px 4px;border-radius:4px;">SUBMISSION.md</code>. Put every screenshot in <code style="color:#111827;background:#ddd6fe;padding:2px 4px;border-radius:4px;">docs/</code>, embed it under the correct task, and write a short description below each image explaining what it proves. The grader should not need any file outside this repository.
</div>

## Student Information

| Field | Value |
|---|---|
| Name | `Muneeb ur Rehman` |
| Roll Number | `26100346` |
| GitHub Repository URL | https://github.com/muneeb0346/CS487-PA4 |
| Resource Group | `rg-sp26-26100346` |
| Assigned Region | `swedencentral` |

## Evidence Rules

- Use relative image paths, for example: `![AKS nodes](docs/aks-nodes.png)`.
- Every image must have a 1-3 sentence description below it.
- Azure Portal screenshots must show the resource name and enough page context to identify the service.
- CLI screenshots must show the command and output.
- Mask secrets such as function keys, ACR passwords, and storage connection strings.

## Task 1: App Service Web App (15 points)

### Evidence 1.1: Forked Repository

<img alt="Forked Repository" src="./docs/screenshots/Task 1 forked GitHub repository.png">

Description: This is my working GitHub fork of the CS487-PA4 starter repository containing the required project structure.

### Evidence 1.2: App Service Overview

<img alt="App Service overview" src="./docs/screenshots/Task 1 Web App overview showing Running status.png">

Description: The Web App is running in the rg-sp26-26100346 resource group in Sweden Central using the Node 22 LTS runtime, accessible at pa4-26100346.azurewebsites.net.

### Evidence 1.3: Deployment Center / GitHub Actions

<img alt="Deployment Center showing GitHub Actions workflow" src="./docs/screenshots/Task 1 Deployment Center showing GitHub configuration.png">

Description: The Web App's Deployment Center is configured to use GitHub Actions for CI/CD, automatically pulling from the main branch of my muneeb0346/CS487-PA4 fork.

### Evidence 1.4: Live Web UI

<img alt="Live Web UI" src="./docs/screenshots/Task 1 dashboard loading in a browser.png">

Description: The TaskFlow frontend is successfully deployed and accessible via the App Service public URL.

<img alt="Web App application settings" src="./docs/screenshots/Task 1 Application Settings configured.png">

Description: The application settings FUNCTION_START_URL and FUNCTION_STATUS_URL are temporarily set to "PENDING" until the backend Function App is deployed.

---

## Task 2: Azure Container Registry (15 points)

### Evidence 2.1: ACR Overview

<img alt="ACR overview" src="./docs/screenshots/Task 2 ACR overview in the portal showing Succeeded.png">

Description: The pa426100346 Azure Container Registry was successfully provisioned using the Basic SKU in the rg-sp26-26100346 resource group.

### Evidence 2.2: Docker Builds

<img alt="Docker build output" src="./docs/screenshots/Task 2 successful Docker builds for all three images.png">

Description: The validate-api, report-job, and func-app Docker images were successfully built locally from their respective project folders.

<img alt="Local Test of validate-api" src="./docs/screenshots/Task 2 local test of the validator (POST validate returning JSON).png">

Description: The validate-api container was tested locally via a POST request to confirm it successfully validates order payloads before pushing to Azure.

### Evidence 2.3: ACR Repositories

<img alt="ACR repositories" src="./docs/screenshots/Task 2 successful pushes to ACR for all three images.png">

Description: The validate-api:v1, report-job:v1, and func-app:v1 images were successfully tagged and pushed to the remote Azure Container Registry.

<img alt="ACR repository list" src="./docs/screenshots/Task 2 ACR repository list showing validate-api, report-job, and func-app.png">

Description: The Azure Portal repository list confirms that all three backend microservice images are now securely stored in the container registry.

---

## Task 3: Durable Function Implementation (12 points)

### Evidence 3.1: Completed Function Code

[function_app.py](function-app/function_app.py)

Description: TODO: The orchestrator receives an order payload, awaits the result of `validate_activity`, and conditionally triggers `report_activity` only if the validation is successful.

### Evidence 3.2: Local Function Handler Listing

<img alt="Local function handler listing" src="./docs/screenshots/Task 3 func start showing all four Durable handlers registered (starter, orchestrator, validate_activity, report_activity).png">

Description: Running `func start` locally confirms that the Durable Functions runtime successfully discovered and registered all four required function handlers.

---

## Task 4: Function App Container Deployment (8 points)

### Evidence 4.1: Function App Container Configuration

<img alt="Function App Deployment Center" src="./docs/screenshots/Task 4 Function App in Azure Portal showing the container image configuration.png">

Description: The Function App `pa4-26100346-func` is successfully configured to pull the `func-app:v1` container image from the `pa426100346` Azure Container Registry.

<img alt="Functions List" src="./docs/screenshots/Task 4 Functions list in the Portal showing http_starter, my_orchestrator, validate_activity, report_activity.png">

Description: The Azure Portal confirms that the container deployed successfully and registered the four required Durable Function handlers.

### Evidence 4.2: Orchestration Smoke Test

<img alt="Curl output" src="./docs/screenshots/Task 4 curl output showing the orchestration started (instance id and statusQueryGetUri visible).png">

Description: The returned `id` and `statusQueryGetUri` prove that the HTTP starter successfully received the payload and triggered the Durable Orchestrator.

### Evidence 4.3: Expected Failed Status Before Downstream Wiring

<img alt="Status query JSON" src="./docs/screenshots/Task 4 status query URL JSON showing Failed status at this stage.png">

Description: The `Failed` status is fully expected at this stage because the orchestrator attempts to trigger `validate_activity`, but the `VALIDATE_URL` app setting has not been configured to point to the AKS cluster yet.

---

## Task 5: AKS Validator (15 points)

### Evidence 5.1: AKS Cluster

<img alt="AKS Cluster Overview" src="./docs/screenshots/Task 5 AKS overview showing aks-26100346 succeeded.png">

Description: The cluster `aks-26100346` is deployed in the UK West region with a 1-node pool in the `rg-sp26-26100346` resource group.

### Evidence 5.2: Kubernetes Nodes and Pods

<img alt="kubectl get nodes" src="./docs/screenshots/Task 5 kubectl get nodes showing your cluster is running.png">
<img alt="kubectl get pods" src="./docs/screenshots/Task 5 kubectl get pods showing the validator pod is Running.png">

Description: The `kubectl` output confirms the AKS node is ready and the `validate-api` pod is successfully scheduled and running without errors.

### Evidence 5.3: Kubernetes Service

<img alt="kubectl get services" src="./docs/screenshots/Task 5 kubectl get service showing the assigned EXTERNAL-IP.png">

Description: The `validate-service` LoadBalancer successfully exposed the pod on the external IP `20.58.117.221` at port `8080`.

### Evidence 5.4: Validator API Tests

<img alt="Validator curl tests" src="./docs/screenshots/Task 5 curl health and curl validate both run.png">

Description: The API responds successfully to `/health` checks, accepts valid JSON payloads, and correctly implements the rejection rule for any order where `qty > 100`.

### Evidence 5.5: Function App `VALIDATE_URL`

<img alt="Function App Environment Variables" src="./docs/screenshots/Task 5 Function App Application Setting VALIDATE_URL.png">

Description: The Durable Function orchestrator dynamically reaches the AKS validator using the `VALIDATE_URL` environment variable configured with the LoadBalancer's IP.

### Evidence 5.6: AKS Idle Behavior

<img alt="AKS Idle Metrics" src="./docs/screenshots/Task 5 AKS idle metrics.png">

Description: As shown by the metrics, the AKS node remains running and incurs continuous compute costs even when there are no active validation orders being processed.

---

## Task 6: ACI Report Job (15 points)

### Evidence 6.1: Blob Container

TODO: Embed screenshot of the `reports` blob container.

Description: TODO: Explain where generated PDFs are stored.

### Evidence 6.2: Manual ACI Run

TODO: Embed screenshot of `az container show` for `ci-report-test`.

Description: TODO: State the final container state and why the job exits.

### Evidence 6.3: ACI Logs

TODO: Embed screenshot of `az container logs`.

Description: TODO: Explain what the report job printed after generating and uploading the PDF.

### Evidence 6.4: Generated PDF

TODO: Embed screenshot showing `TEST-001.pdf` in Blob Storage or opened from Blob Storage.

Description: TODO: Explain how this proves the ACI wrote to storage.

### Evidence 6.5: Function App Managed Identity and IAM

TODO: Embed screenshots of system-assigned identity enabled and Contributor role assignment on your resource group.

Description: TODO: Explain why the Function App needs this permission to create ACIs.

### Evidence 6.6: Report App Settings

TODO: Embed screenshot of `REPORT_*`, `ACR_*`, `STORAGE_CONN`, and `SUBSCRIPTION_ID` settings.

Description: TODO: Explain what each group of settings is used for. Mask secrets.

---

## Task 7: End-to-End Pipeline (15 points)

### Evidence 7.1: Web App Wiring

TODO: Embed screenshot showing `FUNCTION_START_URL` and `FUNCTION_STATUS_URL` configured on the Web App.

Description: TODO: Explain how the frontend starts and polls the Durable orchestration.

### Evidence 7.2: Happy Path UI

TODO: Embed screenshots of the form before submit, Running status, and Completed status with report URL.

Description: TODO: Explain the valid order payload and final result.

### Evidence 7.3: Backend Participation

TODO: Embed screenshots showing Function App invocation, AKS validator evidence, ACI evidence, and Blob PDF evidence.

Description: TODO: Trace the same order ID across services.

### Evidence 7.4: Reject Path UI

TODO: Embed screenshot of an order with `qty > 100` being rejected.

Description: TODO: Explain why no report ACI should be created for this order.

---

## Task 8: Write-up and Architecture Diagram (5 points)

### Evidence 8.1: Architecture Diagram

TODO: Embed your architecture diagram from `docs/`.

Description: TODO: Confirm that it shows GitHub, App Service, Durable Function, AKS, ACI, Blob Storage, ACR, and IAM.

### Question 8.2: Service Selection

TODO: In 3-4 sentences each, explain why TaskFlow uses App Service, Durable Functions, AKS, and ACI for their specific roles.

### Question 8.3: ACI vs AKS

TODO: Compare idle behavior, cost behavior, and operational model for AKS and ACI using your screenshots.

### Question 8.4: Durable Functions vs Plain HTTP

TODO: Explain at least two problems that Durable Functions solves for this sequential workflow.

### Question 8.5: Cost Review

TODO: Embed Cost Management screenshot scoped to your resource group.

Description: TODO: Identify the most expensive resource and explain why.

### Question 8.6: Challenges Faced

TODO: Describe at least two real issues you hit and how you debugged them.

---
