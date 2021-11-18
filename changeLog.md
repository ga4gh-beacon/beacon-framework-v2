## Change log for beacon-framework-v2
This document is planned as a manually maintained list of changes to the Beacon Framework version 2 repo.
It should only include changes that affect or could affect implementations, and the log would be updated *after* the corresponding pull request is approved and merged.

**nonFilteredQueriesAllowed** - 2021/11/18
[PR #66](https://github.com/ga4gh-beacon/beacon-framework-v2/pull/66)
Adding an optional element both to entryType & configuration for declaring if queries without any filter are allowed. Default value is *true*

**making numTotalResults mandatory for count responses** - 2021/11/03
[PR #62](https://github.com/ga4gh-beacon/beacon-framework-v2/pull/62)

**Adding declarative granularity in req and response** - 2021/10/30
[PR #59](https://github.com/ga4gh-beacon/beacon-framework-v2/pull/59)
Declarative granularity added to request (*requestBody*) and response (*receivedRequestSummary* and *responseMeta*).
The granularity is now a common element and is referenced from requestBody and in receivedRequestSummary and responseMeta.
It is mandatory in *receivedRequestSummary* and *responseMeta*, as they are informative similarly to pagination and other elements.
Current validations for requests and responses will fail.
