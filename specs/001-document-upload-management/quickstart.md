# Quickstart: Document Upload and Management

## Prerequisites

- .NET 8 SDK installed
- The application running locally with the seeded users available
- A sample PDF, image, or text file for upload testing

## Validation Scenarios

1. Start the app with `dotnet run`.
2. Sign in as a seeded user such as `ni.kang@contoso.com`.
3. Open the Documents page and upload a supported file with a title and category.
4. Confirm that the document appears in the personal documents list and that the metadata is shown correctly.
5. Try uploading a file that exceeds 25 MB or uses an unsupported extension and confirm that the submission is rejected.
6. Open the project documents view for a project the user belongs to and confirm the document is visible there.
7. Verify that a different user without access cannot see the document in their documents list.
8. Confirm that preview works for a PDF or image file and that downloads are available for allowed documents.
