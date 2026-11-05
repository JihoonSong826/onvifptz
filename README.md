# Python ONVIF PTZ Control

An asynchronous Python library for connecting to and controlling multiple ONVIF PTZ cameras. Targets Python 3.11+ with no third-party runtime dependencies.

**Validation status:** 41 offline tests passed on Windows with Python 3.12.14 on November 4, 2026. Physical cameras and NVRs have not been tested. This is not an ONVIF-certified product.

## Features

- Device and Media1/Media2 queries, PTZ profile selection, and SOAP authentication.
- Continuous movement, Stop, status, and preset queries; optional position movement and persistent settings.
- Independent per-camera workers, bounded queues, latest pending movement, priority Stop, and time limits.
- Discovery, RTSP playback, event subscriptions, storage, and display remain explicit `not_implemented` stubs.

## System architecture

![System integration and PTZ boundary](docs/images/system-block.png)

Video and PTZ share a `camera_id` mapping. The interface is an in-process Python API; it does not include a REST or gRPC server. See [architecture](docs/architecture.md).

## Quick start

Run from the repository root with Python 3.11 or later:

```powershell
python -m unittest discover -v
python -m examples.offline_demo
```

Neither command connects to a physical camera. Some tests start a temporary HTTP server on `127.0.0.1`; the demo uses simulated responses without networking.

To install into another Python project:

```powershell
python -m pip install -e .
```

Setuptools is a build dependency, not a runtime dependency.

Real-device use requires the device's ONVIF URL, credentials, and target profile. **The real-camera example sends Stop during connection and shutdown even without the movement flag.** Read [integration](docs/integration.md) and [safety](docs/safety.md) first.

## Documentation

- [Architecture and block diagrams](docs/architecture.md)
- [Integration and connection contract](docs/integration.md)
- [Public API and errors](docs/api.md)
- [Concurrency and safety](docs/safety.md)
- [Camera and NVR compatibility](docs/compatibility.md)
- [Standards and implementation rationale](docs/standards.md)
- [Testing and validation limits](docs/testing.md)

## License

[MIT](LICENSE). Commercial use is permitted; retain the license notice when distributing the code. See [standards and references](docs/standards.md) for the scope of external reference material.
