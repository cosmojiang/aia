## Tips for installation of Tensorflow models
Many deep learning algorithms have been implemented using Tensorflow and maintained by a public community in a repository called "TensorFlow Models". You can access the repo via the link https://github.com/tensorflow/models.

In general, you can just follow the official document (https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md) to install the models. Here, I summarized some common installation issues that the official guideline did not cover.

**Protobuf compiler**

Protocol Buffers (a.k.a., protobuf) are Google's language-neutral, platform-neutral, extensible mechanism for serializing structured data. This is commonly used in the TensorFlow models to convert content between different formats. Since the April update, protobuf 3+ is required to compile necessary proto files for using the models. Although the latest official document expresses the need of protobuf 3+, it did not provide the way of installing it. 

If you are using Ubuntu 18.04 LTS, the apt repository contains a protobuf-compiler at the version of 3.0.0.9. I assume this would work for the models (I did not test this). Alternatively, I prefer to install the latest version of protobuf manually.

Download its prebuilt release from the web page https://github.com/google/protobuf/releases. If you would like to build it by yourself, please visit https://github.com/google/protobuf for detailed information.

Direct to the folder where you save the prebuilt release file.
Unzip the tar file
```shell
$ tar -xvf protobuf-all-3.5.1.tar.gz
$ cd protobuf-3.5.1
$ ./configure
$ make && make check
$ sudo make install
$ sudo ldconfig # refresh shared library cache.
$ protoc --version # check the installed version
```

**model_lib.py**
If you use Python 3.X, you probably need to revise the following python scripts to run the latest tensorflow models (as of the version on 2018-07-13).
  Line 282: revise losses_dict.itervalues() to losses_dict.values()
  Line 391: revise eval_metric_ops.iteritems() to eval_metric_ops.items()

**utils/object_detection_evaluation.py**
If you use Python 3.X, you probably need to revise the following python scripts to run the latest tensorflow models (as of the version on 2018-07-13).
  Line 842 to 844
  revise print 'Scores and tpfp per class label: {}'.format(class_index) to print('Scores and tpfp per class label: {0}'.format(class_index))
  revise print tp_fp_labels to print(tp_fp_labels)
  revise print scores to print(scores)

**utils/learning_schedules.py**

After installing the proper protobuf, you will successfully install the models and run its test builders. However, as of the writing, there is a small bug in the object_detection/utils/learning_schedules.py file, which throws errors when you train networks. 

Open the file and locate the line 168:
Use a list() to wrap the range(num_boundaries)
```python
list(range(num_boundaries))
```
This will fix the ValueError: Tried to convert 't' to a tensor and failed. Error: Argument must be a dense tensor: range(0, 3) - got shape [3], but wanted [].

More detailed history of this issue is referred to https://github.com/tensorflow/models/issues/3752.

**SSD extractor bug**

When you use the SSD_MobileNet models, it appears to have an error TypeError: `pred` must be a Tensor, or a Python bool, or 1 or 0. Found instead: None or "object_detection.protos.SsdFeatureExtractor" has no field named "batch_norm_trainable".

Open the file object_detection/models/ssd_mobilenet_v1_feature_extractor.py (and ssd_mobilenet_v2_feature_extractor.py)
Locate the line 109, change the is_training=None to is_training=True
```python
is_training=True
```
Detailed history of the issue is referred to https://github.com/tensorflow/models/issues/4043
