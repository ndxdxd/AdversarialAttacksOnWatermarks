# Adversarial Attacks on Watermarks

## Overview

### Introduction
With the rise of Generative AI, protecting digital media authenticity has become increasingly crucial. Invisible watermarking is a viable solution, preserving image quality while preventing unauthorized alterations. However, attacks like Gaussian blur, noise, cropping, and AI-driven regeneration methods—such as the Stable Diffusion Watermark Removal Attack—can effectively remove these watermarks, leading to issues like misinformation.

In this project, we explore a novel watermarking approach designed to withstand regeneration attacks. Our method leverages adversarial perturbations using the Projected Gradient Descent (PGD) attack, which subtly modifies images in ways that disrupt AI models while maintaining human imperceptibility. Unlike prior research that focuses on defending against adversarial attacks, we utilize them to create a watermark that remains resilient against AI-based modifications.

By integrating adversarial perturbations, our watermarking technique makes it more challenging for generative models to remove or ignore embedded signals, ensuring ownership verification even after AI alterations. We use the ImageNet dataset and the ResNet50 model to develop and evaluate our approach, testing its robustness on a validation set of 1,000 images.

### Methods

#### PGD Attack
The Projected Gradient Descent (PGD) attack is a widely used adversarial attack that iteratively perturbs an input sample to maximize the loss of a deep learning model while keeping the perturbation within a specified norm constraint. This method extends the Fast Gradient Sign Method (FGSM) by applying multiple iterative updates with projection to ensure that the perturbation remains within a bounded region. We use this attack as the base of our watermark. 

#### Whitebox Attack
Our whitebox watermarking approach applies the PGD attack as a base, but with a novel twist. We select a random subset of k labels and aim to increase the logit scores (model's predicted probability) for these labels. The watermark subtly alters the image such that the model's output probabilities for specific target classes are significantly affected, while maintaining the overall visual appearance.

The watermark is applied as an adversarial perturbation δ to the original image x, generated using a targeted adversarial attack to maximize the model's confidence in a set of target classes while keeping the primary classification stable.

Given a pre-trained classifier f(·), an input image x, a true class label y<sub>true</sub>, and a set of target class labels T, our objective function is:

![Arg-max objective function](assets/obj_func.png)
![L objective function](assets/obj_func2.png)

where:
- L<sub>true</sub> ensures the model still classifies the image correctly
- L<sub>target</sub> encourages the model to increase logits for the secret target labels
- λ is a weighting parameter to balance the two objectives

#### Blackbox Attack
In our black-box watermarking approach, we introduce a perturbation that alters the model's output probabilities for a randomly selected subset of k labels without direct access to the model's gradients. This method relies on an iterative query-based optimization process.

Since we don't have access to the model's internal gradients, we:
1. Select k target labels at random
2. Query the model with the original image to obtain baseline logits
4. Update the perturbation to maximize the probability of target labels
5. Scale the perturbation to account for multiple target labels

#### Watermark Verification
For both approaches, we verify the watermark by setting a percentage threshold for the amount of target labels' logit scores that were raised. If a certain percentage of target labels' logit scores increase, we consider the image watermarked. This is expressed as:

![Watermark ver](assets/watermarkver.png)


where:
- P is the proportion of target labels with increased logit scores
- S is the set of all target labels
- Δℓ<sub>i</sub> is the logit difference for target label i
- p is the predefined percentage threshold for verification

For consistency in our experiments, we used k = 4 with secret labels: plane, cardigan, barrow, and Sealyham Terrier. We set ε = 3.5/255 for white-box and ε = 0.7/255 for black-box approaches, with a verification threshold of 50%.

### Results

Our watermarking techniques successfully increased the logit scores of target labels while maintaining visual integrity of the images. The key findings include:

1. **Logit Distribution Analysis**: Both white-box and black-box approaches showed increased logit scores for secret labels after watermarking, while the overall distribution remained stable.
![White-box Distribution](assets/whitebox_logit_histogram_label_190.png)
![Black-box Distribution](assets/blackbox_logit_histogram_label_190.png)
![White-box Overall Distribution](assets/whitebox_combined_histogram.png)
![Black-box Overall Distribution](assets/blackbox_combined_histogram.png)


2. **ROC Curve Analysis**: Target labels showed high AUC values (e.g., Sealyham Terrier with AUC of 0.93), indicating excellent model performance in distinguishing these classes.
![White-box ROC](assets/whitebox_oc_curve_label_190.png)
![Black-box ROC](assets/bb_roc_curve_label_190.png)

3. **Visual Quality**: By selecting appropriate epsilon values, we ensured that perturbations remained subtle, preserving image appearance while effectively raising logit scores.  

**White-box Approach - Before and After Watermarking:**  

![White-box Watermarking](assets/whitebox_comparison.png)  

*Comparison of preprocessing, perturbation noise, and watermarked image*  

**Black-box Approach - Before and After Watermarking:**  

![Black-box Watermarking](assets/blackbox_comparison.png)  

*Comparison of preprocessing, perturbation noise, and watermarked image*  

4. **Resistance to Attacks**: When subjected to the Stable Diffusion Regenerative Attack at various noise steps (0, 20, 40, 80, 160), our watermarked images remained verifiable, though visual quality degraded at higher noise steps.
![White-box Watermark at Different noise steps](assets/wb_noise_steps.png)
![Black-box Watermark at Different noise steps](assets/bb_noise_steps.png)
5. **Performance Comparison**: There was no substantial difference in effectiveness between white-box and black-box approaches, though the white-box method offered more tunable parameters for experimentation.

For optimal performance, we determined that our watermark works better with fewer target labels (lower k) and at specific epsilon values that balance imperceptibility with watermark strength.

### Conclusion

Our PGD-based watermarking method demonstrated effectiveness against regeneration attacks, with performance varying based on image characteristics and target label selection. The approach provides a viable defense mechanism against sophisticated watermark removal techniques, particularly regeneration attacks that typically defeat conventional watermarking methods.

This work lays a foundation for future advancements in watermark-based security measures to protect digital content authenticity in an era of increasingly powerful generative AI. By turning adversarial perturbations from a vulnerability into a security feature, we offer a novel perspective on digital media protection.

### References

- Kurakin, A., Goodfellow, I., & Bengio, S. (2016). "Adversarial machine learning at scale." arXiv preprint arXiv:1611.01236
- Madry, A., Makelov, A., Schmidt, L., Tsipras, D., & Vladu, A. (2017). "Towards deep learning models resistant to adversarial attacks." arXiv preprint arXiv:1706.06083
- Zhao, X., Zhang, K., Su, Z., Vasan, S., Grishchenko, I., Kruegel, C., Vigna, G., Wang, Y.X., & Li, L. (2024). "Invisible Image Watermarks Are Provably Removable Using Generative AI." Cryptography and Security.

### About Us 
Andy Truong 
amt007@ucsd.edu

Anushka Purohit
apurohit@ucsd.edu

Mentor: Yu-Xiang Wang
yuxiangw@ucsd.edu
