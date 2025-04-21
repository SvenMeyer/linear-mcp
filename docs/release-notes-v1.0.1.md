# Release Notes - v1.0.1

## Fixes

*   **Authentication:** Corrected the handling of Linear Personal Access Tokens (PATs). The server now correctly uses the `apiKey` option when initializing the `@linear/sdk` client, resolving authentication errors caused by an incorrect "Bearer" prefix being added to PATs. (`src/auth.ts`)

## Documentation

*   **README Update:** Updated `README.md` to clarify the different methods for providing the `LINEAR_ACCESS_TOKEN`. It now emphasizes that for reliable integration with the VS Code Cline extension, the token **must** be provided in the `env` block of the `cline_mcp_settings.json` file. Instructions for finding the PAT in Linear settings were also improved.

## Known Issues

*   The server code does not currently load environment variables from a `.env` file automatically. This would require adding a dependency like `dotenv`.
*   Relying solely on system environment variables (e.g., from `.bashrc`) is not sufficient for the VS Code extension integration, as the extension requires the token to be explicitly defined in its `cline_mcp_settings.json` file.

## Troubleshooting Details (v1.0.1 Fix)

*   **Initial Problem:** Attempts to use the server with a Personal Access Token (PAT) failed with authentication errors indicating an incorrect "Bearer" prefix was being added to the token.
*   **Investigation:**
    *   Confirmed the server process was running.
    *   Verified the PAT was correctly set as an environment variable (`LINEAR_ACCESS_TOKEN`) and also tested adding it to the `cline_mcp_settings.json` `env` block.
    *   Examined the `@linear/sdk` source code (`packages/sdk/src/client.ts`) and found that it correctly uses the `apiKey` option for PATs (without adding "Bearer") and the `accessToken` option for OAuth tokens (adding "Bearer").
    *   Examined the `linear-mcp` server code (`src/auth.ts`) and found it was incorrectly using the `accessToken` option when initializing the SDK client with a PAT.
*   **Solution:** Modified `src/auth.ts` to use the `apiKey` option when initializing `LinearClient` for the PAT authentication flow.
*   **Configuration Findings:** Further testing confirmed that for the VS Code extension integration:
    *   Setting the token in `cline_mcp_settings.json` is required and works reliably.
    *   Setting the token only as a system environment variable (e.g., via `.bashrc`) is insufficient for the extension.
    *   Setting the token in a `.env` file is not currently supported by the server code without modifications (e.g., adding `dotenv`).

## Next Steps

*   **Enhance Token Handling:** Update the server code (`src/index.ts`) to optionally load the `LINEAR_ACCESS_TOKEN` from a local `.env` file (e.g., using the `dotenv` package) as an alternative to the `cline_mcp_settings.json` method.
*   **Implement OAuth:** Implement the OAuth 2.0 authentication flow as described in the README (`#### OAuth Flow (Alternative) ***NOT IMPLEMENTED***`), including handling the callback and token refresh logic.
