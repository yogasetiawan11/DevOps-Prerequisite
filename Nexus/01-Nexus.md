# What is Nexus Repository?

Nexus Repository is an artifact repository manager: a central system to store, proxy, manage, and distribute build artifacts (packages) used in your software delivery pipeline — e.g., Maven/JARs, npm packages, Docker images, PyPI, NuGet, Helm charts, and more. It provides hosted repositories (your published artifacts), proxy repositories (cached access to remote registries), and group repositories (logical collections).

The difference between Nexus and Git (GitHub, GitLab):
- Git Used to store and manage source code. It tracks changes to text-based files (code, configuration, documentation) using version control.

- Nexus Repository Manager: Used to store built artifacts or binaries that are produced after the source code is compiled. for example .jar, .war, .zip, .tar.gz, or Docker images.

# Why use Nexus? (benefits & use cases)

Reliable dependency resolution — caches external dependencies so builds remain stable even if upstream registries are slow or down. This reduces build flakiness and external network reliance. 

Speed & bandwidth savings — cached artifacts speed up CI and developer builds and lower outbound network usage. 

Security & governance — control which artifacts enter your org, apply access controls and scanning (when integrated with security tools), and enforce release processes.

Promotion & release control — staging and promotion workflows let you validate artifacts before they become “release” artifacts. This supports clean release pipelines. 

Multi-format support — one platform to manage many package types (Maven, npm, Docker, etc.), which simplifies toolchain integration. 

# How Nexus works — core concepts

- Repository types
- Hosted — you push your internal or released artifacts here.

- Proxy (remote) — Nexus caches artifacts from external registries (e.g., Maven Central, npmjs.org).

- Group — a virtual repository that aggregates several hosted/proxy repos into a single URL for clients. 

- Blob stores & storage
Artifacts are stored as binary blobs. Nexus supports filesystem blobstores and object-store blobstores (S3, etc.). Use file system for performance, object stores for scalable/cloud storage. Plan capacity and quotas. 

- Staging & promotion
Create isolated staging repositories, validate artifacts (integration tests, signing, scanning), then promote to a release repository or discard. This is a controlled way to move artifacts through lifecycle stages. 


- Access control
Users, roles, and privileges control who can read/publish/delete. Integrations with LDAP/SSO (Pro features may add richer enterprise auth).