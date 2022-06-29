.. title: wai.annotations release 0.7.8
.. slug: 2022-06-29-waiannotations-release-0-7.8
.. date: 2022-06-29 16:15:00 UTC+12:00
.. tags: release
.. category: data
.. link: 
.. description: 
.. type: text


A new release of `wai.annotations <https://github.com/waikato-ufdl/wai-annotations>`__ is out now: `0.7.8 <https://github.com/waikato-ufdl/wai-annotations/releases/tag/v0.7.8>`__

This release contains three major updates:

* additional domain for audio classification, uses the `-ac` suffix for domain-specific plugins
* `wai.annotations.audio` module added for processing and augmenting audio data
* `wai.annotations.generic` module makes it easier for plugging in your own Python classes (derived
  from wai.annotations classes), as you don't have to write a lot of boiler-plate code to integrate
  it into the framework (the wrappers take care of that!) - check out the manual for examples

Here is a detailed overview of all the changes since the 0.7.7 release:

* wai.annotations.core updated to 0.1.8

  * Added new audio domain for classification using suffix `-ac`
  * Added dataset reader for audio files: `from-audio-files-sp`, `from-audio-files-ac`
  * Added dataset writer for audio files: `to-audio-files-sp`, `to-audio-files-ac`
  * Added dummy sink for audio files: `to-void-ac`
  * Added ISP for selecting a sub-sample from the stream: `sample`

* wai.annotations.subdir updated to 1.0.1

  * added reader/writer for audio classification: `from-subdir-ac` and `to-subdir-ac`

* wai.annotations.audio added for audio support (currently at 1.0.1)

  * `audio-info-ac`: sink for collating/outputting information on the audio classification files
  * `audio-info-sp`: sink for collating/outputting information on the speech files
  * `convert-to-mono`: ISP for converting MP3/OGG/FLAC/WAV to mono WAV
  * `convert-to-wav`: ISP for converting MP3/OGG/FLAC to WAV
  * `mel-spectrogram`: XDC for generating plot from a mel spectrogram (outputs image classification instance)
  * `mfcc-spectrogram`: XDC for generating plots from Mel-frequency cepstral coefficients (outputs image classification instance).
  * `pitch-shift`: augmentation ISP for shifting the pitch
  * `resample-audio`: ISP for resampling MP3/OGG/FLAC/WAV
  * `stft-spectrogram`: XDC for generating plot from a short-time fourier-transform spectrogram (outputs image classification instance)
  * `time-stretch`: augmentation ISP for time-stretching audio (speed up/slow down)
  * `trim-audio`: ISP for trimming silence from audio

* wai.annotations.generic added (currently at 1.0.0)

  * `generic-source-ac`: wrapper around a user-supplied source class for audio classification
  * `generic-source-ic`: wrapper around a user-supplied source class for image classification
  * `generic-source-is`: wrapper around a user-supplied source class for image segmentation
  * `generic-source-od`: wrapper around a user-supplied source class for object detection
  * `generic-source-sp`: wrapper around a user-supplied sourceclass for speech
  * `generic-isp-ac`: wrapper around a user-supplied ISP class for audio classification
  * `generic-isp-ic`: wrapper around a user-supplied ISP class for image classification
  * `generic-isp-is`: wrapper around a user-supplied ISP class for image segmentation
  * `generic-isp-od`: wrapper around a user-supplied ISP class for object detection
  * `generic-isp-sp`: wrapper around a user-supplied ISP class for speech
  * `generic-sink-ac`: wrapper around a user-supplied sink class for audio classification
  * `generic-sink-ic`: wrapper around a user-supplied sink class for image classification
  * `generic-sink-is`: wrapper around a user-supplied sink class for image segmentation
  * `generic-sink-od`: wrapper around a user-supplied sink class for object detection
  * `generic-sink-sp`: wrapper around a user-supplied sinkclass for speech
