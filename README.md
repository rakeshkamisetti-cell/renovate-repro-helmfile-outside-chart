# Reproduction: helmfile local chart outside the repository aborts the run

`helmfile.yaml` has a release whose `chart:` resolves outside the repository
(a relative path into a sibling repo, and a templated `{{ .Values.chartRepo }}/my-chart`
that becomes `/my-chart` after Renovate strips templates).

## Current behavior

The whole repository run fails:

```
ERROR: Repository has unknown error
       "err": { "message": "file-access-violation-error" }
```

so `bitnami/memcached` (and every other dependency in the repo) never gets an update PR.

## Expected behavior

The out-of-repo charts are skipped (`skipReason: local-chart`) and `memcached`
6.0.0 gets an update PR as usual.
