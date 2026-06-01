# Data Model: Document Upload and Management

## Document
Represents a single uploaded document and its metadata.

- `DocumentId` (int, PK)
- `Title` (string, required, max 255)
- `Description` (string?, max 2000)
- `Category` (string, required, max 100)
- `Tags` (string?, comma-delimited, max 500)
- `ProjectId` (int?, FK to Project)
- `UploadedByUserId` (int, FK to User)
- `UploadedDate` (DateTime, required)
- `FileName` (string, required, max 200)
- `FilePath` (string, required, max 1000)
- `ContentType` (string, required, max 255)
- `FileSize` (long, required)
- `IsDeleted` (bool, default false)

### Relationships
- One `Document` is uploaded by one `User`.
- One `Document` may belong to one `Project`.
- One `Document` can have many `DocumentShare` records.
- One `Document` can have many `DocumentActivity` records.

## DocumentShare
Tracks documents shared with other users or teams.

- `DocumentShareId` (int, PK)
- `DocumentId` (int, FK to Document)
- `SharedWithUserId` (int, FK to User)
- `SharedByUserId` (int, FK to User)
- `SharedDate` (DateTime, required)
- `Message` (string?, max 1000)

### Relationships
- One `DocumentShare` belongs to one `Document`.
- One `DocumentShare` identifies one recipient user.
- One `DocumentShare` identifies one sharing user.

## DocumentActivity
Logs document-related actions for audit and reporting.

- `DocumentActivityId` (int, PK)
- `DocumentId` (int, FK to Document)
- `UserId` (int, FK to User)
- `Action` (string, required, e.g., Upload, Download, Delete, Share)
- `ActionDate` (DateTime, required)
- `Details` (string?, max 1000)

### Relationships
- One `DocumentActivity` belongs to one `Document`.
- One `DocumentActivity` is created by one `User`.

## Data Design Notes
- `Category` stores text values to match the existing training simplicity requirement.
- `Tags` are stored as a comma-delimited string for MVP search support.
- Files are stored outside `wwwroot` and referenced by a secure `FilePath`.
- `IsDeleted` allows soft-delete logic if needed, while physical file deletion is handled by the storage layer.
