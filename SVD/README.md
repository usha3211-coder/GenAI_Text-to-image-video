Architecture:- The model is based on a latent video diffusion architecture It inserts temporal convolution and attention layers into a 2D U-Net architecture.
The model is first pretrained on a large video dataset (LVD) using a 3-stage training process:
          1. Image pretraining on a text-to-image diffusion model like Stable Diffusion to get a strong visual representation.
          2. Video pretraining on a large, curated video dataset (LVD-F) at lower resolution.
         3. High-resolution finetuning on a smaller, high-quality video dataset

Datasets:-  use a large, uncurated video dataset called LVD (600 million samples) and curate it using techniques like optical flow, text detection, and CLIP-based aesthetics scoring to create a cleaner dataset called LVD-F. They then finetune the model on a smaller, high-quality dataset of 1 million video samples at higher resolution (576x1024).

Limitations:- common limitations of diffusion models, such as slower inference speed compared to other architectures like GANs.

Accuracy:- The authors report strong performance of their text-to-video model, outperforming previous state-of-the-art methods on the UCF-101 zero-shot text-to-video generation task, with an FVD score of 242.02 compared to previous best of 355.20.They also show that their model outperforms other image-to-video and multi-view generation methods through human preference studies.

<img width="1151" alt="Screenshot 2024-06-25 at 11 33 35" src="https://github.com/usha3211-coder/Research-Development/assets/150019156/b3cbb6b6-bec1-46ce-b7fe-fc7e953a912a">



