# Tasks: Document Upload and Management

**Input**: Design documents from `/specs/001-document-upload-and-management/`
**Prerequisites**: `plan.md`, `spec.md`, `research.md`, `data-model.md`, `contracts/`

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Establish the storage abstraction and feature scaffolding.

- [ ] T001 Create feature documentation and task artifacts in `specs/001-document-upload-and-management/`
- [ ] T002 Add `ContosoDashboard/Services/IFileStorageService.cs` to define upload/download/delete storage behavior
- [ ] T003 Add `ContosoDashboard/Services/LocalFileStorageService.cs` to implement local filesystem storage outside `wwwroot`
- [ ] T004 Update `ContosoDashboard/Program.cs` to register `IFileStorageService` and configure secure upload path settings

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Add core document entities, service contracts, database wiring, and shared business logic.

- [ ] T005 Create entity models in `ContosoDashboard/Models/Document.cs`, `ContosoDashboard/Models/DocumentShare.cs`, and `ContosoDashboard/Models/DocumentActivity.cs`
- [ ] T006 Update `ContosoDashboard/Data/ApplicationDbContext.cs` to add `DbSet<Document>`, `DbSet<DocumentShare>`, and `DbSet<DocumentActivity>` and configure file metadata mappings
- [ ] T007 Add DTO models in `ContosoDashboard/Models/DocumentUploadRequest.cs`, `ContosoDashboard/Models/DocumentMetadataUpdate.cs`, `ContosoDashboard/Models/DocumentFileUpdate.cs`, and `ContosoDashboard/Models/DocumentQueryParameters.cs`
- [ ] T008 Implement `ContosoDashboard/Services/DocumentService.cs` with upload, list, metadata update, replace, delete, search, preview, project document access, and share methods
- [ ] T009 Update `ContosoDashboard/Program.cs` to register `IDocumentService` and ensure document services are available to pages

---

## Phase 3: User Story 1 - Upload and manage personal or project documents (Priority: P1) 🎯 MVP

**Goal**: Let users upload documents with metadata, view their own uploads, edit metadata, and delete documents.

**Independent Test**: Upload a supported file, verify it appears in the user document list, edit the title or category, and delete it.

- [ ] T010 [P] [US1] Create `ContosoDashboard/Pages/Documents.razor` for document upload and personal document listing
- [ ] T011 [US1] Implement the upload form, metadata entry, and validation inside `ContosoDashboard/Pages/Documents.razor`
- [ ] T012 [US1] Implement document list display, sorting, and filtering by metadata in `ContosoDashboard/Pages/Documents.razor`
- [ ] T013 [US1] Add delete and metadata edit actions for owned documents in `ContosoDashboard/Pages/Documents.razor`
- [ ] T014 [US1] Add upload handling and metadata creation in `ContosoDashboard/Services/DocumentService.cs`
- [ ] T015 [US1] Add training-safe mock scan and file-type/size validation in `ContosoDashboard/Services/LocalFileStorageService.cs`

---

## Phase 4: User Story 2 - Browse, preview, and access project documents (Priority: P2)

**Goal**: Let project team members see and preview project documents with authorized access.

**Independent Test**: View a project page, open the project documents section, preview a PDF or image, and download an authorized file.

- [ ] T016 [US2] Implement `GetProjectDocumentsAsync` and authorized preview/download methods in `ContosoDashboard/Services/DocumentService.cs`
- [ ] T017 [US2] Update `ContosoDashboard/Pages/ProjectDetails.razor` to show project documents with filtering and sorting controls
- [ ] T018 [US2] Add browser preview support for PDF and image documents in `ContosoDashboard/Pages/ProjectDetails.razor`
- [ ] T019 [US2] Add project document search support to `ContosoDashboard/Pages/Documents.razor` and ensure results are permission-limited
- [ ] T020 [US2] Add authorization enforcement for project document access in `ContosoDashboard/Services/DocumentService.cs`

---

## Phase 5: User Story 3 - Share documents and receive notifications (Priority: P3)

**Goal**: Let users share documents with teammates, show shared documents to recipients, and generate notifications.

**Independent Test**: Share a document, verify the recipient sees it in a shared documents view, and confirm a notification is created.

- [ ] T021 [US3] Implement `ShareDocumentAsync` in `ContosoDashboard/Services/DocumentService.cs`
- [ ] T022 [US3] Create `ContosoDashboard/Pages/SharedDocuments.razor` to list documents shared with the logged-in user
- [ ] T023 [US3] Add sharing UI and recipient selection to `ContosoDashboard/Pages/Documents.razor`
- [ ] T024 [US3] Generate in-app notifications for shared documents in `ContosoDashboard/Services/DocumentService.cs`
- [ ] T025 [US3] Add authorization checks so only recipients and authorized project members can open shared documents in `ContosoDashboard/Services/DocumentService.cs`

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Clean up security, documentation, and performance across the new document feature.

- [ ] T026 [P] Update `specs/001-document-upload-and-management/quickstart.md` with the final validation steps and run instructions
- [ ] T027 [P] Review and refactor `ContosoDashboard/Services/DocumentService.cs` and `ContosoDashboard/Services/LocalFileStorageService.cs` for clarity and security
- [ ] T028 [P] Validate `ContosoDashboard/Data/ApplicationDbContext.cs` mappings and `ContosoDashboard/Models/` entity constraints for correct database behavior
- [ ] T029 [P] Confirm document preview and search performance on `ContosoDashboard/Pages/Documents.razor` and `ContosoDashboard/Pages/ProjectDetails.razor`

---

## Dependencies & Execution Order

- **Phase 1: Setup** must be completed before **Phase 2: Foundational**.
- **Phase 2: Foundational** blocks all user stories and must be complete before any story-specific implementation begins.
- **Phase 3: User Story 1** is the MVP slice and should be delivered first.
- **Phase 4: User Story 2** builds on foundational services and adds project-level browsing and preview.
- **Phase 5: User Story 3** adds sharing and notification functionality after document storage and access are implemented.
- **Phase 6: Polish** can run after the main stories are implemented, with many tasks parallelizable.

### Story Completion Order

1. User Story 1 (P1) — MVP upload, list, edit, delete
2. User Story 2 (P2) — project browse, preview, download
3. User Story 3 (P3) — share, shared docs, notifications

## Parallel Opportunities

- Phase 1 tasks `T002` and `T003` can be worked in parallel because they both target storage abstraction files.
- Phase 2 tasks `T005` and `T007` can run in parallel while `T006` waits for entity creation.
- In User Story 1, page tasks in `ContosoDashboard/Pages/Documents.razor` can be developed alongside service methods in `ContosoDashboard/Services/DocumentService.cs` once DTOs exist.
- In User Story 2, project page UI updates and service access methods can be implemented in parallel after foundational storage is ready.
- In User Story 3, the shared documents page and sharing workflow logic can progress in parallel with notification integration.

## Implementation Strategy

1. Complete **Phase 1** and **Phase 2** to establish storage, models, and service contracts.
2. Deliver **User Story 1** as the MVP and validate upload/list/edit/delete flows.
3. Add **User Story 2** to support project document browsing and preview once core storage is stable.
4. Add **User Story 3** for sharing and notifications as the final feature increment.
5. Finish with **Phase 6** polish, including documentation, security review, and performance validation.
