---
title: Troubleshooting
sidebar_position: 1
---

## Troubleshooting

This page provides diagnostics and solutions for common issues when using the MipMapEngine SDK.

### Diagnostic Tools

When you encounter a problem, start with these diagnostic tools:

#### Hardware Check

```bash
hardware_check.exe
```

#### License Status

On Windows, open **Virbox User Tool** to view activation binding, or check bound licenses from the command line:

```bash
license_engine.exe --enum /path/to/license.json
```

On Linux, run the following command to view bound licenses:

```bash
ssclt -l
```

### Common Errors and Solutions

### Configuration File Errors

##### Error: JSON Parse Failed

**Error message**:
```
[ERROR] JSON parse failed: expected ',' or '}' at line 45
```

**Cause**: Invalid JSON syntax, usually a missing or trailing comma.

**Solution**:
1. Validate syntax with a JSON formatter
2. Ensure there is no comma after the last element
3. Check for unclosed brackets or quotes

**Example fix**:
```json
// Wrong
{
  "image_meta_data": [
    {"id": 1, "path": "image1.jpg"},  // No comma after the last element
  ]
}

// Correct
{
  "image_meta_data": [
    {"id": 1, "path": "image1.jpg"}
  ]
}
```

#### Error: Image File Not Found

**Error message**:
```
[ERROR] Image file not found: C:/images/DJI_0001.JPG
```

**Cause**: The image path is wrong or the file does not exist.

**Solution**:
1. Check path spelling
2. Confirm the file exists
3. Use the correct path separator (prefer `/`)
4. Try an absolute path

**Path examples**:
```json
// Windows - recommended
"path": "C:/Projects/Images/DJI_0001.JPG"

// Windows - also valid
"path": "C:\\Projects\\Images\\DJI_0001.JPG"

// Relative path
"path": "../images/DJI_0001.JPG"
```

#### Error: Parameter Out of Range

**Error message**:
```
[ERROR] Invalid resolution_level: 5 (should be 1-3)
```

**Cause**: The parameter value is outside the allowed range.

**Solution**:
Check and correct parameter values:
- `resolution_level`: 1 (high), 2 (medium), 3 (low)
- `input_image_type`: 1 (RGB), 2 (multispectral), 3 (infrared)
- `coordinate_system.type`: 1 (projected), 2 (geographic), 3 (geocentric)

### Image Processing Errors

#### Error: Insufficient Images

**Error message**:
```
[ERROR] Insufficient images for reconstruction. Minimum 5 images required, found 3.
```

**Cause**: Too few images for 3D reconstruction.

**Solution**:
- Provide at least 5 images
- Ensure sufficient overlap between images (recommended >60%)
- Check whether images were filtered automatically (see logs)

#### Error: No GPS Information

**Error message**:
```
[WARNING] No GPS information found in image EXIF
```

**Cause**: No GPS data in image EXIF.

**Solution**:

1. **Use custom POS data**:
```json
{
  "id": 1,
  "path": "image.jpg",
  "meta_data": {
    "pos": [114.123, 22.456, 100.5],
    "pos_sigma": [0.5, 0.5, 1.0]
  }
}
```

2. **Relax position constraints**:
```json
{
  "use_image_position_constraint": false
}
```

### Multi-Camera Reconstruction Issues

#### Error: Camera Group Image Count Mismatch

**Error message**:
```
[ERROR] Camera group image count mismatch: nadir(50), oblique_f(48)
```

**Cause**: Image counts differ across camera groups.

**Solution**:
1. Check for missing images
2. Ensure multi-camera sync triggering works correctly

**Correct grouping example**:
```json
{
  "image_meta_data": [
    // Each position should have images from all cameras
    {"id": 1, "path": "pos1/nadir.jpg", "group": "nadir"},
    {"id": 2, "path": "pos1/forward.jpg", "group": "forward"},
    {"id": 3, "path": "pos1/backward.jpg", "group": "backward"},
    // ... next position
    {"id": 4, "path": "pos2/nadir.jpg", "group": "nadir"},
    {"id": 5, "path": "pos2/forward.jpg", "group": "forward"},
    {"id": 6, "path": "pos2/backward.jpg", "group": "backward"}
  ]
}
```

### Memory and Performance Issues

#### Error: Out of Memory

**Error message**:
```
[ERROR] Out of memory. Required: 24GB, Available: 16GB
```

**Cause**: System memory is insufficient for the current task.

**Solution**:

1. **Lower resolution level**:
```json
{
  "resolution_level": 3  // Use low precision
}
```

2. **Limit memory usage**:
```json
{
  "min_avali_memory_size": 12.0  // Limit to 12GB
}
```

3. **Reduce output formats**:
```json
{
  "generate_obj": true,
  "generate_osgb": false,  // Disable unnecessary outputs
  "generate_3d_tiles": false
}
```

#### Error: GPU Out of Memory

**Error message**:
```
[ERROR] CUDA out of memory. Tried to allocate 2.00 GiB
```

**Cause**: Insufficient GPU VRAM.

**Solution**:
1. Close other GPU-intensive applications
2. Lower `resolution_level`
3. Use tiled processing (large projects)

### License Issues

#### Error: Invalid License

**Error message**:
```
[ERROR] Invalid license: License has expired
```

**Solution**:
1. Check license status: `license_engine.exe --enum ./license.json`
2. Confirm system time is correct
3. Reactivate the license
4. Contact technical support to renew the license

#### Error: Virbox Service Not Running

**Error message**:
```
[ERROR] Virbox service not running
```

**Solution**:

**Windows**:
```powershell
# Start service
net start "Virbox Service"

# Check service status
sc query "Virbox Service"
```

**Linux**:
```bash
# Load driver
sudo modprobe virbox

# Check driver
lsmod | grep virbox
```

### Output Issues

#### Issue: Holes in the Model

**Possible causes**:
- Insufficient image overlap
- Low-texture areas (water, glass, etc.)
- Poor image quality

**Solution**:
1. Increase overlap to 80%
2. Add oblique imagery for side coverage
3. Avoid shooting in harsh light or deep shadow
4. Review and remove blurry images

#### Issue: Blurry Texture

**Possible causes**:
- Blurry source images
- Flight altitude too high
- Incorrect camera settings

**Solution**:
1. Use a faster shutter speed (≥1/1000s)
2. Lower flight altitude
3. Use a higher-resolution camera
4. Ensure correct focus

#### Issue: Coordinate Offset

**Possible causes**:
- Wrong coordinate system settings
- Poor GPS accuracy

**Solution**:

1. **Check coordinate system configuration**:
```json
{
  "coordinate_system": {
    "type": 2,
    "label": "WGS 84",
    "epsg_code": 4326
  }
}
```

### Advanced Diagnostics

#### View Detailed Logs

Log file location: `working_dir/logs/log.txt`

Find key information:
```bash
# View errors
grep -i "error" log.txt

# View warnings
grep -i "warning" log.txt

# View progress
grep "PROGRESS" log.txt
```

#### Performance Analysis

Look for performance metrics in logs:
- `[TIMING]` — time per stage
- `[MEMORY]` — memory usage
- `[GPU]` — GPU utilization

#### Intermediate Result Checks

Inspect intermediate results to locate issues:
1. **AT results**: `working_dir/AT/mvs.xml`
2. **ROI extent**: `working_dir/milestones/roi.json`
3. **Quality report**: `working_dir/report/report.json`

### Getting Help

If the solutions above do not resolve your issue:

1. **Collect diagnostic information**:
   - Complete `log.txt`
   - Configuration file (sanitized)
   - System information (hardware configuration)

2. **Submit an issue**:
   - GitHub Issues: [project URL]
   - Technical support: support@mipmap.com
   - Include the diagnostic information above

3. **Check for updates**:
   - Look for a newer version
   - Review release notes
   - Check the known issues list

---

*This page is updated regularly. If your issue is not listed, contact technical support.*
