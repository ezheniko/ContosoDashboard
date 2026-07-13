# Document Service Contract

## Purpose

Defines the core service operations for the document feature in the MVP.

## Operations

### UploadDocumentAsync

- Input: document metadata, file stream, current user ID
- Behavior: validates metadata and file, stores the file via the configured storage service, persists document metadata, and returns a result object
- Output: success or validation error

### GetDocumentsAsync

- Input: current user ID, optional project filter, optional category filter
- Behavior: returns the list of documents the current user is authorized to view

### SearchDocumentsAsync

- Input: current user ID, search text
- Behavior: returns matching documents based on title, description, tags, uploader name, or project name

### UpdateDocumentMetadataAsync

- Input: document ID, updated metadata, current user ID
- Behavior: updates metadata if the user is allowed to edit the document

### DeleteDocumentAsync

- Input: document ID, current user ID
- Behavior: removes the document from storage and deletes the metadata record if the user is authorized

## Notes

The contract is intentionally service-oriented so the UI can remain thin and the business rules stay centralized.
