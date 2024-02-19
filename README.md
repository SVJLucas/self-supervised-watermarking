# Can Unlabeled Data Improve Watermark Security? Investigating Self-Supervised Models for Image Zero-Watermarking

## Sumary

Analyzing Self-Supervised Models: **CLIP**, **DiNOv2**, and **ViTMAE** in the Context of Zero-Watermarking

- [Introduction and Motivation](#Introduction-and-Motivation)
- [Zero-Watermarking Pipeline](#Zero-Watermarking-Pipeline)
  - [Watermark "Embedding" Process Using Self-Supervised Models: Master Share Construction](#watermark-embedding-process-using-self-supervised-models-master-share-construction)
  - [Watermark Retrieval Process Using Self-Supervised Models: Master Share in Action](#watermark-retrieval-process-using-self-supervised-models-master-share-in-action)
- [The Oxford 102 Flowers Dataset: A Brief Overview](#the-oxford-102-flowers-dataset-a-brief-overview)
- [Considered Attacks](#Considered-Attacks)
- [Metrics, Results and Discussion](#Metrics-Results-and-Discussion)
- [Conclusion](#Conclusion)
- [References](#References)


# Introduction and Motivation
In the rapidly advancing field of digital media security, the concept of zero-watermarking has emerged as a critical strategy, particularly in domains where the slightest image distortion could result in significant inaccuracies in diagnosis or detection, such as in medical and remote sensing images. Traditional watermarking techniques, while serving key roles in copyright protection and content authentication, inherently introduce some distortion to the host image. This limitation calls for a shift towards zero-watermarking approaches, wherein the watermark maintains a logical connection with the host image without physical embedding, thus preserving the original image's integrity.

In the study **"A Robust Image Zero-watermarking using Convolutional Neural Networks"** [1] by _Fierro et al._, the authors explore leveraging the latent space of Convolutional Neural Networks (CNN) for creating image-identifying masks. However, their approach involves training their CNN in a supervised manner. Contrasting this, **"A Comparison of Supervised and Unsupervised Deep Learning Methods for Anomaly Detection in Images"**[2] suggests that supervised learning models, while adept at learning task-specific features, might not fully capture the comprehensive information present within images, tending instead to prioritize task-relevant features over a wider understanding of the image content.

Against this backdrop, _Fernandez et al._, in **"Watermarking Images in Self-Supervised Latent Spaces"**[3], propose an innovative method inspired by **"Are Deep Neural Networks good for blind image watermarking?"**[4]. This new approach employs self-supervised learning models, diverging from previous methods that rely on supervised learning models. However, the requirement for processing thousands of images to sphericalize the latent space of unsupervised models using PCA—a step deemed necessary in the aforementioned studies to ensure a low False Positive Rate (FPR)—presents a significant challenge for implementing the method [3] on a smaller scale.

To navigate these challenges, we propose utilizing the approach detailed by _Fierro et al._[1], which does not require extensive pre-processing, but adapted to employ self-supervised models. This novel method promises adaptability across any resolution and robustness against a broad range of transformations.

This exploration highlights three pioneering models: **CLIP**[5] from OpenAI, **ViTMAE**[6], and **DiNOv2**[7] from META, each known for their unique capabilities:

- **CLIP** excels in deciphering complex visual concepts through the lens of natural language, offering an intuitive understanding of image content.
- **ViTMAE** introduces a novel approach by learning from regions of images that are masked, focusing on the inherent structure and semantics of the image beyond the visible.
- **DiNOv2** stands out for its ability to distill knowledge across various visual domains, demonstrating exceptional adaptability and learning efficiency.
  
These models represent the cutting edge in leveraging self-supervised learning for enhancing the security and efficacy of digital watermarking, indicating a promising future for the protection of digital media against unauthorized manipulation and ensuring the preservation of watermark integrity across diverse applications.

# Zero-Watermarking Pipeline

## Watermark "Embedding" Process Using Self-Supervised Models: Master Share Construction

The watermark embedding process using a self-supervised model involves several steps to ensure the watermark is securely integrated with the image's latent features, as shown in [Figure 1]. The process can be described using the following steps and equations:

**1. Feature Extraction**: Once the have access to a computer vision self-supervised model, we extract the inherent features of the image it. This output provides a high-level representation of the image's content.

**2. Binary Conversion**: The latent space representation `LS` is then converted into a binary sequence. This conversion is done using a thresholding operation, which can be defined as follows: LS[i] = 1 if LS[i] > 0 else 0, for each i in LS, where `LS[i]` is the ith element of the latent space representation. 1 and 0 are the binary states.

**3. Master Share Generation**: Finally, the master share `MS` is generated by performing an XOR operation between the binary sequence of the inherent image feature `LS` and binary watermark pattern `W` as follow: MS = LS ⊗ W, where `⊗` denotes the XOR operation.

**4. Master Share Storage**: The master share `MS` is securely stored by the owner. This ensures that the watermark is logically linked with the host image without physically altering the image, preserving its original quality.



<p align="center">
  <img src="https://github.com/SVJLucas/self-supervised-watermarking/assets/60625769/68f235c5-921f-4162-9dc6-a039eff14a2d" alt="Method Embedding Pipeline" height="400px" />
</p>

<p align="center"><b>Figure 1</b>: Watermark Embedding Pipeline</p>



## Watermark Retrieval Process Using Self-Supervised Models: Master Share in Action

The process of verifying the ownership of potentially attacked images involves extracting features using a self-supervised model and comparing these with the stored master share, `MS`. The steps are as follows, as shown in [Figure 2]:

**1. Feature Extraction from Attacked Image**: The image under analysis is input into the self-supervised model. Then, the model outputs the latent space representation, denoted as `LS'` for the potentially attacked image.

**2. Binarization of the Output Data**: The real output data from the latent space representation is then binarized.  The binarization process converts the real values into binary form using a predefined threshold, typically 0.5.

**3. Watermark Retrieval**: The binary latent space representation of the attacked image, `LS'`, is then XORed with the stored master share, `MS`, to retrieve the permutated watermark sequence: W' = MS ⊗ LS', where `⊗` denotes the XOR bit-by-bit operation,  `LS'` is the binary sequence of latent space features from the potentially attacked image, `MS` is the master share stored securely by the owner and `W'` is the extracted watermark pattern.


<p align="center">
  <img src="https://github.com/SVJLucas/self-supervised-watermarking/assets/60625769/7ae3b85a-5ff9-4b20-8f9b-06743ca1475b" alt="Method Extractor Pipeline" height="400px" />
</p>
<p align="center"><b>Figure 2</b>: Watermark Retrieval Process</p>

# The Oxford 102 Flowers Dataset: A Brief Overview

The Oxford 102 Flowers dataset [8] is a popular image classification dataset that contains 102 flower categories, each with 40-258 images. The images have large scale, pose, and light variations, making it a challenging dataset for image classification algorithms.

We will conduct the experiment using 10 randomly selected images from the Oxford 102 dataset. Our goal is to explore the potential of self-supervised models in zero-watermarking and we're going to use this dataset for it.


<p align="center">
  <img src="https://github.com/SVJLucas/self-supervised-watermarking/assets/60625769/db8716d1-3f11-4237-9253-8baf134ee0f0" alt="Selected Data" height="400px" />
</p>
<p align="center"><b>Figure 3</b>: Dataset samples</p>

We will perform several experiments using these images: first, we'll gerate master share. After that, we will simulate attacks in the image that attempt to break the watermark pattern in the latent space to compare the performance of the models used.

#  Considered Attacks

Assessing the resilience of watermarking algorithms involves simulating a variety of conditions to test their robustness. These tests include manipulating image orientation, introducing visual noise, and varying image visibility through cropping, scaling, and blurring. Additionally, the algorithms are challenged with changes in brightness and contrast to evaluate their performance under different lighting and exposure conditions. 

- **Rotation Attack Test**:The Rotation Attack Test evaluates an algorithm's tolerance to orientation changes by rotating images by a random angle within a specified range (e.g., -5 to 5 degrees). This simulates real-world variations in camera angle or object orientation, challenging the algorithm's ability to recognize rotated objects without loss of accuracy.

- **Noise Test**: The Noise Test assesses an algorithm's robustness against visual noise by adding Gaussian noise with a random standard deviation to images. This mimics sensor noise or low-light conditions, testing the algorithm's capability to maintain performance despite the presence of random pixel value fluctuations.

- **Crop Ratio Test**: The Crop Ratio Test determines the impact of partial visibility on algorithm performance by cropping a random percentage of the image's border and resizing it back to the original dimensions. This simulates scenarios where objects are partially occluded or outside the camera's field of view.

- **Resize Scale Test**: The Resize Scale Test investigates the algorithm's resilience to scale variations by resizing images by a random scale factor and then resizing back to original dimensions. It mimics real-world conditions of objects appearing larger or smaller due to changes in distance to the camera.

- **Gaussian Blur Test**: The Gaussian Blur Test examines an algorithm's ability to handle images with reduced sharpness by applying Gaussian blur with a random intensity. This simulates motion blur or out-of-focus conditions, challenging the algorithm's capacity to recognize blurred objects.

- **Brightness Test**: The Brightness Test evaluates how changes in image brightness affect an algorithm's performance by adjusting the brightness with a random value within a specified range. This test simulates variations in lighting conditions, from underexposure to overexposure.

- **Contrast Test**: The Contrast Test assesses the effect of contrast adjustments on an algorithm's accuracy by modifying the contrast of images with a random factor. This simulates conditions with varying levels of contrast, testing the algorithm's ability to discern objects against backgrounds with minimal to significant contrast differences.

Each attack provides insight into specific aspects of algorithm robustness, highlighting vulnerabilities and areas for improvement in handling real-world image variations.

# Metrics, Results and Discussion

The comparative analysis of the self-supervised models CLIP, DiNOv2, and ViTMAE across various transformation tests unveils insightful trends in their performance, particularly in the context of image zero-watermarking. For instance, DiNOv2 consistently outperforms the other models in Normalized Correlation (NC) [Table 1] across all tests, suggesting its superior capability in maintaining the integrity of the watermark despite various forms of image manipulation. This is especially notable in tests like the "Resize Scale Test" and "Gaussian Blur Test," where DiNOv2's NC values significantly surpass those of CLIP and ViTMAE, highlighting its robustness against scaling and blurring—common challenges in watermark security.

Conversely, when examining the Bit Error Rate (BER) [Table 2], a lower score is preferable as it indicates fewer errors in watermark extraction. Here, DiNOv2 again shows exceptional performance, particularly in the "Gaussian Blur Test" and "Brightness Test," suggesting that it can effectively recover watermarks from images subjected to blurring and brightness alterations. This consistent achievement across both NC and BER metrics underscores the potential of self-supervised learning models like DiNOv2 in enhancing the security and resilience of digital watermarking techniques.

### Normalized Correlation (NC) [Table 1]

| Test                | CLIP            | DiNOv2          | ViTMAE          |
|---------------------|-----------------|-----------------|-----------------|
| Rotation Attack Test| 0.8624          | **0.9431**      | 0.8693          |
| Noise Test          | 0.7872          | **0.8575**      | 0.8506          |
| Crop Ratio Test     | 0.9070          | **0.9423**      | 0.9006          |
| Resize Scale Test   | 0.9408          | **0.9774**      | 0.9093          |
| Gaussian Blur Test  | 0.9953          | **0.9986**      | 0.9085          |
| Brightness Test     | 0.9756          | **0.9914**      | 0.9070          |
| Contrast Test       | 0.9741          | **0.9936**      | 0.9081          |

### Bit Error Rate (BER) [Table 2]

| Test                | CLIP            | DiNOv2          | ViTMAE          |
|---------------------|-----------------|-----------------|-----------------|
| Rotation Attack Test| 0.0700          | **0.0296**      | 0.0681          |
| Noise Test          | 0.1070          | **0.0738**      | 0.0777          |
| Crop Ratio Test     | 0.0472          | **0.0301**      | 0.0518          |
| Resize Scale Test   | 0.0301          | **0.0118**      | 0.0472          |
| Gaussian Blur Test  | 0.0024          | **0.0007**      | 0.0476          |
| Brightness Test     | 0.0124          | **0.0045**      | 0.0485          |
| Contrast Test       | 0.0132          | **0.0033**      | 0.0479          |

# Conclusion

The exploration into the efficacy of self-supervised models like CLIP, DiNOv2, and ViTMAE in the realm of image zero-watermarking sheds light on the transformative potential of these algorithms for enhancing watermark security. Through meticulous evaluation across a spectrum of transformation tests—ranging from rotation and noise addition to cropping, scaling, blurring, and adjustments in brightness and contrast—this study has underscored the nuanced capabilities of each model to maintain the fidelity of watermarks under adversarial conditions. DiNOv2, in particular, emerged as a standout performer, consistently demonstrating superior normalized correlation (NC) and lower bit error rates (BER), indicative of its robustness and reliability in watermark recovery across various distortive scenarios.

This research contributes to the broader discourse on digital watermarking security, presenting self-supervised learning as a promising avenue for fortifying watermark resilience against sophisticated manipulations. The findings advocate for the integration of models like DiNOv2 into watermarking systems, improving watermark security strategies by leveraging unlabeled data for improved performance. As digital content continues to proliferate and the sophistication of adversarial attacks advances, the adoption of self-supervised learning models could play a critical role in safeguarding digital media integrity. Future research should aim to extend these findings, exploring the scalability of these models in diverse real-world applications and their adaptability to evolving threats, ensuring the continuous evolution of digital watermarking technologies in the face of emerging challenges.


# References

[1]: Atoany Fierro-Radilla, Mariko Nakano-Miyatake, Manuel Cedillo-Hernandez, Laura Cleofas-Sanchez, and Hector Perez-Meana. A robust image zero-watermarking using convolutional neural networks. In 2019 7th International Workshop on Biometrics and Forensics (IWBF), pages 1–5. IEEE, 2019.

[2]: Vincent Wilmet and Sauraj Verma and Tabea Redl and Håkon Sandaker and Zhenning Li. A Comparison of Supervised and Unsupervised Deep Learning Methods for Anomaly Detection in Images.

[3]: Pierre Fernandez, Alexandre Sablayrolles, Teddy Furon, Hervé Jégou, and Matthijs Douze. Watermarking images in self-supervised latent spaces. In ICASSP 2022-2022 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP), pages 3054–3058. IEEE, 2022.

[4]: Vedran Vukotic, Vivien Chappelier, and Teddy Furon. Are classification deep neural networks good for blind image watermarking? Entropy, 22(2):198, 2020.

[5]: Alec Radford, Jong Wook Kim, Chris Hallacy, Aditya Ramesh, Gabriel Goh, Sandhini Agarwal, Girish Sastry, Amanda Askell, Pamela Mishkin, Jack Clark, Gretchen Krueger, and Ilya Sutskever. Learning Transferable Visual Models From Natural Language Supervision. In ICML,2021.

[6]: Kaiming He, Xinlei Chen, Saining Xie, Yanghao Li, Piotr Doll´ar, and Ross Girshick. Masked autoencoders are scalable vision learners. arXiv:2111.06377, 2021

[7]: M. Oquab, T. Darcet, T. Moutakanni, H. Vo, M. Szafraniec, V. Khalidov, P. Fernandez, D. Haziza, F. Massa, A. El-Nouby et al., “Dinov2: Learning robust visual features without supervision,” arXiv preprint arXiv:2304.07193, 2023.

[8]: Nilsback, M.-E., & Zisserman, A. (2008). Automated flower classification over a large number of classes. Visual Geometry Group, Department of Engineering Science, University of Oxford, United Kingdom.

