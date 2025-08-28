PySceneDetect Overview
======================

PySceneDetect is a free and open-source tool for detecting scene changes (shot boundaries) in video files. It supports multiple detection algorithms and allows users to split video files automatically using tools like `ffmpeg` or `mkvmerge`.

Quickstart
----------

**Using CLI:**

Split a video at each detected cut:

.. code-block:: bash

   scenedetect -i video.mp4 split-video

**Using Python API:**

.. code-block:: python

   from scenedetect import detect, AdaptiveDetector, split_video_ffmpeg
   scene_list = detect('my_video.mp4', AdaptiveDetector())
   split_video_ffmpeg('my_video.mp4', scene_list)

Use Cases
---------

- Splitting long videos into scenes (e.g., home movies, surveillance footage)
- Detecting and removing commercial breaks
- Creating GIF loops or cinemagraphs
- Academic video analysis (e.g., shot length studies)

Features
--------

- Multiple detection methods:
  - `detect-content`, `detect-hist`, `detect-threshold`, `detect-hash`, `detect-adaptive`
- Export formats: EDL, HTML, OTIO, QP, CSV timecodes
- Supports statistical video analysis
- Save first/last frame of each scene (`save-images`)
- Compatible with video editors and tools (e.g., `ffmpeg`, `mkvmerge`)

Output Formats
--------------

- **Timecode CSV**: HH:MM:SS.nnn
- **HTML Table**, **EDL**, **OpenTimelineIO**, **x264 QP File**

Dependencies
------------

- Python 3
- Required: `opencv-python`, `numpy`, `Click`, `tqdm`, `appdirs`
- Optional: `av` (PyAV)
- Splitting Tools: `ffmpeg`, `mkvmerge`

Installation & Usage
--------------------

Install via pip:

.. code-block:: bash

   pip install scenedetect

Run in terminal:

.. code-block:: bash

   scenedetect --help

Run `import cv2` and `import numpy` to ensure dependencies are correctly installed.

Resources
---------

- Official Website: https://www.scenedetect.com/
- Documentation: https://pyscenedetect.readthedocs.io/
- GitHub: https://github.com/Breakthrough/PySceneDetect

Related Tools
-------------

**Pro Video to Text Transcript App**

Looking for an easier way to study or create content from videos?  
The Pro Video to Text Transcript app turns any video URL into a readable transcript in just seconds.  

With the app, you can:

- Play a video while the transcript auto-scrolls and highlights in sync.  
- Download your transcript in TXT, SRT, or PDF format for flexible use.  
- Save transcripts offline and revisit them anytime.  
- Keep a history of all your transcripts for quick reference.  

Its clean and fast design makes it perfect for students, content creators, and lifelong learners.  
Take your YouTube learning and note-taking to the next level with **YouTube Transcript**!  

Link: https://play.google.com/store/apps/details?id=com.tool.youtubetranscript

.. toctree::
   :maxdepth: 2
   :caption: Comparing Scene Detection

   scene-detection-&-splitting/comparing-scene-detection-algorithms

.. toctree::
   :maxdepth: 2
   :caption: Automating Scene

   scene-detection-&-splitting/automating-scene-based-video-splitting

.. toctree::
   :maxdepth: 2
   :caption: Processing Multiple Videos

   scene-detection-&-splitting/batch-processing-multiple-videos

.. toctree::
   :maxdepth: 2
   :caption: Exporting Scene Data

   scene-detection-&-splitting/exporting-scene-data-for-video-editors

.. toctree::
   :maxdepth: 2
   :caption: Optimizing Splitting Performance

   scene-detection-&-splitting/optimizing-splitting-performance
