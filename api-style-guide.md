# Adello API Styleguide

## General
* API's are binding contracts between different domains and/or teams. They represent domain boundaries.
* API's main purpose is to decouple teams (with potentially different velocities) and abstract technology choices
* API design decisions should be mainly from an outside-in from an user and business perspective, not driven from concrete implementations or data persistence models

## Rules
* Every new API needs to be reviewed by CTO, and Dev Management - no exceptions.
* Every change to an existing API needs to be reviewed within the team owning the API.
* Every breaking change (i.e. requiring versioning) of an API is treated like a new API. (See first rule)

## Best Practices
* API's are documented in YAML following the Swagger / Open API Spec
* Each API definition is stored in a single repository to facilitate sharing
* Consider adding API mocks into the same repo to allow initial API integration without requiring access to the actual API backend
* Syntax and terminology must be consistent not within just a single API but also across all APIs in the company.
* Use human readable and commonly understandable naming over programmatic or internal names
* Read and adhere to [Postel's law](https://en.wikipedia.org/wiki/Robustness_principle) for robust API design (i.e. receiving empty parameters or unknown fields are ignored, not an error)
* Avoid API versioning until and only when you must - extend the API instead
* Adherence to [Postel's law](https://en.wikipedia.org/wiki/Robustness_principle) allows for ignoring unknown fields by the receiver, therefor a tight coupling of having to upgrade both API provider and API client can be avoided
* Service URI should follow the scheme 'v1/<resources>' (the version identifier is a escape hatch when extending the AI is not longer useful or possible)
* Define the version 'v1' as the base path property in Swagger 
* Relying on pre-defined and hardcoded URI schemes constitutes a form of close coupling - embrace [hypermedia style relationship attribute links](https://en.wikipedia.org/wiki/Hypertext_Application_Language) instead
* Relationship attribute links allow the avoidance of sub resource structures by flattening the resource endpoint depth to exactly one (i.e. /resources/{resource-id})
* Avoid sub-resource hierarchies. They tend to make sense in the beginning but quickly make the API hard to understand and does not scale. There is also no consistency in the uri schema anymore since some sub-resources are singular, i.,e. report only exists exactly ones per campaign
* Stay within single resource dimension and create multiple endpoints over deep resource hierarchies.
* The swagger file needs to be importable to the AWS API Gateway. This imposes certain constrains on the syntax. (Note: An easy way to check if your swagger file is compatible, go to the AWS Management console, select API Gateway service and try to import your swagger to create a new gateway. You can ignore the warnings regarding unable to create documentation due to the use of '-' in attribute names)

## Syntax
* Do not use CamelCase for human readable attribute names
* Do use CamelCase for model types due to AWS import constrain
* Use '-' instead of '_'
* Use small cap
* Use plural for parameter arrays, and resource endpoints (i.e. /accounts and /accounts/{account-id})
* URL query property sets are separated with a comma (i.e. options=1,2,3) or the pipe symbol (i.e. options=1|2|3)
* Empty URL query parameters or unknown fields should be explicitly ignored (applying [Postel's law](https://en.wikipedia.org/wiki/Robustness_principle))

## Response Codes
Do pay attention to response codes (and read this excellent rest-api-error-codes-101 primer)
* 1xx: Informational - Communicates transfer protocol-level information
* 2xx: Success -Indicates that the client’s request was accepted successfully.
* 3xx: Redirection - Indicates that the client must take some additional action in order to complete their request
* 4xx: Client Error - This category of error status codes points the finger at clients.
* 5xx: Server Error - The server takes responsibility for these error status codes.

For health check api, use the following
* 200 - Service Up
* 503 - Service Down
* 500 - Service status can not be determined

# Additional Reading

## General
* https://blog.restcase.com/5-basic-rest-api-design-guidelines/
* https://haufe-lexware.gitbooks.io/haufe-api-styleguide/content/
* https://haufe-lexware.gitbooks.io/haufe-api-styleguide/content/further-reading/further-reading.html

## Status Codes
* https://blog.restcase.com/rest-api-error-codes-101/

## Healthchecks
* https://medium.com/ether-labs/designing-a-health-check-api-a98784eb6a07
* https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/twlp_microprofile_healthcheck.html
