# FileRise: account read-only flag skipped on file changes

https://github.com/error311/FileRise/security/advisories/GHSA-8r8m-mxx6-23pp

FileRise is a self-hosted PHP file manager (error311/FileRise). I reviewed v3.24.0, commit `765eccc`, and reported this on 2026-07-31. The vendor published the advisory on 2026-08-12. Fixed in 3.25.0. No CVE has been assigned.

The product documents an administrator-controlled "Read-only" flag as view and download only. The interface hides write actions for that account, and the folder-capability response applies the flag when it draws the UI. The server did not apply it on every mutation.

What already enforced the flag: ordinary file and folder copy, and folder create, delete, and rename. What did not: creating a file, overwriting one, uploading, and deleting a file into trash. A read-only user can do those in any folder they already own or already hold an ACL on. They do not gain read access to anything new. Trash filled this way is not browsable or restorable by a non-admin, which is why availability is Low rather than none. Integrity is High because files can be replaced.

I wrote the first report as if no write path checked the flag. That was too broad. The vendor corrected it, and the published advisory describes partial enforcement. I agreed.

Two adjacent gaps stayed out of this advisory on purpose. A read-only account can still create a file-share link on 3.25.0. That is the separate `canShare` behavior, not this bug. `canZip` and `viewOwnOnly` were the same kind of leftover. The vendor asked for them to be evaluated on their own. I left them out.

| | |
|---|---|
| Affected | 3.24.0 and earlier |
| Fixed | 3.25.0 |
| Severity | High, 7.1 |
| Vector | CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:L |
| CWE | CWE-862, CWE-863 |
| Privileges | Logged-in user who already has folder ACL or ownership, and whose account is marked read-only |
| CVE | Not assigned |

3.25.0 treats the flag as a hard ceiling over folder ownership, direct ACLs, inherited ACLs, and Pro group grants. The same ceiling covers same-source folder moves, WebDAV writes, ONLYOFFICE edits, portal uploads, and background transfers. I retested the file create, overwrite, and delete paths. They are refused. The share-link gap is still there and is not this issue.

The vendor was going to request the CVE. I did not file another one. As of 2026-09-22 the advisory has no CVE id.

Local Docker only, `error311/filerise-docker` at v3.24.0 and v3.25.0.

L0stHeart
https://github.com/L0stHeart
