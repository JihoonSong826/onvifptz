# Testing and Validation Limits

[README](../README.md) | [Documentation index](index.md)

## Running checks

From the repository root:

```powershell
python -m unittest discover -v
python -m examples.offline_demo
```

Recorded result: **41 passing tests** on Windows / Python 3.12.14 on November 4, 2026. Tests use synthetic SOAP responses and localhost HTTP servers, not physical cameras. The code targets Python 3.11+, but other operating systems and versions require separate execution checks.

`127.0.0.1` is the computer's loopback address, not a Python virtual-environment address. Tests use temporary local servers; production uses actual device URLs. The offline demo uses no network.

This English package changes documentation and diagram labels only. Runtime code and tests are unchanged. The earlier result is not a new hardware validation claim.

## Reproduced review defects

| Defect | Fix | Regression evidence |
| --- | --- | --- |
| String seconds could terminate actor | Normalize before queueing; protect actor and settle waiters | String duration movement and Stop complete |
| Final Stop failure blocked re-registration | Release closed registration in finally | Failed remove followed by successful add |
| Shutdown Stop could wait forever | Reject on closing/finished actor with NotReady | No pending Stop caller in shutdown race |
| Manager close waited for caller service lifetime | Track add-completion Future | Close completes while caller remains alive |
| Slow headers exceeded response budget | Connected-socket shutdown watchdog | Header trickling bounded by deadline |

## Additional coverage

Media1/Media2 and Type=All, token/source selection, ambiguity rejection, unsupported axes, nonfinite/out-of-range values, Timeout limits, option gates, UsernameToken digest, HTTP Digest challenge, redirect rejection, response-size limits, camera isolation, movement replacement, Stop priority, cancellation-to-Stop, no movement retry after response loss, and explicit stub results.

## Review scope

An initial read-only review reproduced the five defects. A second independent review did not run because of a workspace credit error. Final changes were checked directly against code and regression tests; a completed second independent review is not claimed.

## Remaining validation

Hardware TLS/certificates, full WSDL/XSD conformance, official ONVIF testing, physical directions, device Timeout adherence, NVR forwarding, and model/firmware performance remain unverified. DNS and OS connection setup cannot be forcibly terminated by this worker design. See [safety](safety.md) and the [acceptance checklist](compatibility.md).
