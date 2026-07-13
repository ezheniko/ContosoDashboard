# Research: Document Upload and Management

## Decision: Local filesystem storage with an interface abstraction

**Decision**: Use a dedicated `IFileStorageService` abstraction with a `LocalFileStorageService` implementation that stores uploaded files under a local directory outside the web root.

**Rationale**: The constitution requires offline capability and a training-friendly implementation. A local storage implementation satisfies this requirement and preserves a straightforward migration path to Azure Blob Storage later without changing the app's business logic.

**Alternatives considered**:
- Storing files directly in the database as BLOBs — rejected because it would be less aligned with the existing offline-first guidance and would complicate future cloud migration.
- Storing files under `wwwroot` — rejected because it introduces security concerns and makes direct file access less controlled.
- Using a cloud-only implementation — rejected because the feature must work fully offline for training.

## Decision: Service-layer authorization and validation

**Decision**: Enforce document authorization, validation, and visibility rules in a dedicated document service rather than relying on UI logic alone.

**Rationale**: The constitution and the existing application patterns emphasize service-layer security checks to prevent IDOR and protect data access. Centralizing these checks makes the feature easier to reason about and safer to extend.

**Alternatives considered**:
- Relying only on page-level checks — rejected because it leaves the system vulnerable to bypasses and inconsistent rules.
- Embedding authorization checks in Razor components — rejected because it couples UI behavior to sensitive data rules.

## Decision: MVP scope limited to upload, browse, search, and access control

**Decision**: The MVP will provide document upload, metadata capture, personal and project browsing, search, preview for PDFs and images, and basic access control. Sharing, notifications, and reporting will be deferred.

**Rationale**: This aligns with the clarified MVP decision and keeps the first implementation small and verifiable while still delivering tangible user value.

**Alternatives considered**:
- Including sharing and notifications in the initial release — rejected because it expands the surface area and introduces more coordination work before the core document experience is stable.
- Including full audit reporting in the MVP — rejected because it adds complexity without being required to validate the core value proposition.

## Decision: Background virus scanning via Azure Functions and Queue Storage

**Decision**: Use Azure Functions with Queue Storage triggers to process uploaded files asynchronously after the initial upload succeeds. The application will enqueue a scan request message containing the document identifier, storage path, and file metadata, and the background function will update the document state when scanning completes.

**Rationale**: The requirement calls for virus scanning and the architecture should remain compatible with the existing offline-first training app while also supporting a realistic cloud-ready pattern. Queue-based background processing avoids blocking the upload user experience and provides a clear place to add future scanning integrations.

**Alternatives considered**:
- Scanning synchronously inline during upload — rejected because it would increase upload latency and make the user experience feel slow.
- Polling a local worker process — rejected because it does not align with the intended Azure-oriented background processing pattern or the future migration path.
