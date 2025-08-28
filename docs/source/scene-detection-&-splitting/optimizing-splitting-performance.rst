Optimizing Splitting Performance: ffmpeg Parameters and Compression Options
===========================================================================

Overview
--------

When splitting videos with **PySceneDetect**, the actual cutting is handled by **ffmpeg** (or mkvmerge).  
Performance, quality, and storage size depend heavily on the ffmpeg parameters you use.  

This guide covers key ffmpeg options to balance speed, quality, and storage requirements.

Core ffmpeg Settings
--------------------

**1. Stream Copy Mode (`-c copy`)**

- **Description:** Copies video/audio streams without re-encoding.
- **Pros:** Extremely fast, no quality loss.
- **Cons:** Split points may only occur at keyframes (cuts may not be frame-accurate).
- **Example:**

  .. code-block:: bash

     ffmpeg -i input.mp4 -ss 00:00:00 -to 00:00:12 -c copy output.mp4

**2. Re-encoding Mode**

- **Description:** Decodes and re-encodes video at split points.
- **Pros:** Accurate cuts (frame-level precision).
- **Cons:** Slower, quality may reduce slightly depending on codec and bitrate.
- **Example:**

  .. code-block:: bash

     ffmpeg -i input.mp4 -ss 00:00:00 -to 00:00:12 -c:v libx264 -crf 18 -preset fast output.mp4

**3. Bitrate Control**

- **Constant Rate Factor (CRF):** Balances quality and file size (lower CRF = higher quality).
- **Average Bitrate (ABR):** Fixed bitrate target (e.g., 2000k).
- **Two-Pass Encoding:** Best for archiving or predictable file sizes.

- Example (CRF, high quality, smaller size):

  .. code-block:: bash

     ffmpeg -i input.mp4 -ss 00:00:00 -to 00:00:12 -c:v libx264 -crf 23 -preset medium output.mp4

- Example (Bitrate control):

  .. code-block:: bash

     ffmpeg -i input.mp4 -ss 00:00:00 -to 00:00:12 -b:v 1500k output.mp4

Performance Considerations
--------------------------

- **Use `-c copy`** for fast splitting when exact frame accuracy is not required.
- **Use re-encoding** when precision is critical (e.g., editing or analysis).
- **Choose CRF values** between `18–28` for H.264:
  - `18–20`: Archival quality.
  - `22–24`: Good balance for general use.
  - `26–28`: Smaller preview files.

Use Cases
---------

**1. Archiving**

- Goal: Preserve maximum quality.
- Settings: `-c:v libx264 -crf 18 -preset slow`
- Larger file sizes but ideal for long-term storage.

**2. Lightweight Preview Files**

- Goal: Small, quick-to-share clips.
- Settings: `-c:v libx264 -crf 26 -preset veryfast`
- Fast export, small size, lower quality.

**3. Editing Pipelines**

- Goal: Frame-accurate cuts and compatible codecs.
- Settings: Re-encode with intra-frame codecs (`-c:v prores` or `-c:v dnxhd`).
- Ensures smooth editing in NLE software.

Integration with PySceneDetect
------------------------------

When using `split_video_ffmpeg` in Python, you can pass ffmpeg options via parameters:

.. code-block:: python

   from scenedetect import detect, ContentDetector, split_video_ffmpeg

   scenes = detect("input.mp4", ContentDetector())
   split_video_ffmpeg(
       "input.mp4",
       scenes,
       output_file_template="output/scene-${scene_number:03d}.mp4",
       ffmpeg_args=["-c:v", "libx264", "-crf", "23", "-preset", "fast"]
   )

This lets you customize compression and performance for your workflow.

Summary
-------

+--------------------+----------------------------+-----------------------------+
| Mode               | Best For                   | Trade-offs                  |
+====================+============================+=============================+
| `-c copy`          | Fast splitting, no quality | Keyframe-only precision     |
+--------------------+----------------------------+-----------------------------+
| Re-encode (CRF 18) | Archiving                  | Large files, slower         |
+--------------------+----------------------------+-----------------------------+
| Re-encode (CRF 23) | Balanced workflow          | Medium speed, smaller size  |
+--------------------+----------------------------+-----------------------------+
| CRF 26–28          | Previews / lightweight     | Lower quality, very small   |
+--------------------+----------------------------+-----------------------------+

Resources
---------

- ffmpeg Documentation: https://ffmpeg.org/documentation.html
- PySceneDetect API Reference: https://pyscenedetect.readthedocs.io/en/latest/reference/
- CRF Guide (x264): https://trac.ffmpeg.org/wiki/Encode/H.264
