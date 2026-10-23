# memex-jenkins

A [memex] adapter for the `ci` role in Jenkins. The role is specified in
memex's [ci.md].

## Reaching Jenkins

ADDR is the URL of a job, a folder or the server; a folder or the server
stands for every job under it. The adapter uses `api/json` with the user
and API token for ADDR's host in `~/.netrc`.

`detect` prints nothing.

## Commands

    runs COMMIT     each job's `builds`, keeping those that tested COMMIT
    run RUN         RUN/api/json
    suites RUN      RUN/testReport/api/json
    tests SUITE     the suite's `cases`
    logs ID         RUN/consoleText and RUN's `artifacts`
    retest RUN      JOB/buildWithParameters with the build's parameters

Capabilities: `bugs` is false, the others true.

## Run

- RUN is the build's `url`; each configuration of a matrix build is one.
- A build tested COMMIT when COMMIT is its `lastBuiltRevision.SHA1` or
  its GERRIT_PATCHSET_REVISION parameter.
- `result`: SUCCESS is pass, UNSTABLE and FAILURE fail, NOT_BUILT skip,
  ABORTED error.
- `links`: `commit`; `change` and `revision` from GERRIT_CHANGE_NUMBER
  and GERRIT_PATCHSET_NUMBER.

## Suite and test

- SUITE and TEST are RUN and the suite's and case's index, joined by `#`.
- Test `name`: `className` and `name`, joined by `.`.
- Test `result`: PASSED and FIXED are pass, FAILED and REGRESSION fail,
  SKIPPED skip.

## Open questions

- `builds` lists only the newest builds: how far back to look?
- A folder or server ADDR costs a request per job: require a job?
- A build of a merge has the merge's SHA1: match the head too?
- FAILURE with no failed test: fail or error?
- Pipeline stages as suites when there is no test report?

[memex]: https://github.com/roolebo/memex
[ci.md]: https://github.com/roolebo/memex/blob/main/ci.md
