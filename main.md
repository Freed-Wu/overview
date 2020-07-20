# A Overview of NN-based Image/Video Compression

## Main Objectives

Image and video compression aims to seek more compact representation for
visual signals while keeping high quality.

### Compression Ratio

For signal of definite size,
$$\mathrm{Compression Ratio} = \frac{\mathrm{Uncompressed
Size}}{\mathrm{Compressed Size}}$$
For signal of indefinite size(e.g. streaming audio and video),
$$\mathrm{Compression Ratio} = \frac{\mathrm{Uncompressed Data
Rate}}{\mathrm{Compressed Data Rate}}$$

### Reconstruction Quality

Lossless compression can preserve all the information, but it does not
generally achieve high compression ratio due to the intrinsic entropy of the
data.
In contrast, lossy compression(e.g. JPEG for images, MPEG for videos) can
achieve higher compression ratio of at the cost of a decrease in quality.

Rate distortion can be measured by MSE(Mean Square error):
$$\mathrm{MSE} = \frac{1}{mn} \sum_{i = 1}^n \sum_{i = 1}^m (\hat{y}_{ij} -
y_{ij})^2$$
However, human eye is not comparatively sensitive to the variance of MSE.
Hence, a subjective index---PSNR(Peak Signal-to-noise ratio) was proposed.
$$\mathrm{PSNR} = 10\mathrm{lg}\frac{\max_{i, j}{y_{ij}}}{\mathrm{MSE}}$$
Another objective index---MS-SSIM(Multi-scale Structural Similarity Index
Measure) was designed to improve on PSNR.
$$\mathrm{SSIM} = \frac{(2\bar{x}\bar{y} + c_1)(2\sigma_{xy} +
c_2)}{(\bar{x}^2 + \bar{y}^2 + c_1)(\sigma_x^2 + \sigma_y^2 + c_2)}$$

### Encode/Decode Speed

For static images, decode speed is stricter than encode speed.
For dynamic images, because of real-time depend on camera, not only encode
speed but also decode speed need to be concerned.

### Hardware/Software System

Except software realization, some hardware systems have been designed
specially for the task.
A algorithm should be parallel-friendly and easy for VLSI design in order to
be leveraged.

## Key Challenges

It is apparent that the state-of-the-art neural network based end-to-end
image compression is still in its infancy which only outperforms the JPEG2000
and struggles against HEVC. The marriage of neural network and traditional
hybrid video coding framework obtained significant performance improvement
compared with the latest video coding standard, HEVC.

## Current Solutions

### MLP(multi-layer-perceptron)

MLP is one of the early NN techniques.
It consists of an input layer, an output layer, and several hidden layers.
Its easiness makes it famous.
However, due to fully-connected hidden layers, it is too sensitive for input
error, which sometimes result in overfitting.

### CNN(convolution neural network)

CNN solved problem of overfitting---it reduce dependence for data quantity by
some ingenious design(e.g. Downsample layer, Pooling layer, ReLU(rectified
linear unit) layer).

### RNN(recurrent neural network)

Unlike the CNN architecture mentioned above, RNN is a class of neural network
with memory to store the recent behaviors. This allows it to exhibit temporal
dynamic behavior.
Some RNN based image compression scheme can outperform JPEG while it is
inferior to JPEG2000.

### GAN(generative adversarial network)

Generative Adversarial Network is one of most attractive improvements in the
application of deep neural network.
Two network models(i.e. generator and discriminator) takes advantage of deep
neural network to distinguish whether the samples are generated from the
generator or produce samples which pass the inspection.
One of the representative works is proposed by Rippel and Bourdev in 2017,
and it is an integrated and well optimized GAN based image compression, which
not only achieves amazing compression ratio improvement but also can run in
real-time by leveraging the massive parallel computation cores of GPU.

### Intra Prediction

Intra prediction leveraging spatial redundancy plays an important role in the
random jump at a video, which makes it cannot be replaced by inter
prediction.
The latest video coding standard, HEVC, utilized neighboring reconstructed
pixels to predict the current coding block, with 33 angular intra prediction
modes.

Many neural network based image compression methods which have been proposed
to be regarded as intra-coding strategy for video compression can only
surpass JPEG and JPEG2000 and are inferior to HEVC intra coding framework.

### Inter Prediction

On previous coded frames against the current frame, motion compensation
algorithm realizes inter prediction.
In some hybrid video coding, FRCNN(fractional-pixel reference generation CNN)
different from the previous interpolation or super-resolution problem
generates the fractional-pixels from reference frame to approach the current
coding frame, and achieves the state-of-the-art result.

### Quantizing and Entropy Coding

Quantizing and entropy coding are the lossy and lossless compression
procedures respectively.
The scalar quantization reduces the visual redundancy with a low cost.
However, it is not friendly to perceptual quality improvement that a
quantization not conforming to the characteristics of human visual system.
A CNN named VNet-2 is utilized to solve this problem, which predicts local
visual threshold $C_\mathrm{T}$ in the first stage, then calculate a
quantization step by a formula $\log Q_\mathrm{step} = \alpha C_\mathrm{T}^2
+ \beta C_\mathrm{T} + \gamma$.

HEVC adopts the CABAC(context adaptive binary algorithm coding) as its
entropy coding.
Inspired by the prediction efficiency of CNN, the network architecture based
on LeNet-5 improves the CABAC performance on compressing.

### Loop Filter

Loop filter are introduced into video coding standard since H.263+.
Inspired by the success of CNN on image/video restoration filed, many of CNN
based loop filters are designed to remove compression artifacts, which are
much easier to implement the end-to-end training compared with other video
coding modules.

## Unresolved Problems

### Semantic-fidelity Oriented Images/Video Compression

Benefit from the fast development of computer vision techniques,
Semantic-fidelity become critical for further applications as well as
traditional visual-fidelity requirement.

### Rate-distortion Optimization Guided Neural Network Training and Adaptive Switching for Compression Task

The RD(rate-distion) theory has not been well explored in current neural network
based compression tasks.
A single network to deal with all the images and videos with diverse
structures is inefficient obviously.
Hence, the multi-network adaptively training and switching according to RD
is a possible solution.

### Memory and Computation Efficient Design for Practical Image and Video Codec

The burdens in computation and memory hinder the deployment of deep learning
based image and video compression. There is no related research work by
jointly considering both the compression performance and the efficiency in
computation and memory for neural networks, which is important for practical
applications.
