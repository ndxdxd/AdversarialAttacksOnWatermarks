**Overview**

**Introduction**
With the rise of Generative AI, protecting digital media authenticity has become increasingly crucial. Invisible watermarking is a viable solution, preserving image quality while preventing unauthorized alterations. However, attacks like Gaussian blur, noise, cropping, and AI-driven regeneration methods—such as the Stable Diffusion Watermark Removal Attack—can effectively remove these watermarks, leading to issues like misinformation.

In this project, we explore a novel watermarking approach designed to withstand regeneration attacks. Our method leverages adversarial perturbations using the Projected Gradient Descent (PGD) attack, which subtly modifies images in ways that disrupt AI models while maintaining human imperceptibility. Unlike prior research that focuses on defending against adversarial attacks, we utilize them to create a watermark that remains resilient against AI-based modifications.

By integrating adversarial perturbations, our watermarking technique makes it more challenging for generative models to remove or ignore embedded signals, ensuring ownership verification even after AI alterations. We use the ImageNet dataset and the ResNet50 model to develop and evaluate our approach, testing its robustness on a validation set of 1,000 images.

**Methods**

*PGD Attack*

*Whitebox Attack*

*Blackbox Attack*

**Results**

**Conclusion**

**References**

**About Us** 
Andy Truong 
amt007@ucsd.edu

Anushka Purohit
apurohit@ucsd.edu

Mentor: Yu-Xiang Wang
yuxiangw@ucsd.edu
