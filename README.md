# End-to-End_AI_for_Bone_Marrow_Smear_Based_Leukemia_Diagnostics
Project with Helmholz and TUM as a part of the Computational Method for Single Cell Biology

## Downloading the data

The DICOM data is not stored in this repository. Download the image and
annotation series from the original **Bone Marrow WSI – Pediatric Leukemia**
dataset using the identifiers below, and place the series under
`_data/bonemarrowwsi_pediatricleukemia/`.

| Identifier | Value |
| --- | --- |
| Patient ID | `002BD524C907A2A5121D3ADAED05D442` |
| StudyInstanceUID | `1.2.826.0.1.3680043.8.498.72426040555675237648887893204163818289` |
| WSI SeriesInstanceUID | `1.2.826.0.1.3680043.8.498.56046203939823473875129608016149951794` |
| Annotation SeriesInstanceUID | `1.2.826.0.1.3680043.10.511.3.665340093413205649801019945659048` |

The main image instance is named
`01024348-5507-4e07-a80b-b7fcd1b39bd2.dcm`. The SeriesInstanceUID is the
preferred identifier for retrieving the exact image and annotation series; the
Patient ID alone may refer to multiple studies or slides.

## Viewing whole-slide images with napari

The napari viewer is an optional tool for inspecting whole-slide images (WSI). It
is not required for linting or for installing the core project.

From the repository root, install napari, the WSI reader plugin, all reader
backends, and the OpenSlide runtime into the existing virtual environment:

```powershell
.\.venv\Scripts\python.exe -m pip install "napari[pyqt6]>=0.10,<0.11" "napari-wsi[all]>=1.3,<2" openslide-bin
```

Launch the viewer without activating the environment:

```powershell
.\.venv\Scripts\napari.exe
```

In napari, open **Plugins > WSI Reader**, select the backend matching the image
format, choose the slide path, and select **Load**. Use the `wsidicom` backend
for DICOM WSI data and the `openslide` backend for formats such as `.svs`,
`.ndpi`, and `.scn`.

To confirm which napari installation is being used:

```powershell
.\.venv\Scripts\napari.exe --version
```

## Code quality

Install the development tools in the virtual environment:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements-dev.txt
```

Run the same checks used in CI:

```powershell
.\.venv\Scripts\python.exe -m ruff check .
.\.venv\Scripts\python.exe -m ruff format --check .
```

Apply safe lint fixes and formatting:

```powershell
.\.venv\Scripts\python.exe -m ruff check --fix .
.\.venv\Scripts\python.exe -m ruff format .
```

### Git hooks

The hook configuration is stored in `.pre-commit-config.yaml`, but Git hooks
themselves are local and are not included when cloning the repository. After a
fresh clone, install the development tools and register the hooks once:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements-dev.txt
.\.venv\Scripts\python.exe -m pre_commit install
```

Verify the installation by running every hook against the repository:

```powershell
.\.venv\Scripts\python.exe -m pre_commit run --all-files
```

The hooks now run automatically on `git commit`. They fix lint issues and format
staged Python files. If a hook changes a file, review it, stage it again with
`git add`, and repeat the commit. To remove the local hooks, run:

```powershell
.\.venv\Scripts\python.exe -m pre_commit uninstall
```

GitHub Actions independently checks every push and pull request without modifying
files.
