# FileRise read-only accounts can still change files

https://github.com/error311/FileRise/security/advisories/GHSA-8r8m-mxx6-23pp

Severity: high (CVSS 7.1, `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:L`). CWE-862, CWE-863. No CVE.

FileRise 3.24.0 and earlier is affected. Fixed in 3.25.0.

The administrator read-only flag is documented as view and download only. The interface hides write actions, and the folder-capability response applies the flag when it draws that interface. The server did not apply it on every mutation.

Copy of a file or a folder, and folder create, delete, and rename, already enforced the flag. Creating a file, overwriting one, uploading, and deleting a file into trash did not. A read-only user can do those in any folder they already own or already hold an ACL on. They do not gain read access to anything new. Trash filled this way is not browsable by a non-admin, which is why availability is low. Integrity is high because files can be replaced.

The first write-up said no write path checked the flag. That was too broad. The published advisory describes partial enforcement. I agreed with that correction.

A read-only account can still create a file-share link on 3.25.0. That is separate `canShare` behavior, and the vendor asked for it to stay out of this advisory, along with `canZip` and `viewOwnOnly`. I left them out.

3.25.0 treats the flag as a ceiling over folder ownership, direct ACLs, inherited ACLs, and Pro group grants. The same ceiling covers same-source folder moves, WebDAV writes, ONLYOFFICE edits, portal uploads, and background transfers. I retested file create, overwrite, and delete. They are refused.

The vendor was going to request the CVE. I did not file another one. As of 22 September 2026 the advisory has no CVE id.

Local Docker only, `error311/filerise-docker`, v3.24.0 (commit 765eccc) and v3.25.0.

Reported privately on 31 July 2026. The vendor published the advisory on 12 August 2026.

L0stHeart
