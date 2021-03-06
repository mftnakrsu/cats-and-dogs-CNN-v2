# Cats and Dogs


One of the most common methods used when our dataset is insufficient is to use pre-trained models.

In our case, we will consider a large convnet trained on the ImageNet dataset (1.4 million labeled images and 1000 different classes). ImageNet contains many animal classes, including different species of cats and dogs, and we can thus expect to perform very well on our cat vs. dog classification problem.

Some of the backbones we can use:

•Xception
•InceptionV3
•ResNet50
•VGG16
•VGG19
•MobileNet

I will use the VGG16 architecture, developed by Karen Simonyan and Andrew Zisserman in 2014, a simple and widely used convnet architecture for ImageNet. 

## VGG16:

![alt text](https://neurohive.io/wp-content/uploads/2018/11/vgg16-1-e1542731207177.png)

    from keras.applications import VGG16
    
    conv_base=VGG16(weights=('imagenet'),
                    include_top=False,
                    input_shape=(150,150,3)
                    )

•weights, to specify which weight checkpoint to initialize the model from

•include_top, which refers to including or not the densely-connected classifier on top of the network. By default, this densely-connected classifier would correspond to the 1000 classes from ImageNet. Since we intend to use our own densely-connected classifier (with only two classes, cat and dog), we don't need to include it.

You can see detail of the architecture of the VGG16 convolutional base:


    conv_base.summary()
    
![alt text](https://i.imgyukle.com/2021/02/08/LnsuWG.jpg)

### Let's take a look at the loss and accuracy curves:


![alt text](https://i.hizliresim.com/hgRALi.jpg)

As you can see, we reach a validation accuracy of about 92%. This is much better than our small convnet trained from scratch.



# What ConvNets Learn:

This channel appears to encode a diagonal edge detector.

![alt text](https://i.hizliresim.com/LA0jJ3.jpg)

![alt text](https://i.hizliresim.com/zR8OMT.jpg)

![alt text](https://i.hizliresim.com/QQLRJW.jpg)

![alt text](https://i.hizliresim.com/Lai2Sh.jpg)

![alt text](https://i.hizliresim.com/PUVnye.jpg)

A few remarkable things to note here:

The first layer acts as a collection of various edge detectors. At that stage, the activations are still retaining almost all of the information present in the initial picture.

As we go higher-up, the activations become increasingly abstract and less visually interpretable. They start encoding higher-level concepts such as "cat ear" or "cat eye". Higher-up presentations carry increasingly less information about the visual contents of the image, and increasingly more information related to the class of the image.

The sparsity of the activations is increasing with the depth of the layer: in the first layer, all filters are activated by the input image, but in the following layers more and more filters are blank. This means that the pattern encoded by the filter isn't found in the input image.
