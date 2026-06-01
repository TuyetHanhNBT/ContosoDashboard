# Document Management Contracts

## UI Contract: Document Management

### Routes
- `/documents` — My Documents page
- `/documents/shared` — Shared with Me page
- `/projects/{projectId}` — Project Details page with Project Documents section
- `/documents/{documentId}` — Document Details page

### Upload Form
- Fields:
  - `Title` (required)
  - `Description` (optional)
  - `Category` (required; values: Project Documents, Team Resources, Personal Files, Reports, Presentations, Other)
  - `Project` (optional; dropdown of user-accessible projects)
  - `Tags` (optional; comma-separated string)
  - `Files` (one or more uploads)
- Validation rules:
  - supported types: PDF, DOC/DOCX, XLS/XLSX, PPT/PPTX, TXT, JPEG, PNG
  - max file size 25 MB
  - required `Title` and `Category`

### List/Filter/Sort Behavior
- Columns: `Title`, `Category`, `UploadedDate`, `FileSize`, `Project`
- Filters: category, project, date range
- Sort options: title, upload date, category, file size
- Search fields: title, description, tags, uploader name, project

### Preview/Download
- Preview supported for PDF and image files in-browser
- Download available for all authorized files
- Access control enforced on both preview and download

### Sharing
- Owners can select recipients by user or team
- Shared documents appear in recipients' `Shared with Me` view
- Sharing actions generate an in-app notification

## Service Contract: DocumentService

### Public methods
- `Task<Document> UploadDocumentAsync(DocumentUploadRequest request, int userId)`
- `Task<List<Document>> GetUserDocumentsAsync(int userId, DocumentQueryParameters query)`
- `Task<List<Document>> GetProjectDocumentsAsync(int projectId, int userId)`
- `Task<Document?> GetDocumentByIdAsync(int documentId, int userId)`
- `Task<bool> UpdateDocumentMetadataAsync(DocumentMetadataUpdate request, int userId)`
- `Task<bool> ReplaceDocumentFileAsync(int documentId, DocumentFileUpdate request, int userId)`
- `Task<bool> DeleteDocumentAsync(int documentId, int userId)`
- `Task<bool> ShareDocumentAsync(int documentId, IEnumerable<int> recipientUserIds, int userId)`
- `Task<List<Document>> SearchDocumentsAsync(int userId, string query)`
- `Task<List<DocumentShare>> GetSharedDocumentsAsync(int userId)`

## Storage Contract: IFileStorageService

### Public methods
- `Task<string> UploadAsync(Stream fileStream, string fileName, string contentType, string destinationPath)`
- `Task DeleteAsync(string filePath)`
- `Task<Stream> DownloadAsync(string filePath)`
- `Task<bool> ExistsAsync(string filePath)`

## Download/Preview Endpoints
- `GET /api/documents/{id}/download` — downloads authorized document
- `GET /api/documents/{id}/preview` — returns PDF/image preview stream for authorized document
- `POST /api/documents/{id}/share` — shares a document with recipients
- `POST /api/documents/search` — returns authorized search results

## Data Contract: Document Upload Payload

```json
{
  "title": "Project charter",
  "description": "Initial draft of the charter",
  "category": "Project Documents",
  "projectId": 1,
  "tags": "charter,planning,project",
  "files": [
    { "fileName": "charter.pdf", "contentType": "application/pdf", "size": 125000 }
  ]
}
```

## Authorization Contract
- Document owners may edit metadata, replace files, and delete documents.
- Project Managers may delete any document in their projects.
- Project members may view and download project documents.
- Shared recipients may view and download shared documents.
- Administrators have full access for auditing.
