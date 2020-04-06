# Instrumentation Checklist

## Span Status and Creation
* All error conditions under a given span appropriately set the span status to an error state.
* RPC framework result codes are mapped to span status (i.e., Internal Error, Not Found, etc.).
* All spans that are started are also finished, even in the case of unrecoverable errors if possible.
* Spans should only represent work that is semantically important to the request life cycle of a service; try not to create spans around endpoints only receiving synthetic traffic, like a /status or /health endpoint.

## Span Boundaries
* Egress and ingress spans have appropriate labels (SpanKind is set).
* Egress and ingress spans have appropriate relationships (client/server, consumer/producer).
* Internal spans are appropriately labeled and do not imply a remote call.

## Attributes
* Spans include a version attribute for the service they represent.
* Spans that represent work by a dependency have an attribute for that dependency's version.
* Spans should include attributes identifying underlying infrastructure:
    * Hostname / FQDN
    * Container name, if appropriate
    * Runtime version
    * Application server version, if applicable
    * Region or availability zone
* Attributes are namespaced where appropriate (i.e., to prevent collisions between key names where the semantic meaning of the key differs between services in a request).
* Attributes with numerical values should include the unit of measurement in the key name (i.e, payload_size_kb versus payload_size).
* Attributes should not contain any PII.

## Events
* Useful and descriptive event messages that would be useful for upstream or downstream service users should be added:
    * Request-response payloads (sanitized)
    * Stack traces, exceptions, and error messages
* Long-running operations (such as waiting for a mutex) should be wrapped in events; one when the operation begins, and one when it ends.