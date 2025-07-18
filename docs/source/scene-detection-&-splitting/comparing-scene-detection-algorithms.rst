Comparing Scene Detection Algorithms in PySceneDetect: Which One Should You Use?
=================================================================================

Overview
--------

PySceneDetect provides multiple algorithms to detect scene changes in videos, each designed for different needs and scenarios. Choosing the right one can significantly improve your video processing workflow, accuracy, and performance.

This guide explores five key detection methods available in PySceneDetect:

- **detect-content**: Default method using frame-wise content difference (ideal for general-purpose use).
- **detect-hist**: Based on histogram changes between frames (fast but less precise).
- **detect-threshold**: Uses a fixed threshold (requires manual tuning).
- **detect-hash**: Compares perceptual hashes (good for duplicate/near-duplicate frames).
- **detect-adaptive**: Smart algorithm using local video statistics (adaptive and accurate).

Detection Methods
-----------------

**1. detect-content**

- **Description**: Compares pixel changes between frames to identify cuts.
- **Use Case**: Balanced accuracy and performance for most video types.
- **Strengths**: General-purpose, no manual tuning needed.
- **Example**:

  .. code-block:: python

     from scenedetect import detect, ContentDetector
     scenes = detect('video.mp4', ContentDetector())

**2. detect-hist**

- **Description**: Compares color histograms over time.
- **Use Case**: Fast scanning, surveillance or low-motion videos.
- **Strengths**: Lightweight; fast processing.
- **Limitations**: Misses subtle scene changes.
- **Example**:

  .. code-block:: python

     from scenedetect import detect, HistogramDetector
     scenes = detect('video.mp4', HistogramDetector())

**3. detect-threshold**

- **Description**: Detects scenes where the difference exceeds a defined threshold.
- **Use Case**: Fine control when scene boundaries are highly predictable.
- **Strengths**: Highly tunable.
- **Limitations**: Requires trial and error for threshold values.
- **Example**:

  .. code-block:: python

     from scenedetect import detect, ThresholdDetector
     scenes = detect('video.mp4', ThresholdDetector(threshold=12.0))

**4. detect-hash**

- **Description**: Uses perceptual hashing to find scene changes.
- **Use Case**: Detecting repeated or similar content (e.g., logos, intros).
- **Strengths**: Useful for near-duplicate frames.
- **Limitations**: Less sensitive to subtle visual changes.
- **Example**:

  .. code-block:: python

     from scenedetect import detect, PerceptualHashDetector
     scenes = detect('video.mp4', PerceptualHashDetector())

**5. detect-adaptive**

- **Description**: Automatically adjusts detection sensitivity based on frame variance.
- **Use Case**: Ideal for videos with variable pacing and lighting.
- **Strengths**: AI-inspired; handles complex cuts well.
- **Limitations**: Slightly slower due to adaptive logic.
- **Example**:

  .. code-block:: python

     from scenedetect import detect, AdaptiveDetector
     scenes = detect('video.mp4', AdaptiveDetector())

Benchmarking Example
--------------------

You can compare these algorithms by applying each to the same video and counting the detected scenes:

.. code-block:: python

   from scenedetect import detect, ContentDetector, HistogramDetector, AdaptiveDetector

   video_path = 'my_video.mp4'

   content_scenes = detect(video_path, ContentDetector())
   hist_scenes = detect(video_path, HistogramDetector())
   adaptive_scenes = detect(video_path, AdaptiveDetector())

   print(f"Content scenes: {len(content_scenes)}")
   print(f"Histogram scenes: {len(hist_scenes)}")
   print(f"Adaptive scenes: {len(adaptive_scenes)}")

Visual Output
-------------

To save and review visual outputs:

.. code-block:: python

   from scenedetect import save_images

   save_images('my_video.mp4', content_scenes, output_dir='scenes/content')
   save_images('my_video.mp4', adaptive_scenes, output_dir='scenes/adaptive')

This will extract the first and last frame of each detected scene to image files, allowing visual comparison.

Summary Table
-------------

+------------------+-----------------------------+----------------------------+
| Method           | Best Use Case               | Notes                      |
+==================+=============================+============================+
| detect-content   | General-purpose detection   | Default and reliable       |
+------------------+-----------------------------+----------------------------+
| detect-hist      | Fast processing             | Less accurate              |
+------------------+-----------------------------+----------------------------+
| detect-threshold | Manual tuning               | Good for predictable cuts  |
+------------------+-----------------------------+----------------------------+
| detect-hash      | Duplicate frame detection   | Low sensitivity to noise   |
+------------------+-----------------------------+----------------------------+
| detect-adaptive  | Variable or complex videos  | Smart and precise          |
+------------------+-----------------------------+----------------------------+

Further Reading
---------------

- Official Docs: https://pyscenedetect.readthedocs.io/
- PySceneDetect GitHub: https://github.com/Breakthrough/PySceneDetect
- Export Guide: :doc:`exporting-scenes`
