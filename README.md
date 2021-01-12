# Faster RCNN with tf.Keras and Tensorflow and pretrained weights (Updated code to work with current version of TF. A pure Keras installation is not required.)
Keras implementation of Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks.
cloned from https://github.com/neilsen1994/keras-frcnn-2020.updated.git.
pre-trained weights provided by [@VRA](https://github.com/vra/keras-frcnn) with resnet50 backbone and 1500 trained epochs.



USAGE:
- Both theano and tensorflow backends are supported. However compile times are very high in theano, and tensorflow is highly recommended.
- `train_frcnn.py` can be used to train a model. To train on Pascal VOC data, simply do:
`python train_frcnn.py -p /path/to/pascalvoc/`. Ex: python train_frcnn.py -p './data/VOCdevkit'
- the Pascal VOC data set (images and annotations for bounding boxes around the classified objects) can be obtained from: http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar
- simple_parser.py provides an alternative way to input data, using a text file. Simply provide a text file, with each
line containing:

    `filepath,x1,y1,x2,y2,class_name`

    For example:

    /data/imgs/img_001.jpg,837,346,981,456,cow
    
    /data/imgs/img_002.jpg,215,312,279,391,cat

    The classes will be inferred from the file. To use the simple parser instead of the default pascal voc style parser,
    use the command line option `-o simple`. For example `python train_frcnn.py -o simple -p my_data.txt`.

- Running `train_frcnn.py` will write weights to disk to an hdf5 file, as well as all the setting of the training run to a `pickle` file. These
settings can then be loaded by `test_frcnn.py` for any testing.

- test_frcnn.py can be used to perform inference, given pretrained weights and a config file. Specify a path to the folder containing
images:
    `python test_frcnn.py -p /path/to/test_data/`
- Data augmentation can be applied by specifying `--hf` for horizontal flips, `--vf` for vertical flips and `--rot` for 90 degree rotations
- Please click on the following colab link to run Faster RCNN:
[Colab Faster RCNN] (https://colab.research.google.com/drive/12xCIuns9mLukRD4mrCXIbo7m_0WcKZRg?usp=sharing)



NOTES:
- This version runs successfully with Keras 2.4.3 and Tensorflow 2.3.0
- config.py contains all settings for the train or test run. The default settings match those in the original Faster-RCNN
paper. The anchor box sizes are [128, 256, 512] and the ratios are [1:1, 1:2, 2:1].
- The theano backend by default uses a 7x7 pooling region, instead of 14x14 as in the frcnn paper. This cuts down compiling time slightly.
- The tensorflow backend performs a resize on the pooling region, instead of max pooling. This is much more efficient and has little impact on results.


Example output:

![alt text](https://github.com/neilsen1994/keras-frcnn-2020.updated/blob/master/results_imgs-fp-mappen-test/000002.png)
![alt text](https://github.com/neilsen1994/keras-frcnn-2020.updated/blob/master/results_imgs-fp-mappen-test/000004.png)
![alt text](https://github.com/neilsen1994/keras-frcnn-2020.updated/blob/master/results_imgs-fp-mappen-test/000007.png)
![alt text](https://github.com/neilsen1994/keras-frcnn-2020.updated/blob/master/results_imgs-fp-mappen-test/000010.png)
![alt text](https://github.com/neilsen1994/keras-frcnn-2020.updated/blob/master/results_imgs-fp-mappen-test/1.png)
![alt text](https://github.com/neilsen1994/keras-frcnn-2020.updated/blob/master/results_imgs-fp-mappen-test/2.png)
![alt text](https://github.com/neilsen1994/keras-frcnn-2020.updated/blob/master/results_imgs-fp-mappen-test/4.png)
![alt text](https://github.com/neilsen1994/keras-frcnn-2020.updated/blob/master/results_imgs-fp-mappen-test/5.png)

ISSUES:

- I ran this code with only 1000 images and for 1755 epochs. More images will give better result.
- If you get this error:
`ValueError: There is a negative shape in the graph!`    
    than update keras to the newest version

- `python3` should work. Thank me later :).

- If you run out of memory, try reducing the number of ROIs that are processed simultaneously. Try passing a lower `-n` to `train_frcnn.py`. Alternatively, try reducing the image size from the default value of 600 (this setting is found in `config.py`.
