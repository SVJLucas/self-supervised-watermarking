# Can Unlabeled Data Improve Watermark Security? Investigating Self-Supervised Models for Image Zero-Watermarking


# Zero-Watermarking Pipeline

## Embedding Pipeline

<p align="center">
  <img src="https://github.com/SVJLucas/self-supervised-watermarking/assets/60625769/68f235c5-921f-4162-9dc6-a039eff14a2d" alt="Method Embedding Pipeline" height="400px" />
</p>

## Extractor Pipeline
<p align="center">
  <img src="https://github.com/SVJLucas/self-supervised-watermarking/assets/60625769/7ae3b85a-5ff9-4b20-8f9b-06743ca1475b" alt="Method Extractor Pipeline" height="400px" />
</p>

# The Oxford 102 Flowers Dataset: A Brief Overview

The Oxford 102 Flowers dataset is a popular image classification dataset that contains 102 flower categories, each with 40-258 images. The images have large scale, pose, and light variations, making it a challenging dataset for image classification algorithms.

We will conduct the experiment using 10 randomly selected images from the Oxford 102 dataset. Our goal is to explore the potential of self-supervised models in zero-watermarking and we're going to use this dataset for it.

<p align="center">
  <img src="https://github.com/SVJLucas/self-supervised-watermarking/assets/60625769/db8716d1-3f11-4237-9253-8baf134ee0f0" alt="Selected Data" height="400px" />
</p>

## Attacks

- **Rotation Attack Test**:The Rotation Attack Test evaluates an algorithm's tolerance to orientation changes by rotating images by a random angle within a specified range (e.g., -5 to 5 degrees). This simulates real-world variations in camera angle or object orientation, challenging the algorithm's ability to recognize rotated objects without loss of accuracy.

- **Noise Test**: The Noise Test assesses an algorithm's robustness against visual noise by adding Gaussian noise with a random standard deviation to images. This mimics sensor noise or low-light conditions, testing the algorithm's capability to maintain performance despite the presence of random pixel value fluctuations.

- **Crop Ratio Test**: The Crop Ratio Test determines the impact of partial visibility on algorithm performance by cropping a random percentage of the image's border and resizing it back to the original dimensions. This simulates scenarios where objects are partially occluded or outside the camera's field of view.

- **Resize Scale Test**: The Resize Scale Test investigates the algorithm's resilience to scale variations by resizing images by a random scale factor and then resizing back to original dimensions. It mimics real-world conditions of objects appearing larger or smaller due to changes in distance to the camera.

- **Gaussian Blur Test**: The Gaussian Blur Test examines an algorithm's ability to handle images with reduced sharpness by applying Gaussian blur with a random intensity. This simulates motion blur or out-of-focus conditions, challenging the algorithm's capacity to recognize blurred objects.

- **Brightness Test**: The Brightness Test evaluates how changes in image brightness affect an algorithm's performance by adjusting the brightness with a random value within a specified range. This test simulates variations in lighting conditions, from underexposure to overexposure.

- **Contrast Test**: The Contrast Test assesses the effect of contrast adjustments on an algorithm's accuracy by modifying the contrast of images with a random factor. This simulates conditions with varying levels of contrast, testing the algorithm's ability to discern objects against backgrounds with minimal to significant contrast differences.

Each attack provides insight into specific aspects of algorithm robustness, highlighting vulnerabilities and areas for improvement in handling real-world image variations.
