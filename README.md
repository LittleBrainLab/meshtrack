## meshtrack

**`meshtrack`** is a Python package designed for diffusion MRI tractography guided by the geometry of a 3D mesh. It provides tools for seed generation, tracking, and filtering of tractograms based on mesh surfaces. With a focus on usability and flexibility, `meshtrack` helps researchers leverage surface-based geometry to enhance tractography analysis. Note this repository was designed for work the analysis performed in Little et al., HBM 2025 and the functionality will eventually be incorporated into scilpy (https://github.com/scilus/scilpy) when surfaces/meshes are fully supported.

### Features

- **Mesh-to-Seeds Conversion**: Convert 3D meshes into coordinates and normals for tractography seeding.
- **Mesh-Based Tractography**: Perform tractography using extracted seeds and mesh geometry.
- **Tractogram Filtering**: Filter tractograms based on their interaction with input meshes.

---

### Usage Workflow

The workflow for using `meshtrack` involves three primary steps, executed via the provided scripts:

#### 1. Convert Mesh to Coordinates and Normals
Use `meshtrack_surface_convert_to_coordinates.py` to extract seeding coordinates and normals from a `.vtk` mesh file.

```bash
python meshtrack_surface_convert_to_coordinates.py surf.ply coords.txt norms.txt
```

- **Inputs**: 
  - `surf.ply`: Input 3D mesh in `.ply` format.
- **Outputs**:
  - `coords.txt`: Text file with vertex coordinates.
  - `norms.txt`: Text file with vertex normals.

#### 2. Perform Tractography
Use `meshtrack_tracking_based_on_surface.py` to perform tractography based on extracted seeds.

```bash
python meshtrack_tracking_based_on_surface.py in_odf.nii.gz coords.txt mask.nii.gz out_tractogram.trk --in_norm_list norms.txt
```

- **Inputs**:
  - `in_odf.nii.gz`: Spherical harmonics representation of the ODF.
  - `coords.txt`: Seeding coordinates from the previous step.
  - `mask.nii.gz`: Mask for constraining tracking.
- **Outputs**:
  - `out_tractogram.trk`: Generated tractogram.

#### 3. Filter Tractogram by Mesh Geometry
Use `meshtrack_tractogram_filter_by_surface.py` to filter tractograms based on mesh geometry.

```bash
python meshtrack_tractogram_filter_by_surface.py in_tractogram.trk out_tractogram_filtered.trk --mesh_roi surf.ply both_ends include
```

- **Inputs**:
  - `in_tractogram.trk`: Input tractogram.
  - `surf.ply`: 3D mesh for filtering.
- **Outputs**:
  - `out_tractogram_filtered.trk`: Filtered tractogram.

---

### Example Workflow

Here’s an example workflow to guide you:

1. Convert a 3D mesh file (`surf.ply`) to seeding coordinates and normals:

   ```bash
   python meshtrack_surface_convert_to_coordinates.py surf.ply coords.txt norms.txt
   ```

2. Perform tractography using the generated coordinates (`coords.txt`) and normals (`norms.txt`):

   ```bash
   python meshtrack_tracking_based_on_surface.py fod.nii.gz coords.txt mask.nii.gz tractogram.trk --in_norm_list norms.txt
   ```

3. Filter the resulting tractogram (`tractogram.trk`) to retain streamlines passing through a mesh region:

   ```bash
   python meshtrack_tractogram_filter_by_surface.py tractogram.trk filtered_tractogram.trk --mesh_roi region.ply both_ends include
   ```

---

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/username/meshtrack.git
   ```

2. Get dependencies and install:

   ```bash
   pip install -r requirements.txt
   pip install -e .
   ```

---
