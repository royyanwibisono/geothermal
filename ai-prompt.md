# Blender MCP Task: Coordinate-Based Geothermal Facility Reconstruction

Use the attached reference photograph as the primary blueprint. Reconstruct the actual geothermal facility layout using the explicit coordinates and pipeline paths below.

DO NOT generate a random geothermal facility. DO NOT independently invent the pipeline arrangement.

## 1. Global Coordinate System

Configure Blender:

```python
scene.unit_settings.system = 'METRIC'
scene.unit_settings.length_unit = 'METERS'
scene.unit_settings.scale_length = 1.0
```

Facility dimensions:

```python
SITE_WIDTH = 60.0
SITE_LENGTH = 33.75
SITE_HEIGHT = 15.0

ORIGIN = (0.0, 0.0, 0.0)
```

Coordinate convention:

- X = left to right.
- Y = bottom to top.
- Z = vertical elevation.
- Origin = bottom-left corner of the reference photograph.

Reference image dimensions: 1600 × 900 pixels.

Use this conversion:

```python
world_x = pixel_x * 60.0 / 1600.0
world_y = (900.0 - pixel_y) * 33.75 / 900.0
```

Use the reference photograph as a temporary image plane in the XY plane, aligned with these coordinates.

The coordinates below are estimated from visual inspection. Treat them as the initial layout constraints and preserve their relative positions.

## 2. Equipment Coordinates

Create the following equipment at these approximate world coordinates.

| Equipment |    X |    Y | Approx. Z |
| --------- | ---: | ---: | --------: |
| Sep SS    |  3.8 | 12.4 |         0 |
| LHD-23    | 10.5 | 24.8 |         0 |
| LHD-5     | 17.3 | 22.4 |         0 |
| LHD-21    | 23.8 | 22.5 |         0 |
| LHD-19    | 29.3 | 23.2 |         0 |
| LHD-20    | 35.3 | 23.2 |         0 |
| Sep 5.1   | 41.5 | 26.5 |         0 |
| Sep 5.2   | 51.4 | 25.3 |         0 |

These coordinates represent equipment footprints, not vessel centerline elevations.

### Equipment dimensions

| Equipment          | Approximate dimensions    |
| ------------------ | ------------------------- |
| Sep SS             | 1.5m diameter × 7m height |
| LHD stations       | 3m × 3m equipment pads    |
| Sep 5.1            | 2m diameter × 10m height  |
| Sep 5.2            | 2m diameter × 10m height  |
| Horizontal vessels | 2m diameter × 5m length   |

Place the two large separators near the right side, with Sep 5.2 farther right than Sep 5.1.

## 3. Pipeline Routing Coordinates

Create pipelines using explicit 3D polylines.

Each coordinate represents a pipeline centerline waypoint:

```python
PIPELINE_ROUTES = {
    "MAIN_LOWER_HEADER": [
        (0.0, 3.4, 0.8),
        (10.0, 3.4, 0.8),
        (20.0, 3.4, 0.8),
        (30.0, 3.4, 0.8),
        (40.0, 3.4, 0.8),
        (50.0, 3.4, 0.8),
        (58.5, 3.4, 0.8),
    ],

    "CENTRAL_DISTRIBUTION_HEADER": [
        (8.0, 6.8, 0.7),
        (17.3, 6.8, 0.7),
        (24.0, 6.8, 0.7),
        (30.0, 6.8, 0.7),
        (36.0, 6.8, 0.7),
        (45.0, 6.8, 0.7),
        (53.0, 6.8, 0.7),
    ],

    "LHD_5_BRANCH": [
        (17.3, 22.4, 1.0),
        (17.3, 20.0, 1.0),
        (17.3, 15.0, 0.8),
        (17.3, 6.8, 0.8),
    ],

    "LHD_21_BRANCH": [
        (23.8, 22.5, 1.0),
        (23.8, 18.0, 1.0),
        (24.0, 12.0, 0.8),
        (24.0, 6.8, 0.8),
    ],

    "LHD_19_BRANCH": [
        (29.3, 23.2, 1.0),
        (29.3, 19.0, 1.0),
        (30.0, 12.0, 0.8),
        (30.0, 6.8, 0.8),
    ],

    "LHD_20_BRANCH": [
        (35.3, 23.2, 1.0),
        (35.3, 18.0, 1.0),
        (36.0, 12.0, 0.8),
        (36.0, 6.8, 0.8),
    ],

    "SEP_SS_BRANCH": [
        (3.8, 12.4, 2.0),
        (3.8, 9.0, 1.0),
        (3.8, 3.4, 0.8),
    ],

    "RIGHT_MAIN_HEADER": [
        (53.0, 6.8, 1.0),
        (57.0, 8.0, 1.0),
        (59.0, 12.0, 1.0),
        (59.0, 20.0, 1.0),
    ],
}
```

IMPORTANT:

These are initial reference-based centerline estimates, not verified process-flow connections. Use the photograph to refine each segment. In particular, do not assume that every LHD branch connects to the same header just because the coordinates are aligned.

## 4. Right-Side Separator Pipeline Network

This area is particularly important.

Reconstruct the complicated network surrounding Sep 5.1 and Sep 5.2.

Use these approximate manifold locations:

```python
SEPARATOR_MANIFOLD = {
    "lower_manifold": [
        (40.0, 15.0, 1.5),
        (43.0, 15.0, 1.5),
        (46.0, 15.0, 1.5),
        (49.0, 15.0, 1.5),
        (52.0, 15.0, 1.5),
    ],

    "separator_5_1_inlet": [
        (40.0, 15.0, 1.5),
        (40.0, 19.0, 2.0),
        (40.0, 24.0, 4.0),
        (41.5, 26.5, 5.0),
    ],

    "separator_5_2_inlet": [
        (49.0, 15.0, 1.5),
        (49.0, 19.0, 2.0),
        (50.0, 23.0, 4.0),
        (51.4, 25.3, 5.0),
    ],

    "parallel_upper_pipes": [
        (42.0, 15.0, 2.0),
        (45.0, 19.0, 3.0),
        (48.0, 22.0, 4.0),
        (51.0, 25.0, 4.0),
    ],
}
```

Treat these as spatial anchors for tracing the visible right-side network. Trace individual parallel pipes separately from the photograph rather than treating them as one combined pipeline.

The right side should contain:
- Two large vertical separators.
- Multiple parallel yellow insulated pipelines.
- Several vertical risers.
- Lower manifold with multiple individual pipe connections.
- Elevated connections and bends.
- Supporting pipe racks.
- Smaller connecting pipelines.

Do not simplify this section into a few straight pipes.

## 5. Pipe Dimensions

Use the following approximate diameters:

```python
PIPE_DIAMETERS = {
    "main_header": 0.80,
    "secondary": 0.50,
    "branch": 0.35,
    "utility": 0.15,
    "instrument": 0.05,
}
```

Apply these dimensions consistently.

For each pipe:
- Use curve geometry with bevel depth.
- Use smooth transitions.
- Add elbows at direction changes.
- Add flanges at equipment connections.
- Add reducers when diameter changes.
- Add T-junctions where branches actually exist.
- Add pipe supports every 3–5 meters where appropriate.

## 6. 2D Pipeline Map

Create a separate top-down map based on the same coordinates.

The map must use the same equipment and pipeline geometry as the 3D scene.

Create:
- Equipment footprints.
- Pipeline centerlines.
- Individual pipeline groups.
- Junction markers.
- Direction arrows where supported by the photograph.
- Equipment labels.
- Coordinate grid with 5m spacing.

Camera:

```python
CAM_PIPELINE_MAP:
    type = ORTHO
    location = (30.0, 16.875, 90.0)
    rotation = (0.0, 0.0, 0.0)
    ortho_scale = 68.0
```

Adjust camera orientation if needed to look directly downward.

## 7. Reference Camera

Create the aerial camera to reproduce the photograph's perspective.

The camera should:
- View the facility from the front/bottom side.
- Be elevated above the facility.
- Point toward the separator area.
- Show the entire pipeline network.
- Preserve the relative equipment positions.

Create `CAM_AERIAL_REFERENCE`.

## 8. Modeling and Validation

Build the scene in stages:

1. Create the 60m × 33.75m ground plane.
2. Place the reference photograph as an image plane.
3. Create equipment location markers using the coordinates above.
4. Trace and refine pipeline centerlines against the reference.
5. Build the main headers.
6. Build the individual branches.
7. Model Sep SS, Sep 5.1 and Sep 5.2.
8. Add LHD equipment stations.
9. Build the separator manifold.
10. Add pipe supports, structures, and details.
11. Apply materials.
12. Render and compare with the photograph.

Do not proceed to detailed equipment modeling until the overall top-down layout is reasonably aligned with the reference.

## 9. Final Deliverables

Save as:

`geothermal_facility.blend`

Deliver:
- Full editable 3D facility.
- Top-down pipeline map.
- Aerial reference camera render.
- Organized pipeline collections.
- Metric-scale geometry.
- Descriptive object names.

The most important success criterion is that the 3D facility reproduces the spatial arrangement and visible pipeline paths of the attached reference photograph.

If a connection is uncertain, mark it as uncertain instead of silently inventing a connection.