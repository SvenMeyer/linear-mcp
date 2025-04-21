# Release Notes - v1.0.1

## Fixes

*   **Authentication:** Corrected the handling of Linear Personal Access Tokens (PATs). The server now correctly uses the `apiKey` option when initializing the `@linear/sdk` client, resolving authentication errors caused by an incorrect "Bearer" prefix being added to PATs. (`src/auth.ts`)

## Documentation

*   **README Update:** Updated `README.md` to clarify the different methods for providing the `LINEAR_ACCESS_TOKEN`. It now emphasizes that for reliable integration with the VS Code Cline extension, the token **must** be provided in the `env` block of the `cline_mcp_settings.json` file. Instructions for finding the PAT in Linear settings were also improved.

## Known Issues

*   The server code does not currently load environment variables from a `.env` file automatically. This would require adding a dependency like `dotenv`.
*   Relying solely on system environment variables (e.g., from `.bashrc`) is not sufficient for the VS Code extension integration, as the extension requires the token to be explicitly defined in its `cline_mcp_settings.json` file.
