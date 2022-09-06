.. title: wai.annotations release 0.8.0
.. slug: 2022-09-06-waiannotations-release-0-8-0
.. date: 2022-09-06 15:38:00 UTC+12:00
.. tags: release
.. category: data
.. link: 
.. description: 
.. type: text


A new release of `wai.annotations <https://github.com/waikato-ufdl/wai-annotations>`__ is out now: `0.8.0 <https://github.com/waikato-ufdl/wai-annotations/releases/tag/v0.8.0>`__

This release contains two major updates:

* `wai.annotations.coqui` module added for processing `Coqui AI <https://github.com/coqui-ai>`__ STT and TTS datasets
* a bug-fix that applies to the dataset splitting (``--split-names``/``--split-ratios``) breaks backwards compatibility;
  the new ``--no-interleaving`` flag enables the old behavior again

Here is a detailed overview of all the changes since the 0.7.8 release:

* wai.annotations.tf is now optional
* install.sh now has -o flag to install optional modules when installing latest (-l)
* Upgraded wai.annotations.commonvoice to 1.0.2

  * the expected header now uses *accents* rather than *accent* (but the reader accepts both)

* Upgraded wai.annotations.core to 0.2.0

  * FilterLabels ISP now treats elements as negative ones if no labels left after
    filtering (in order to use `discard-negatives` in pipeline); also works on
    image classification domain now as well
  * FilterLabels ISP can filter out located objects that don't fall within a certain
    region (x,y,w,h - normalized or absolute) using a supplied IoU threshold; useful
    when concentrating on annotations in the center of an image, e.g., for images
    generated with the subimages ISP (object detection domain only)
  * `logging._LoggingEnabled` module now sets the *numba* logging level to `WARNING`
  * `logging._LoggingEnabled` module now sets the *shapely* logging level to `WARNING`
  * `core.domain.Data` class now stores the path of the file as well
  * Rename ISP allows renaming of files, e.g., for disambiguating across batches
  * `batch_split.Splitter` now handles cases when the regexp does not produce any matches
    (and outputs a warning when in verbose mode)
  * Added LabelPresent ISP, which skips object detection images that do not have specified
    labels (or if annotations do not overlap with defined regions; can be inverted).
  * Using wai.common==0.0.40 now to avoid parse error output when accessing `poly_x`/`poly_y`
    meta-data in `LocatedObject` instances when containing empty strings.
  * The CleanTranscript ISP can be used to clean up speech transcripts.
  * Bug fix for splitting where split-scheduling was calculated with swapped iteration order,
    leading to runs of splits rather than desired interleaving. Added --no-interleave flag to
    re-enable bug for backwards compatibility.

* Upgraded wai.annotations.imgaug to 1.0.6

  * `sub-images` plugin now has a `--verbose` flag; only initializes the regions now once

* Upgraded wai.annotations.layersegments to 1.0.2

  * `FromLayerSegments` class now outputs logging message if setting of new annotation indices fails, as error
    occurs before the `wai.annotations - Sourced ...` logging message, making it possible to track the image
    causing the problem.
  * Added `--lenient` flag to `FromLayerSegments` class which allows conversion of non-binary images with just
    two unique colors to binary ones instead of throwing an error.
  * Added `--invert` flag to `FromLayerSegments` class which allows inverting the colors (b/w <-> w/b) of the
    binary annotation images.

* Upgraded wai.annotations.video to 1.0.2

  * VideoFileReader source now passes on a file path with the frames as well

* Upgraded wai.annotations.yolo to 1.0.1

  * the `read_labels_file` method of `FromYOLOOD` now strips leading/trailing whitespaces
    from the labels.

* Added wai.annotations.coqui 1.0.0

  * from-coqui-stt-sp: reads speech transcriptions in the Coqui STT CSV-format
  * from-coqui-tts-sp: reads speech transcriptions in the Coqui TTS text-format
  * to-coqui-stt-sp: writes speech transcriptions in the Coqui STT CSV-format
  * to-coqui-tts-sp: writes speech transcriptions in the Coqui TTS text-format
