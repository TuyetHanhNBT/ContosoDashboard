# Research: Document Upload and Management

## Decision
The document management feature will be implemented as a secure, offline-capable extension to the existing Blazor Server app using local filesystem storage behind an abstraction layer. The upload workflow will use a training-safe mock scan for file-type and size validation, with no external malware scanning service required for this repository.

## Rationale
- The current repository is a training-focused Blazor Server application with mock authentication and local database storage.
- The feature requirements explicitly demand offline storage, no external cloud dependency, and local file storage outside `wwwroot` for security.
- A local `IFileStorageService` abstraction preserves the migration path to Azure Blob Storage while keeping the training implementation simple.
- Simulating file scanning through validation avoids introducing unsupported antivirus dependencies and matches the training-first constitution.

## Alternatives considered
- Real antivirus integration: rejected because it would create an external dependency that is not compatible with offline training and the existing repository structure.
- Storing uploaded documents in `wwwroot`: rejected because it violates the security requirement for authorization-guarded downloads and increases attack surface.
- Using a separate file storage project or service: rejected because the repository and feature scope are intentionally kept within the existing Blazor Server application.

## Outcome
The implementation approach is to add a document feature module in the existing `ContosoDashboard` project and support file upload, metadata, project association, search, preview, sharing, and audit logging using EF Core and local filesystem storage.
