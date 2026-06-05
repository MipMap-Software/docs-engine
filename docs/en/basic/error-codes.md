---
title: Error Code Reference
sidebar_position: 4
---

## Error Code Reference

MipMapEngine SDK uses a standardized error code system to help developers quickly locate and resolve issues. Error codes are grouped into reconstruction API errors, license errors, file I/O errors, GPU errors, and more.

### Reconstruction API Error Codes

#### Core Logic

| Code | Description |
|--------|------|
| 0 | Reconstruction succeeded |
| 1000 | Reconstruction failed; check logs for details |
| 1001 | Insufficient memory |
| 1002 | Coordinate system error |
| 1003 | Empty AT result |
| 1004 | Empty point cloud result |
| 1005 | Empty mesh result |
| 1006 | Insufficient orthophoto images; cannot generate 2D deliverables |

#### License

| Code | Description |
|--------|------|
| 2000 | License error |
| 2001 | Failed to enumerate licenses |
| 2002 | No matching license found |
| 2003 | License expired |
| 2004 | License not valid for this version |
| 2005 | License feature module mismatch |
| 2006 | Exceeded maximum image count supported by license |

#### File I/O

| Code | Description |
|--------|------|
| 3000 | File read/write error |
| 3001 | JSON field parse error |
| 3002 | Image read error |
| 3003 | File creation error |
| 3004 | Machine learning model load error |

#### GPU / Driver

| Code | Description |
|--------|------|
| 4000 | GPU device error; verify NVIDIA GPU and driver version |

#### LiDAR Reconstruction

| Code | Description |
|--------|------|
| 5000 | Point cloud file load error |
| 5001 | Timestamp mismatch between point cloud and trajectory file |

### License Engine Error Codes

| Code | Description |
|--------|------|
| 0 | Success |
| 0x00000040 | Network error |
| 0x0000004A | Network request timeout |
| 0x05000004 | Server not found |
| 0x13000051 | License requires online activation |
| 0x02000003 | Connection failed; Virbox not installed or not connected to network |
| 0x51005001 | Exception while querying license code status |
| 0x51005002 | Exception during license code redemption |
| 0x51005003 | License code does not exist |
| 0x51005004 | License deduction failed |
| 0x51005013 | Cannot bind (license expired) |
| 0x51005014 | Cannot bind (concurrent device limit reached) |
| 0x51005015 | Cannot bind (cumulative device binding limit reached) |
| 0x51005018 | Terminal unbind failed; contact vendor |
| 0x51005019 | Server cannot find binding record; contact vendor |
| 0x51005021 | License cannot bind (locked); contact vendor |
| 0x51005025 | License code does not allow binding |
| 0x51005033 | Cannot activate license temporarily; upgrade Virbox user tool |
| 0x51005034 | Vendor revoked device access; license code cannot bind |
| 0x5100612F | License code does not exist; verify the code |
| 0x51006130 | Invalid hardware information data |
| 0x51006134 | License deduction failed; contact vendor |
| 0x5100502C | Cannot unbind (not bound) |
