# Perceptual Losses for Real-Time Style Transfer and Super-Resolution

## Main Idea
Combining feed-forward convolutional neural network for image transformation with percecptual loss for perceptual optimization (image generation) to achieve real time style transfer and super resolution.

For image transformation, it usually takes advantage of a supervised CNN with a per-pixel loss. The input is a degraded image and the output is a high-quality image. For perceptual optimization, images are often generated by optimizing a perceptual loss which penalizes dissimilarity on high-level feature maps rather on pixels. This paper combines the advantage of these two methods and proposes a CNN with perceptual loss to do real time optimization (style transfer & super resolution).

## Advantages
- By introducing the transform net, style transfer in its testing stage is three orders of magnitude faster. This is because rather than solving an optimization problem by multiple forward&backward passes, the transform net only takes one forward pass to achieve style transfer.
- By taking advantage of perceptual loss rather than per-pixel loss, the synthesized image captures more semantic information and gives visually pleasing results.

## Architechure
![](https://raw.githubusercontent.com/sunshineatnoon/Paper-Collection/master/images/RTNS.png)
(Note: For super resolution, there's no style target, only content target)

## Algorithm
### Style Transfer
- Input: A 3\*256\*256 photo
- Output: A 3\*256\*256 synthesized image
- Loss function: content loss + style loss. 
   ![](https://raw.githubusercontent.com/sunshineatnoon/Paper-Collection/master/images/content%20loss.png)
   For style loss:
![](https://raw.githubusercontent.com/sunshineatnoon/Paper-Collection/master/images/style%20loss%201.png)
![](https://raw.githubusercontent.com/sunshineatnoon/Paper-Collection/master/images/style%20loss%202.png)
Detailed explainations of these two losses can be found [here](https://github.com/sunshineatnoon/Paper-Collection/blob/master/A%20Neural%20Algorithm%20of%20Artistic%20Style.md). To summarize, the content loss is the distance of feature maps between the photo and synthesized image while the style loss is the distance of Gram Matrix between the painting and synthesized image.And to encourage spatial smoothness in the output image, they also make use of total variation regularizer.
- Training Schema: Per CNN for each style; Roughly two epochs over 80k images from MS COCO; Adam.

### Super Resolution
- Input: A low-resolution 3\*(288/f)\*(288/f) image
- Output: A high-resolution 3\*288\*288 image
- Loss function: pixel loss + perceptual loss. Perceptual loss is the same with style transfer. and pixel loss is below:
  ![](https://raw.githubusercontent.com/sunshineatnoon/Paper-Collection/master/images/pixel%20loss.png)
- Training schema: Per CNN for each resolution.

## TL;DR
1. Peceptual loss worths being considered whenver per pixel loss is used.
2. It's more efficient to integrate the "style information" in a CNN than solving optimization problem for each single image.

## Reference
Johnson J, Alahi A, Fei-Fei L. Perceptual losses for real-time style transfer and super-resolution[J]. arXiv preprint arXiv:1603.08155, 2016.
