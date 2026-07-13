# Tasks: Document Upload and Management

**Input**: Design documents from `/specs/001-document-upload-management/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

## Constitution-Driven Quality Tasks

- [ ] T000 Validate that the work preserves the offline training experience
- [ ] T000 Add or update manual verification steps for the document upload and browse flows
- [ ] T000 Update README or stakeholder documentation if user-visible behavior changes
- [ ] T000 Verify authentication, authorization, and data isolation for protected document flows

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and the supporting infrastructure for document management

- [ ] T001 Create the document feature folder structure under the existing Blazor project layout
- [ ] T002 [P] Add configuration support for storage path and upload limits in appsettings.json and related configuration code
- [ ] T003 [P] Add the initial document-related models and DbContext wiring for Document, DocumentShare, and DocumentActivity

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before user story work can begin

- [ ] T004 Implement the storage abstraction interface and the local filesystem implementation for uploads
- [ ] T005 [P] Register the storage service and document service in Program.cs with dependency injection
- [ ] T006 Create the document service with validation, authorization, upload, browse, search, update, and delete operations
- [ ] T007 Define the queue message contract and add the upload-to-queue submission step for async virus-scan processing
- [ ] T008 Add the document-related database schema support and seed-safe initialization logic for the new entities

**Checkpoint**: Foundation ready - user story implementation can now begin

---

## Phase 3: User Story 1 - Upload and organize documents (Priority: P1) 🎯 MVP

**Goal**: Enable employees to upload supported documents with metadata and see them appear in the appropriate personal or project view.

**Independent Test**: A logged-in user can upload a supported document, enter metadata, and confirm the document appears in the correct document list.

### Implementation for User Story 1

- [ ] T009 [P] [US1] Create the Document model in ContosoDashboard/Models/Document.cs
- [ ] T010 [P] [US1] Create the DocumentShare and DocumentActivity models in ContosoDashboard/Models/
- [ ] T011 [US1] Extend ContosoDashboard/Data/ApplicationDbContext.cs with the document DbSets and relationships
- [ ] T012 [US1] Implement document upload validation and persistence in ContosoDashboard/Services/DocumentService.cs
- [ ] T013 [US1] Implement the local file storage service in ContosoDashboard/Services/LocalFileStorageService.cs
- [ ] T014 [US1] Add the upload UI and metadata form in a new documents page under ContosoDashboard/Pages/
- [ ] T015 [US1] Add success and error messaging for upload completion and rejection cases

**Checkpoint**: User Story 1 should be fully functional and independently testable

---

## Phase 4: User Story 2 - Browse, search, and manage documents (Priority: P2)

**Goal**: Enable users to browse, filter, sort, and search documents they are authorized to access.

**Independent Test**: A user can open the document views, search for a known document, and confirm that only authorized documents are shown.

### Implementation for User Story 2

- [ ] T016 [P] [US2] Add document list filtering, sorting, and search support in the document service
- [ ] T017 [US2] Create the documents browsing page and project-based document view under ContosoDashboard/Pages/
- [ ] T018 [US2] Add preview handling for PDF and image files in the documents UI
- [ ] T019 [US2] Add authorization-aware document listing and access checks for personal and project views
- [ ] T020 [US2] Add delete and metadata edit flows for allowed documents

**Checkpoint**: User Stories 1 and 2 should both work independently

---

## Phase 5: User Story 3 - Share and control document access (Priority: P3)

**Goal**: Allow document owners to share documents with specific users and maintain access control for those shares.

**Independent Test**: A document owner can share a document with another authorized user and confirm access is granted while unauthorized users remain blocked.

### Implementation for User Story 3

- [ ] T021 [P] [US3] Implement document share creation and lookup logic in the document service
- [ ] T022 [US3] Add a basic share UI for selecting a recipient and creating the share
- [ ] T023 [US3] Add notification support for shared-document access events
- [ ] T024 [US3] Add authorization checks for shared-document access and ensure unauthorized access remains blocked

**Checkpoint**: All user stories should now be independently functional

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] T025 [P] Add dashboard integration for recent documents and document counts
- [ ] T026 [P] Add navigation entry points for documents in the main menu
- [ ] T027 Add documentation updates and quickstart validation for the new feature
- [ ] T028 Run dotnet build and perform manual validation of the upload, browse, preview, and access-control flows
