# Implementation Plan: Document Upload and Management

**Branch**: `001-document-upload-management` | **Date**: 2026-07-13 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from [spec.md](spec.md)

## Summary

Implement a document management feature for ContosoDashboard that supports offline document upload, metadata capture, authorization-based browsing, search, preview for PDFs and images, and basic document management within the existing ASP.NET Core 8 / Blazor Server / EF Core / SQLite architecture. The MVP will focus on personal and project document access, with sharing deferred to a later phase.

## Technical Context

**Language/Version**: C# / .NET 8  
**Primary Dependencies**: ASP.NET Core, Blazor Server, Entity Framework Core, SQLite  
**Storage**: Local filesystem plus EF Core relational data  
**Testing**: Manual validation flows and targeted .NET build verification  
**Target Platform**: Linux development environment, web application  
**Project Type**: Web application  
**Performance Goals**: Uploads up to 25 MB complete within 30 seconds; document lists and search respond within 2 seconds for typical local datasets  
**Constraints**: Must remain offline-capable, use local storage for training, preserve current authentication model, and avoid introducing major architectural rewrites  
**Scale/Scope**: Single web app with user, project, and document entities plus a small document access model

## Constitution Check

- Training-First and Offline-Capable: The feature will use local filesystem storage behind an abstraction and remain functional without any external services.
- Security-by-Design: Document access will be enforced in the service layer and UI, with per-user authorization and project-based visibility rules.
- Test-First and Evidence-Based Change: Manual validation scenarios will be defined before implementation, and the feature will be verified by running the app and exercising the relevant flows.
- Documentation and Traceability: README or stakeholder docs will be updated if user-visible behavior changes.

## Project Structure

```text
ContosoDashboard/
├── Data/
├── Models/
├── Pages/
├── Services/
└── wwwroot/
```

**Structure Decision**: Add the feature directly within the existing ASP.NET Core Blazor Server structure by introducing document models, a document service, storage abstraction, and a documents page plus supporting navigation and dashboard integration.

## Implementation Approach

### 1. Data model and persistence
- Add a `Document` entity with fields for title, description, category, project association, file path, MIME type, size, upload date, uploader, and visibility state.
- Add a `DocumentShare` entity for the post-MVP sharing flow, but keep it inert in the MVP unless needed for future compatibility.
- Add `DocumentActivity` support for audit logging.
- Extend `ApplicationDbContext` with new DbSets and relationships to `User`, `Project`, and `TaskItem`.

### 2. Storage abstraction
- Introduce `IFileStorageService` with `UploadAsync`, `DeleteAsync`, `DownloadAsync`, and `GetUrlAsync` methods.
- Implement `LocalFileStorageService` in the training environment, storing files under a dedicated local directory such as `AppData/uploads`.
- Keep the business logic independent from the concrete storage implementation so future Azure migration remains straightforward.
- Add an asynchronous background processing path for file scanning: after upload, the app writes a queue message describing the uploaded document and a worker function processes the scan.

### 3. Business service layer
- Add `IDocumentService` and `DocumentService` to manage validation, authorization, upload, metadata updates, deletion, browsing, and search.
- Enforce authorization rules in the service layer so unauthorized access is blocked even if a page route is manipulated.
- Reuse the existing `IProjectService` and current user claim handling patterns from the app.
- Publish a scan request after successful upload so an Azure Function worker can process the file asynchronously.

### 4. UI and user experience
- Add a new Documents page for personal and project document views.
- Add a simple upload modal or form with metadata fields and file selection.
- Add a document preview experience for PDF and image files where supported.
- Add navigation from the main menu and dashboard summary integration for recent documents.

### 5. Validation and verification
- Verify uploads, validation errors, browsing, search, and authorization restrictions through manual runs in the app.
- Build the application with `dotnet build` after implementation and fix any issues before completion.

## Phases

### Phase 0: Foundation
- Add document models and database context support
- Add storage abstraction and local implementation
- Register the storage and document services in DI
- Define the queue message contract for async virus-scan processing

### Phase 1: Core document workflows
- Implement upload validation and file persistence
- Implement document list, search, and filtering
- Implement authorization checks for personal and project documents
- Add the background queue submission step for virus-scan processing after a successful upload

### Phase 2: UX integration
- Add the documents page and navigation
- Add preview support for PDFs and images
- Add dashboard integration and status messaging

### Phase 3: Hardening and final verification
- Validate delete/edit flows and error handling
- Validate the async scan workflow and queue processing behavior
- Update documentation and confirm the app builds successfully

## Risks and Mitigations

- Risk: File storage and database persistence could diverge if upload fails mid-way. Mitigation: generate a unique path before database persistence and only write the database record after the file is stored successfully.
- Risk: Authorization bugs could expose documents to unauthorized users. Mitigation: enforce all authorization decisions in the service layer and validate the behavior through manual runs.
- Risk: The app may not have existing upload UI patterns. Mitigation: keep the first implementation simple and reuse Bootstrap-based forms and existing Blazor component patterns.

## Complexity Tracking

No constitution violations are expected for this feature.
