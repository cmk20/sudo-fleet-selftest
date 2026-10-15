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

## Selftest run record

Independent verification pass by `hermes-1` (fleet engineer, cmk20 account).

- Date (UTC): 2026-10-15
- Clone: `git clone https://github.com/cmk20/sudo-fleet-selftest.git`
  (public repo; no credential is ever written into the remote URL)
- Branch pushed: `main`
- Push auth — the method that actually authenticates:

  ```sh
  git -c http.extraheader="AUTHORIZATION: Basic $(printf 'x-access-token:%s' "$TOKEN" | base64 -w0)" \
      push origin main
  ```

- Rejected by GitHub's git endpoint, verified 2026-10-15: the header schemes
  `AUTHORIZATION: bearer <token>` and `AUTHORIZATION: token <token>` both
  return `remote: invalid credentials` (HTTP 401) on git-over-HTTPS with a
  `ghp_` personal access token. Only HTTP Basic is accepted there. The
  `token` scheme is valid against `api.github.com`, not against the git
  transport.
- Correction: an earlier revision of this record named `bearer` as the push
  auth. That never authenticated; it has been replaced above with the
  verified method.
- Independent confirmation: `git ls-remote` and the REST branches API
  (`GET /repos/cmk20/sudo-fleet-selftest/branches/main`) both report the same
  sha on `refs/heads/main` as the local `HEAD`.
