# Introduction to BRIEF(Binary Robust Independent Elementary Features)

Feature point descriptors are now at the core of many Computer Vision technologies, such as object recognition, 3D reconstruction, image retrieval, and camera localization. Since applications of these technologies have to handle ever more data or to run on mobile devices with limited computational resources, there is a growing need for local descriptors that are fast to compute, fast to match, and memory eﬃcient.

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/0*iA-pC5LfbTcTgAcS" />
</p>

In this article, we use binary strings as an eﬃcient feature point descriptor, which is called BRIEF. BRIEF is very fast both to build and to match.BRIEF easily outperforms other fast descriptors such as SURF and SIFT in terms of speed and terms of recognition rate in many cases.

## Background

After detecting keypoint we go on to compute a descriptor for every one of them. Feature descriptors encode interesting information into a series of numbers and act as a sort of numerical “fingerprint” that can be used to differentiate one feature from another. The defined neighborhood around pixel(keypoint) is known as a patch, which is a square of some pixel width and height.

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2976/1*lZ1sloEeHwZ3tXw4InCZJw.png" />
</p>

Image patches could be eﬀectively classiﬁed on the basis of a relatively small number of pair-wise intensity comparisons. Brief convert image patches into a binary feature vector so that together they can represent an object. Binary features vector also know as binary feature descriptor is a feature vector that only contains 1 and 0. In brief  each keypoint is described by a feature vector which is 128–512 bits string.

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/NaN/1*XWpgdt4Z4xeT-g8hn5JLsA.png" />
</p>

## **Smoothing Kernels**

Brief deals with the image at pixel level so it is very noise-sensitive. By pre-smoothing the patch, this sensitivity can be reduced, thus increasing the stability and repeatability of the descriptors. It is for the same reason that images need to be smoothed before they can be meaningfully diﬀerentiated when looking for edges.

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/0*dPQZlNjW3bcO0jES.jpg" />
</p>

Brief uses Gaussian kernel for smoothing image. In the figure below we comparison of Gaussian smoothing on the recognition rates for variances of Gaussian kernel ranging from 0 to 3. The more diﬃcult the matching, the more important smoothing becomes to achieving good performance. The recognition rates remain relatively constant in the 1 to 3 range and, in practice, we use a value of 2.

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/1*hCHYVWW0CJHEP1ie9IwP0g.jpeg" />
</p>

## Smoothed Image to Binary Feature Vector

Now we have smoothed image patch, p. Now our target is to create a binary feature vector out of this patch. We simply create a binary feature vector of the binary test(τ) responses. A binary test τ is defined by:

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2292/1*4OnDKy41p6ycUmhIaE_xIQ.png" />
</p>

where p(x) is the intensity of p at a point x. Choosing a set of **n(**x,y**)**-location pairs uniquely deﬁnes a set of binary tests. Where **n** is the length of the binary feature vector and it could be 128, 256, and 512.

### How to select (x, y) pairs?

Generating a length **n** bit vector leaves many options for selecting the test locations((x, y) pair). The (x,y) pair is also called random pair which is located inside the patch. Total we have to select **n** test(random pair) for creating a binary feature vector and we have to choose this **n** test from one of five approaches(Sampling Geometries) given below.

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/0*bTfQfO4qOxk3qL78" />
</p>

Let consider the size of patch p is (S x S) and assuming keypoint is located in the center of the patch.

1. **Uniform(G I)**: Both x and y pixels in the random pair is drawn from a Unifrom distribution or spread of **S/2 around keypoint**. The pair(test) can lie close to the patch border.

1. **Gaussian(G II)**: Both x and y pixels in the random pair is drawn from a Gaussian distribution or spread of **0.04 * S² around keypoint**.

1. **Gaussian(G III)**: The first pixel(x) in the random pair is drawn from a Gaussian distribution centered around the **keypoint** with a stranded deviation or spread of **0.04 * S²**. The second pixel(y) in the random pair is drawn from a Gaussian distribution centered around the **first pixel(x) **with a standard deviation or spread of **0.01 * S²**. This forces the test(pair) to be more local. Test(pair) locations outside the patch are clamped to the edge of the patch.

1. **Coarse Polar Grid(G IV):** Both x and y pixels in the random pair is sampled from discrete locations of a coarse polar grid introducing a spatial quantization.

1. **Coarse Polar Grid(G V):** The first pixel(x) in random pair is at (0, 0) and the second pixel(y) in the random pair is drawn from discrete locations of a coarse polar grid.

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/0*k0oja80SnH9p8fqe.png" />
</p>

Finally, our BRIEF descriptor look like:

<p align="center">
  <img src="https://cdn-images-1.medium.com/max/2000/1*YIpY-B3oQCg6CQgpxpHipg.png" />
</p>

## Advantages of BRIEF

Brief relies on a relatively small number of intensity diﬀerence tests to represent an image patch as a binary string. Not only is construction and matching for this descriptor much faster than for other state-of-the-art ones, but it also tends to yield higher recognition rates, as long as invariance to large in-plane rotations is not a requirement.


## References

* [https://docs.opencv.org/3.1.0/dc/d7d/tutorial_py_brief.html](https://docs.opencv.org/3.1.0/dc/d7d/tutorial_py_brief.html)

* [https://in.udacity.com/course/computer-vision-nanodegree--nd891](https://in.udacity.com/course/computer-vision-nanodegree--nd891)

* [https://gilscvblog.com/2013/09/19/a-tutorial-on-binary-descriptors-part-2-the-brief-descriptor/](https://gilscvblog.com/2013/09/19/a-tutorial-on-binary-descriptors-part-2-the-brief-descriptor/)

* [https://www.researchgate.net/publication/221304115_BRIEF_Binary_Robust_Independent_Elementary_Features](https://www.researchgate.net/publication/221304115_BRIEF_Binary_Robust_Independent_Elementary_Features)

## Thanks for reading! If you enjoyed it, hit that clap button below and subscribe to updates on [my website](http://deepanshut041.github.io)! It would mean a lot to me and encourage me to write more stories like this.
