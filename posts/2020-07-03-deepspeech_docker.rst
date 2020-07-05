.. title: DeepSpeech Docker image available
.. slug: 2020-07-03-deepspeech-docker
.. date: 2020-07-03 16:26:00 UTC+12:00
.. tags: release
.. category: docker
.. link: 
.. description: 
.. type: text


A new Docker image is available for training `DeepSpeech <https://github.com/mozilla/DeepSpeech>`__ models using a GPU backend. 
The image is based on Mozilla's DeepSpeech 0.7.4 one for training models, adding more functionality to it:

* support for MP3 and OGG files, not just WAV
* automatic alphabet generation from the transcripts
* split sound files into chunks based on detected pauses
* batch process audio files (can be continuous)
* many Python utilities have been sym-linked 

More information on the Docker image is available from Github:

`github.com/waikato-datamining/tensorflow/tree/master/deepspeech <https://github.com/waikato-datamining/tensorflow/tree/master/deepspeech>`__

