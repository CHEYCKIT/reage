# Reage for Windows — portable build

The Windows portable release is a single x64 executable. It does not require a separate Python installation or an installation wizard. The frontend runs in an Electron window and the PDF-processing API runs locally as a bundled Windows executable.

## Create a release

1. Open the Actions tab in the CHEYCKIT/reage repository.
2. Select **Windows portable release**.
3. Select **Run workflow** on the main branch.
4. Wait for the workflow to finish, then open the release named **Reage vX.Y.Z — Windows Portable** on the repository's Releases page.

The workflow builds the Vite frontend, freezes the Python/FastAPI service with PyInstaller, smoke-tests the bundled API, packages the Electron portable executable, generates a SHA-256 checksum, and publishes both files as GitHub release assets. The portable tag is named portable-vX.Y.Z so it does not collide with an upstream source release tag.

## Runtime and privacy

PDF operations use the bundled local service bound to 127.0.0.1 on a free local port. Uploaded PDFs are processed on the same machine and kept in the service's in-memory session, not sent to Reage's hosted API. Export the edited PDF before closing the app because sessions are temporary.

First-use OCR language models and optional Google Fonts downloads need an internet connection. Cache files are kept beside the portable executable in a ReageData folder when that location is writable; otherwise the app uses the current user's application-data folder. No PDF documents are intentionally written to this cache.

## Windows notes

The build is unsigned. Windows SmartScreen or antivirus software may show a first-run reputation prompt. The checksum asset lets you verify the downloaded executable, but it is not a code-signing certificate.

Generated build output, the server executable, the icon, and release files are produced by the workflow and are not committed to the source tree.
