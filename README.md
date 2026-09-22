# FileRise: account read-only flag skipped on file changes

https://github.com/error311/FileRise/security/advisories/GHSA-8r8m-mxx6-23pp

Affects FileRise 3.24.0 and earlier. Fixed in 3.25.0.

High. CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:L (7.1).
CWE-862, CWE-863.
No CVE assigned.

The administrator "Read-only" flag is documented as view and download only. The interface hides write actions, and some server paths already enforced the flag: ordinary file and folder copy, and folder create, delete, and rename. Creating, overwriting, and deleting files did not. A read-only account can still change files in folders it already owns or holds an ACL on. It does not gain any new read access.

Reported 2026-07-31 through GitHub private vulnerability reporting. The vendor published the advisory on 2026-08-12.

L0stHeart
