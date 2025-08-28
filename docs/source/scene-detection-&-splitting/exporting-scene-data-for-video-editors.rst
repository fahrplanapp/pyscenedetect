Exporting Scene Data for Video Editors: OTIO, EDL, and CSV
==========================================================

Overview
--------

PySceneDetect allows you to export detected scene boundaries into various formats that can be imported into professional video editors.  
This enables a seamless workflow: **Detect → Export → Import → Edit**.

Supported formats include:

- **CSV (Comma-Separated Values):** Easy to read and script, works for custom pipelines.
- **EDL (Edit Decision List):** Widely supported in editing software.
- **OTIO (OpenTimelineIO):** Open standard for timeline exchange, supported by tools like DaVinci Resolve.
- **HTML/QP files:** For reports and x264 workflows.

This guide shows how to export and use scene data across different editors.

CLI Export Workflow
-------------------

**Step 1: Detect Scenes**

.. code-block:: bash

   scenedetect -i input.mp4 detect-content list-scenes

**Step 2: Export to CSV**

.. code-block:: bash

   scenedetect -i input.mp4 detect-content export-scenes --format csv --output input_scenes.csv

**Step 3: Export to EDL**

.. code-block:: bash

   scenedetect -i input.mp4 detect-content export-scenes --format edl --output input_scenes.edl

**Step 4: Export to OTIO**

.. code-block:: bash

   scenedetect -i input.mp4 detect-content export-scenes --format otio --output input_scenes.otio

Python Export Workflow
----------------------

Use the API to export scenes directly from a Python script:

.. code-block:: python

   from scenedetect import detect, ContentDetector, SceneManager, open_video, StatsManager
   from scenedetect.export import export_csv, export_edl, export_otio

   video_path = "input.mp4"
   scene_list = detect(video_path, ContentDetector())

   # Export CSV
   export_csv(scene_list, "input_scenes.csv", video_path)

   # Export EDL
   export_edl(scene_list, "input_scenes.edl", video_path)

   # Export OTIO
   export_otio(scene_list, "input_scenes.otio", video_path)

Importing into Video Editors
----------------------------

**1. DaVinci Resolve (OTIO or EDL)**

- Import the `.otio` file via the OpenTimelineIO plugin.
- Alternatively, import `.edl` directly using Resolve's Media Pool.
- Each scene cut appears as a timeline marker or separate clip.

**2. Adobe Premiere Pro (EDL/CSV)**

- Import `.edl` directly via the "Import EDL" option.
- For CSV, use third-party scripts or plugins to translate timecodes into markers.

**3. Final Cut Pro (EDL/CSV via XML conversion)**

- Convert `.edl` or `.csv` into Final Cut XML using OTIO converters or utilities.
- Import the XML file to view scene markers.

Workflow Example
----------------

1. **Detect:** Run scene detection with PySceneDetect.
2. **Export:** Save results as `.otio` or `.edl`.
3. **Import:** Bring the exported file into your editor.
4. **Edit:** Use markers to trim, cut, or annotate scenes.

Example CLI:

.. code-block:: bash

   scenedetect -i movie.mp4 detect-adaptive export-scenes --format otio --output movie_scenes.otio

Example Timeline:

- Scene 1: 00:00:00.000 – 00:00:10.500  
- Scene 2: 00:00:10.500 – 00:00:27.800  
- Scene 3: 00:00:27.800 – 00:01:02.200  

This information is preserved when imported into editors.

Best Practices
--------------

- Always verify timecode alignment between exported files and your editor’s frame rate.
- For large projects, CSV is best for custom workflows, while OTIO/EDL is best for direct editor import.
- Keep exported files alongside raw footage for reference.

Resources
---------

- PySceneDetect Export Guide: https://pyscenedetect.readthedocs.io/en/latest/examples/usage/#export-scenes
- OpenTimelineIO: https://opentimeline.io/
- DaVinci Resolve OTIO Integration: https://github.com/PixarAnimationStudios/OpenTimelineIO
