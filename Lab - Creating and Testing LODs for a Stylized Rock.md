# Lab – Making 3D Assets in a Game-Ready Production Pipeline Part 4 - Creating and Testing LODs

*In this final lab, we will create a full set of Level of Detail (LOD) meshes from our textured hero asset – a stylized rock. You’ll learn both manual and automated reduction techniques in Blender and then export and configure your LODs inside Unity for performance testing.*

![alt text](<LOD Lab Cover Image-1.png>)

---

### Introduction

In real-time applications like games, using LODs allows models to render at varying resolutions depending on their distance from the camera. This improves performance by reducing the complexity of geometry that’s far from the viewer.

In this lab, you will:

- Manually reduce a clean ~750 triangle retopo mesh
- Create 3 progressively simpler LODs (approx. 300, 100, and 30 triangles)
- Explore both **manual reduction** and **automated methods** (Decimate, Remesh, Shrinkwrap)
- Export a single .FBX with all LODs
- Import and configure the model in Unity using Unity's LOD Group component

---

### Core Concepts

#### **1. What are LODs?**
LODs are simplified versions of a mesh that display based on camera distance, helping optimize game performance.

#### **2. Clean Topology First**
Our base mesh is already retopologized cleanly (~750 tris). This gives us full control for both manual loop reductions and clean decimations.

#### **3. Consistent UVs and Baking**
We won’t rebake textures. LODs should preserve UVs and normals from the original. All LODs will share the same material and maps.

---

### Section 1 – Manual Loop Reduction (Creating LOD1 ~300 tris)

#### **Step 1: Duplicate the Base Mesh**
- In **Blender**, select your final textured mesh.
- **Shift + D** to duplicate.
- Rename the duplicate: `rock_LOD1`.

#### **Step 2: Enable Triangle Count Display**
- In the **Overlay** menu, enable **Statistics** to view triangle counts live.

#### **Step 3: Enter Edit Mode**
- Go to **Edge Select Mode** (`2` key).
- Begin selecting **edge loops** that contribute little to the silhouette.
- Press **X** > **Dissolve Edges** to simplify without affecting UVs.

> *Try to preserve the overall shape and UV islands.*

---

### Section 2 – Creating LOD2 (~100 tris) Using Automated Tools

#### **Step 1: Duplicate LOD1**
- Select `rock_LOD1`, **Shift + D**, rename to `rock_LOD2`.

#### **Step 2: Apply Decimate Modifier**
- Go to **Modifiers Panel** > **Add Modifier** > **Decimate**.
- Set **Ratio** to around **0.33**.
- Click **Apply** once satisfied.

#### **Step 3: Clean Up**
- Inspect the result. Use **Merge by Distance** or minor manual edits if needed.

---

### Section 3 – Creating LOD3 (~30 tris) with Shrinkwrap & Remesh

#### **Step 1: Duplicate LOD2**
- Select `rock_LOD2`, **Shift + D**, rename to `rock_LOD3`.

#### **Step 2: Use Remesh Modifier**
- Add **Remesh Modifier**.
- Set **Mode** to **Blocks** or **Smooth**.
- Lower **Voxel Size** until reaching around **30 triangles**.
- Apply modifier.

#### **Step 3: Restore Shape with Shrinkwrap**
- Add **Shrinkwrap Modifier**.
- Set **Target** to the **LOD0 (hero rock)**.
- Apply Shrinkwrap to pull distorted LOD3 mesh back onto the original shape.

> *Optional: Apply smoothing with a **Smooth Modifier** if desired.*

---

### Section 4 – Exporting All LODs in One FBX

#### **Step 1: Arrange and Rename All LODs**
- Rename each mesh to:  
  - `rock_LOD0`  
  - `rock_LOD1`  
  - `rock_LOD2`  
  - `rock_LOD3`

#### **Step 2: Parent LODs to Empty (Optional but clean)**
- Create an **Empty** and parent all LODs to it for organized export.

#### **Step 3: Export FBX**
- Select all LODs.
- File > Export > **FBX (.fbx)**
- Set options:
  - ✔ Selected Objects
  - ✔ Apply Transform
  - Path Mode: Copy, ✔ Embed Textures (if needed)
- Name your file `rock_LODs.fbx`

---

### Section 5 – Import and Set Up LOD Group in Unity

#### **Step 1: Import into Unity**
- Drag your `rock_LODs.fbx` into the Unity **Assets** folder.

#### **Step 2: Drag into Scene**
- Unity will import all LOD meshes as children of the FBX object.

#### **Step 3: Add LOD Group**
- Select your imported prefab.
- In the **Inspector**, click **Add Component** > **LOD Group**.

#### **Step 4: Configure LOD Group**
- Add 4 LOD slots in the **LOD Group**.
- Assign each mesh:
  - LOD0: `rock_LOD0`
  - LOD1: `rock_LOD1`
  - LOD2: `rock_LOD2`
  - LOD3: `rock_LOD3`
- Adjust the **sliders** to control when each LOD appears.

> *Preview the LOD transitions by moving the Scene View camera closer/farther from the object.*

---

### Final Checklist

✅ Created LODs at ~300, 100, and 30 triangles  
✅ Used both manual and automated reduction tools  
✅ Exported a multi-object FBX  
✅ Set up and tested LODs in Unity using LOD Group  
