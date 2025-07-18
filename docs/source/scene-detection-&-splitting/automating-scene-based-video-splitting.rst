Automating Scene-Based Video Splitting with ffmpeg and PySceneDetect
=====================================================================

Overview
--------

This guide shows how to automate the process of detecting scene boundaries in videos and splitting them into separate files using **PySceneDetect** and external tools like **ffmpeg** or **mkvmerge**.

Whether you're preparing data for machine learning, isolating action sequences, or cutting out commercials, PySceneDetect can handle detection while ffmpeg/mkvmerge takes care of the actual splitting.

Tools Covered:

- PySceneDetect (scene detection)
- ffmpeg (splitting, transcoding)
- mkvmerge (alternative splitter for `.mkv` files)

CLI Workflow
------------

**Step 1: Detect Scenes and Export Timecodes**

Use the `scenedetect` CLI to analyze the video and generate split points:

.. code-block:: bash

   scenedetect -i input.mp4 detect-content list-scenes

To export timecodes in CSV format:

.. code-block:: bash

   scenedetect -i input.mp4 detect-content export-scenes --filename-format "{VIDEO_NAME}-Scene-{SCENE_NUMBER:03d}" --output scenes.csv

**Step 2: Split Video Using ffmpeg**

Use a simple loop or batch script with the timecodes to split the video using ffmpeg:

Example (Linux bash script):

.. code-block:: bash

   ffmpeg -i input.mp4 -ss 00:00:00.000 -to 00:00:12.345 -c copy scene-001.mp4

You can automate this using the CSV timecodes from PySceneDetect.

Python Workflow
---------------

**Step 1: Detect and Split Automatically**

Use PySceneDetect’s Python API with `split_video_ffmpeg`:

.. code-block:: python

   from scenedetect import detect, ContentDetector, split_video_ffmpeg

   scenes = detect('input.mp4', ContentDetector())
   split_video_ffmpeg('input.mp4', scenes, output_file_template='output/scene-${scene_number:03d}.mp4')

This command will:
- Detect cuts using `ContentDetector`
- Split the input into separate clips using `ffmpeg`
- Save them in the `output/` directory with numbered filenames

**Step 2 (Optional): Use mkvmerge**

If you prefer `mkvmerge`, export the timecodes as `.mkvmerge`-compatible format using:

.. code-block:: bash

   scenedetect -i input.mp4 detect-content export-scenes --format mkvmerge

Then split using `mkvmerge`:

.. code-block:: bash

   mkvmerge -o output.mkv --split parts:00:00:00-00:00:12,00:00:12-00:00:34 input.mp4

File Naming and Organization
----------------------------

Use `--filename-format` in CLI or `output_file_template` in Python to customize filenames:

.. code-block:: bash

   scenedetect -i input.mp4 detect-content split-video --filename-format "scene-{SCENE_NUMBER:03d}.mp4"

You can include:
- `{VIDEO_NAME}`
- `{SCENE_NUMBER}`
- `{START_TIME}`, `{END_TIME}`

Split files can be saved in a subfolder:

.. code-block:: bash

   --output output_dir/

Use Cases
---------

**1. Remove Commercials from TV Recordings**

- Detect commercial breaks using `detect-content` or `detect-hist`
- Export only non-commercial scenes (manually or with logic)

**2. Isolate Action or Dialog Scenes**

- Use `detect-adaptive` to separate fast-paced segments
- Combine results with a video tagger or transcript parser

**3. Prepare Clips for Training AI Models**

- Break long footage into uniform, scene-based segments
- Export frames from each scene if needed (`save-images`)

Summary
-------

Automated video splitting with PySceneDetect and ffmpeg or mkvmerge enables:

- Efficient video preprocessing
- Scene-based organization
- Targeted editing workflows

Resources
---------

- PySceneDetect Docs: https://pyscenedetect.readthedocs.io/
- ffmpeg: https://ffmpeg.org/
- mkvmerge: https://mkvtoolnix.download/
- Python API Reference: https://pyscenedetect.readthedocs.io/en/latest/reference/

Related Topics
--------------

- :doc:`comparing-algorithms`
- :doc:`exporting-scenes`
