Batch Processing Multiple Videos with PySceneDetect and ffmpeg
==============================================================

Overview
--------

When working with large collections of video files, processing each one manually can be time-consuming.  
PySceneDetect combined with **ffmpeg** makes it possible to automate workflows across entire directories of videos.

In this guide, you will learn how to:

- Automate scene detection for multiple files.
- Use Python scripts to detect and split scenes in bulk.
- Organize outputs into structured directories.

Command-Line Workflow
---------------------

PySceneDetect can be run in a loop over multiple files.  
Example (Linux bash):

.. code-block:: bash

   for file in *.mp4; do
       scenedetect -i "$file" detect-content split-video \
           --output "output/${file%.*}/" \
           --filename-format "${file%.*}-Scene-${SCENE_NUMBER:03d}.mp4"
   done

Explanation:

- `*.mp4` processes all MP4 files in the current folder.
- Each video is split into its own subdirectory inside `output/`.
- Filenames are numbered sequentially per scene.

Python Workflow
---------------

You can automate multiple video splits with Python:

.. code-block:: python

   import os
   from scenedetect import detect, ContentDetector, split_video_ffmpeg

   input_dir = "videos"
   output_dir = "output"

   for filename in os.listdir(input_dir):
       if filename.endswith(".mp4"):
           filepath = os.path.join(input_dir, filename)
           print(f"Processing {filepath}...")

           # Detect scenes
           scene_list = detect(filepath, ContentDetector())

           # Define per-video output folder
           video_name = os.path.splitext(filename)[0]
           video_output = os.path.join(output_dir, video_name)
           os.makedirs(video_output, exist_ok=True)

           # Split scenes using ffmpeg
           split_video_ffmpeg(
               filepath,
               scene_list,
               output_file_template=os.path.join(video_output, f"{video_name}-scene-{{scene_number:03d}}.mp4")
           )

Organizing Outputs
------------------

A recommended structure for batch outputs is:

.. code-block::

   output/
   ├── video1/
   │   ├── video1-scene-001.mp4
   │   ├── video1-scene-002.mp4
   │   └── ...
   ├── video2/
   │   ├── video2-scene-001.mp4
   │   ├── video2-scene-002.mp4
   │   └── ...
   └── video3/
       ├── video3-scene-001.mp4
       ├── video3-scene-002.mp4
       └── ...

This keeps clips organized per original video, making it easy to browse or import into editors.

Tips & Best Practices
---------------------

- Use `--filename-format` to customize filenames (include timestamps if needed).
- Store logs or CSV exports alongside each video for reference.
- Consider using `AdaptiveDetector` for complex footage with variable scene changes.

Use Cases
---------

- Processing large archives of surveillance or lecture videos.
- Preparing movie datasets for research or machine learning.
- Automatically cutting raw footage into smaller, manageable clips.

Resources
---------

- PySceneDetect CLI Guide: https://pyscenedetect.readthedocs.io/en/latest/cli/
- Python API Reference: https://pyscenedetect.readthedocs.io/en/latest/reference/
- ffmpeg Documentation: https://ffmpeg.org/documentation.html
