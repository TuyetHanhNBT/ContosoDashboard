# Quickstart: Document Upload and Management

## Run the application

1. Open a terminal in `d:\AI-learn\claude code\TrainingProjects\ContosoDashboard\ContosoDashboard`
2. Start the app:

```powershell
cd ContosoDashboard
dotnet run
```

3. Open the browser at `https://localhost:5001` or `http://localhost:5000`
4. Login using one of the seeded training users (e.g. `admin@contoso.com`, `camille.nicole@contoso.com`, `floris.kregel@contoso.com`, `ni.kang@contoso.com`)

## Exercise the feature

1. Navigate to the document management page at `/documents`.
2. Upload one or more supported files with required metadata:
   - `Title`
   - `Category`
   - optional `Description`, `Project`, and `Tags`
3. Verify the uploaded documents appear in your document list.
4. Use filters and sorting to find documents by category, project, date, or size.
5. Open a PDF or image preview where supported.
6. Download documents you are authorized to access.
7. Edit metadata for a document you own and confirm the update.
8. Delete a document you uploaded and verify it is removed from both the UI and storage.
9. Share a document with a teammate and verify the recipient receives an in-app notification.

## Expected storage behavior

- Files are stored outside `wwwroot` in a secure upload directory.
- Upload path pattern: `{userId}/{projectId or "personal"}/{guid}.{extension}`
- File metadata is stored in the database, and downloads are authorized through the application.

## Validation checks

- Upload only supported MIME types.
- Reject files larger than 25 MB.
- Reject unsupported file types with a clear error message.
- Ensure preview works for PDF and image files.
- Ensure shared documents appear in the recipient's shared documents list.
- Ensure search only returns documents the current user can access.
