# sudo-fleet-selftest

End-to-end selftest repository for the **sudo-fleet**.

This repo exists to prove the full machine-side GitHub path works, end to end:
repository creation via the REST API, an authenticated clone, a local commit,
and a push to `main` that `git ls-remote` can independently confirm.

## What it verifies

- Authenticated git-over-HTTPS and REST API access on the `cmk20` account.
- Create-and-clone against a brand new public remote.
- Commit and push of a real artifact (this file) to `main`.
- Remote ref agreement: local `HEAD` equals `origin/main` on the server.

## Usage

Nothing to run. If the repository contains a commit on `main` and
`git ls-remote origin main` reports that same sha, the selftest passed.

```sh
git ls-remote https://github.com/cmk20/sudo-fleet-selftest.git main
```

## Contents

| Path        | Purpose                                        |
|-------------|------------------------------------------------|
| `README.md` | Description of the selftest (this file).       |

## Notes

Generated as a one-shot provisioning check. It is a public repository and
holds no credentials, secrets, or operational data.
