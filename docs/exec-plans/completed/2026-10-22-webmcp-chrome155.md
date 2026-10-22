# WebMCP input support for Chrome 155

## Goal

Fix tool execution on Chrome 155, retain older Chrome support, and install the tested local fix in the browser profiles that use Open Browser Use.

## Scope

- Base the fork branch on upstream main at b136cac.
- Change the shared WebMCP bridge and add regression checks.
- Keep unrelated checkout changes and browser profile data.
- Build the extension, keep an install backup, and verify a real PM read.

## Risks and controls

- Retry only the older native API's input parse error before a tool starts. Do not retry site tool failures.
- Keep extension IDs and browser settings. Use the supported extension UI when Chrome requires confirmation.
- Keep an install record outside the public repository. Do not commit local paths or browser account data.

## Checks

- Run the regression before and after the fix.
- Run extension tests, package checks, and the repository CI checks that apply.
- Verify discovery and a read-only tool call in a real Chrome 155 tab.
- Record each installed profile and any remaining manual action.

## Progress

- [x] Confirm upstream main still sends a JSON string.
- [x] Create an isolated fork branch and link existing dependencies.
- [x] Fix and test the bridge.
- [x] Build, back up, install, and verify the local extension.
- [x] Prepare the fork branch and record the result.

## Result

- The fork uses object input first. It retries only the older native input parse error. Both thrown errors and rejected promises are covered.
- All 20 extension tests pass. The package build and repository checks pass.
- Ten local Chrome profiles run the fixed bridge. Three retain their prior local changes at extension version 0.1.42.2. Seven use the clean fork extension at 1.1.0.1.
- A native read-only probe passed once per profile on Chrome 155. A read on the existing work dashboard also passed after a page reload.
- Prior files are backed up. The CLI and native host are unchanged. The profile install record stays outside the repository.
- Test tabs and setup windows are closed. The work dashboard remains open.

## Next run

1. Check the running extension version with `info`; the profile inventory can show a file version before Chrome reloads it.
2. Reload the target page after an extension update. List tools again to obtain current registration IDs.
3. Run one read-only tool before any task changes. A successful list does not prove execution works.
4. Use the native Chrome extension UI for local package changes. Keep each UI batch short and verify the result after each profile.
