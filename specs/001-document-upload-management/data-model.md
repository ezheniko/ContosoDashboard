# Data Model: Document Upload and Management

## Entities

### Document

Represents a stored document and its metadata.

- DocumentId: int (primary key)
- Title: string
- Description: string?
- Category: string
- FileName: string
- FilePath: string
- FileType: string
- FileSize: long
- UploadedAt: DateTime
- UploadedByUserId: int
- ProjectId: int?
- TaskId: int?
- IsDeleted: bool

**Relationships**
- Belongs to one uploader (`User`)
- May belong to one project (`Project`)
- May belong to one task (`TaskItem`)

### DocumentShare

Represents a direct sharing relationship for a document.

- DocumentShareId: int
- DocumentId: int
- UserId: int
- SharedAt: DateTime
- SharedByUserId: int

**Relationships**
- Belongs to one document
- Belongs to one recipient user

### DocumentActivity

Represents an audit entry for document-related actions.

- DocumentActivityId: int
- DocumentId: int?
- UserId: int
- ActivityType: string
- ActivityDate: DateTime
- Details: string?

**Relationships**
- May refer to one document
- Belongs to one acting user

## Validation Rules

- Title is required.
- Category is required and must come from the allowed values defined in the spec.
- File size must be within the configured limit of 25 MB.
- File extension must be in an allowed whitelist.
- File path must be generated before database persistence.
- Only authorized users can view or update a document.

## Notes

The model keeps the implementation simple and aligned with the existing integer-key pattern used by the current application.
