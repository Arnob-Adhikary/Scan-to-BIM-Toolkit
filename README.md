# Scan-to-BIM-Toolkit
## 📖 Introduction

This repository contains a collection of Dynamo scripts developed to automate and streamline challenging **Scan-to-BIM** workflows in Autodesk Revit. The toolkit addresses common bottlenecks in converting raw point cloud data into intelligent BIM elements, with a focus on terrain modeling and handling complex, non-standard geometries that are difficult to model using traditional methods.

This project showcases practical solutions to real-world problems faced in digital construction and asset management, demonstrating the power of computational design in the AEC industry.
***

## ✨ Features & Scripts

This toolkit includes four primary scripts, each designed to solve a specific problem in the Scan-to-BIM process.

| Script Name       | Description                                                                                              |
| ----------------- | -------------------------------------------------------------------------------------------------------- |
| `Point-to-Topo`   | Creates a native Revit topography surface directly from a pre-processed point cloud (`.csv` file).       |
| `Floor-into-Topo` | Conforms or "projects" a Revit floor element onto an underlying topography surface, ideal for hardscapes.  |
| `Mass_to_Family`  | Converts any solid geometry from an in-place or conceptual mass into a loadable Generic Model family.     |
| `Loft Funnel`     | A specialized script to model highly irregular, lofted shapes, developed for a unique project challenge.   |
***

## 🛠️ Workflows & Usage

### General Installation

1.  **Clone Repository:** Download or clone this repository to your local machine.
2.  **Dynamo Packages:** Ensure you have the necessary Dynamo packages installed. This toolkit may require packages like `Clockwork`, `Spring Nodes`, or `LunchBox`.
3.  **Open in Dynamo:** Launch Revit, open a project file, and start Dynamo. Navigate to the script you wish to use.
### 1. Point-to-Topo

This script requires a significant pre-processing workflow to ensure the input data is clean. It is designed to work with a `.csv` file containing only ground surface points.

#### **Pre-processing Pipeline:**

1.  **Export from ReCap:** Start with your `.rcp` point cloud and export it to an `.e57` file format using Autodesk ReCap.
2.  **Convert to LAS:** Use a data interoperability tool like **FME Desktop** to convert the `.e57` file into a `.las` file.
3.  **Clean Point Cloud:** In a dedicated point cloud software like **Terrasolid** (running on MicroStation), classify the point cloud to isolate the ground points and remove noise, vegetation, and buildings.
4.  **Extract Key Points:** Generate model key points from the classified ground surface. This thins the data to a manageable size while preserving accuracy.
5.  **Save as CSV:** Export the final ground points as a `.csv` file. The file must be formatted with only three columns: **X, Y, Z** (without headers).

#### **Script Usage:**

1.  Open the `Point-to-Topo.dyn` script in Dynamo.
2.  Provide the file path to your prepared `.csv` file.
3.  Click "Run". The script will generate a topography surface in your Revit project.

---

### 2. Floor-into-Topo

This script is useful for modeling hardscapes like roads, plazas, or pathways that follow the site's terrain.

#### **Script Usage:**

1.  In your Revit project, model a `Floor` element positioned above the target topography. The shape of the floor should represent the hardscape area.
2.  Open the `Floor-into-Topo.dyn` script.
3.  Use "Select Model Element" nodes to select the floor and the topography.
4.  Click "Run". The script will add points to the floor's sub-element editor, matching its shape to the topography below it.

---

### 3. Mass\_to\_Family

This script bridges the gap between flexible mass modeling and standard family-based workflows, perfect for complex building elements discovered during a scan.

#### **Script Usage:**

1.  Model your complex geometry as a **solid** within a Revit in-place mass or a conceptual mass family (`.rfa`).
2.  Open the `Mass_to_Family.dyn` script.
3.  Select the solid mass geometry.
4.  Provide a name and a save location for the new family file that will be created.
5.  Click "Run". The script will generate a new Generic Model family (`.rfa`) containing your geometry and load it back into the project.

---

### 💡 Case Study: The `Loft Funnel` Script

The `Loft Funnel` script is a testament to adaptive problem-solving. During a project involving large industrial funnels, the geometry proved too irregular and complex for the `Mass_to_Family` script to process reliably. The surfaces were non-planar and twisted in ways that caused the solid-to-family conversion to fail.

To overcome this, the `Loft Funnel.dyn` script was created. It uses a different approach:
* It takes a series of profile curves (circles or ellipses) at different heights.
* It uses a lofting function to generate a surface that passes through these profiles.
* It then thickens the surface to create a solid form.

This script demonstrates a deeper, more tailored approach to computational modeling when general-purpose tools are insufficient.

***
