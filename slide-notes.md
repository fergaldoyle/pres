# Slide Notes: From Windows Servers to OpenShift

These notes support the accompanying Reveal.js slideshow in `index.html`. They are written for a broad audience that may include application teams, operations, security, release managers, testers, business stakeholders, and technical leadership.

## 1. Platform modernization

**Speaker intent:** Set the context that this is a modernization journey, not just a hosting swap.

**Talk track:**
- We are changing how applications are packaged, deployed, operated, and promoted to production.
- The move from Windows n-tier hosting to OpenShift changes both technology and team workflows.
- The goal is more reliable delivery, fewer environment surprises, and a stronger foundation for future application architecture.

## 2. What is changing?

**Speaker intent:** Give the audience a simple before-and-after model.

**Talk track:**
- In the old model, servers were the center of gravity. We installed software onto servers and deployed applications into that pre-existing state.
- In the new model, the application image becomes the deployable artifact. It includes the runtime and declared dependencies needed to run the app.
- OpenShift provides the standard platform where those containers run across environments.

**Emphasize:** This is a shift from server-centric operations to platform-centric and application-centric delivery.

## 3. Why Windows servers became difficult

**Speaker intent:** Explain the business and operational pain without blaming a specific technology or team.

**Talk track:**
- Long-lived servers accumulate state: installed packages, manual fixes, registry entries, certificates, scheduled tasks, and local files.
- Over time, servers drift from the documented baseline and from each other.
- Operating systems and middleware eventually go out of support, creating security, audit, and reliability risk.
- Rebuilding an environment can be slow because the server's exact state is not always captured in source control.

**Important nuance:** Windows itself is not the enemy. The issue is the fragile operating model around long-lived, manually maintained servers.

## 4. The dependency problem

**Speaker intent:** Make the hidden-dependency problem concrete.

**Talk track:**
- Many older applications require hard dependencies to be installed on each server before the application can run.
- Examples include runtime versions, framework versions, IIS modules, COM components, certificates, ODBC drivers, local directories, registry settings, or vendor middleware.
- If one server has a slightly different setup, the application may behave differently.
- Containers reduce this problem because the image declares and packages what the app needs.

**Emphasize:** Containers do not remove dependencies; they make them explicit, versioned, reviewable, and reproducible.

## 5. Target hosting model

**Speaker intent:** Show the simplified lifecycle from code to running workload.

**Talk track:**
- Developers commit application code and container build instructions.
- The pipeline builds a container image and tags it as a versioned artifact.
- Deployment manifests describe how that image should run on OpenShift.
- OpenShift handles scheduling, health checks, restarts, rollout behavior, networking integration, and platform controls.

**Business framing:** We get a more predictable path from source code to runtime.

## 6. Moving toward microservices

**Speaker intent:** Clarify that microservices are enabled by the platform, but not automatic or mandatory.

**Talk track:**
- Large applications often have tight coupling: one change can require a full release and broad regression testing.
- Microservices can help by splitting business capabilities into smaller deployable units.
- Smaller services can be owned, scaled, tested, and released more independently.
- However, microservices also require mature observability, API discipline, automated testing, resilience patterns, and ownership clarity.

**Emphasize:** Containerization is a foundation. Service decomposition should happen where it creates business or operational value.

## 7. CI/CD delivery flow

**Speaker intent:** Explain the role of GitLab, Jenkins, image registries, and Argo CD.

**Talk track:**
- GitLab is where code changes are proposed, reviewed, and merged through merge requests.
- Jenkins runs automated steps such as build, unit tests, quality checks, security scans, image creation, and publishing.
- The image registry stores the versioned container image so the same artifact can be promoted through environments.
- Argo CD watches Git-based desired state and synchronizes OpenShift to match that desired state.

**Emphasize:** We promote a known artifact through environments rather than rebuilding differently for each environment.

## 8. Promotion through environments

**Speaker intent:** Explain DEV, SIT, UAT, PREPROD, and PROD in terms of confidence building.

**Talk track:**
- DEV validates that the application can build and run early.
- SIT validates integration between services and systems.
- UAT validates user workflows and business acceptance.
- PREPROD validates production-like configuration, readiness, and release procedures.
- PROD is protected and changes are reviewed, approved, and traceable.

**Kustomization repo point:**
- For preprod and production, promotion is controlled through merge requests in the kustomization repository.
- Merging a change updates the desired state that Argo CD applies to the target OpenShift environment.

## 9. Team responsibilities

**Speaker intent:** Make ownership and collaboration explicit.

**Talk track:**
- Application teams own the application code, container definition, application configuration requirements, health endpoints, and runtime behavior.
- Ops/platform teams own the OpenShift platform: cluster health, capacity, upgrades, network integration, platform policies, and operational guardrails.
- Ops or release approvers manage sensitive operational changes, including protected environment promotions and secrets workflows.
- Security and governance teams define controls such as scanning, access policy, separation of duties, auditability, and compliance evidence.

**Emphasize:** This is shared ownership with clearer contracts, not a handoff of all responsibility to one team.

## 10. Secrets and configuration

**Speaker intent:** Address a common migration concern: environment differences and sensitive values.

**Talk track:**
- We should not rebuild the application separately for every environment just to change configuration.
- The same image should be configurable at runtime through environment-specific manifests, overlays, config maps, and secrets.
- Secrets are not placed in source code. They are managed through controlled OpenShift processes and access controls.
- The kustomization repo can represent environment-specific desired state while keeping sensitive values protected.

**Emphasize:** Configuration is code; secrets require stronger controls than ordinary configuration.

## 11. What success looks like

**Speaker intent:** Translate the migration into outcomes.

**Talk track:**
- Faster: teams can deliver smaller changes through automated pipelines.
- Safer: deployments are repeatable, reviewed, observable, and easier to roll back.
- Cleaner: fewer snowflake servers and fewer undocumented dependencies.
- More transparent: every change has a source, review, artifact, environment target, and audit trail.

**Possible metrics:** Deployment frequency, lead time, failed deployment rate, mean time to recovery, support ticket volume, vulnerability remediation time.

## 12. Migration roadmap

**Speaker intent:** Give a practical path forward.

**Talk track:**
- Start by assessing each application: dependencies, runtime, data flows, network needs, certificates, scheduled jobs, support status, and operational procedures.
- Containerize the app and externalize configuration.
- Build a repeatable pipeline that tests, scans, packages, and publishes the image.
- Define OpenShift deployment manifests and kustomization overlays for each environment.
- Use Argo CD to sync desired state and observe deployment health.
- Optimize after migration: scaling, resilience, observability, cost, and possible service decomposition.

**Emphasize:** Discovery is often the hardest part because older servers may contain undocumented application knowledge.

## 13. Key takeaways

**Speaker intent:** Close with the message the audience should remember.

**Talk track:**
- We are moving away from dependency-heavy, manually maintained Windows servers.
- We are moving toward container images, automated pipelines, OpenShift runtime, and GitOps-based deployment.
- The operating model relies on collaboration between application teams, Ops/platform teams, security, and release approvers.
- This foundation supports safer modernization and selective movement toward microservices.

## Suggested Q&A prompts

Use these questions if discussion slows:
- Which current applications have the most hidden server dependencies?
- What deployment steps are still manual today?
- Where do environment differences cause the most defects?
- Which services would benefit most from independent scaling or independent releases?
- What approvals are required for preprod and production, and are they represented clearly in GitLab merge requests?

## Optional presenter reminders

- Avoid positioning OpenShift as magic. It still requires good engineering practices.
- Avoid promising that every application becomes a microservice immediately.
- Reinforce that the same artifact should move through environments.
- Reinforce that protected environment changes are controlled through merge requests and approvals.
- Connect technical improvements to business outcomes: speed, safety, auditability, and reliability.
