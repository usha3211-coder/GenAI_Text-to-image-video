# Architecture:- 
   The model is based on a latent video diffusion architecture It inserts temporal convolution and attention layers into a 2D U-Net architecture.
The model is first pretrained on a large video dataset (LVD) using a 3-stage training process:
          1. Image pretraining on a text-to-image diffusion model like Stable Diffusion to get a strong visual representation.
          2. Video pretraining on a large, curated video dataset (LVD-F) at lower resolution.
          3. High-resolution finetuning on a smaller, high-quality video dataset

# Datasets:-  
   use a large, uncurated video dataset called LVD (600 million samples) and curate it using techniques like optical flow, text detection, and CLIP-based aesthetics scoring to create a cleaner dataset called LVD-F. They then finetune the model on a smaller, high-quality dataset of 1 million video samples at higher resolution (576x1024).

# Limitations:- 
  common limitations of diffusion models, such as slower inference speed compared to other architectures like GANs.

# Accuracy:- 
   The authors report strong performance of their text-to-video model, outperforming previous state-of-the-art methods on the UCF-101 zero-shot text-to-video generation task, with an FVD score of 242.02 compared to previous best of 355.20.They also show that their model outperforms other image-to-video and multi-view generation methods through human preference studies.

<img width="1151" alt="Screenshot 2024-06-25 at 11 33 35" src="https://github.com/usha3211-coder/Research-Development/assets/150019156/31bbc243-3291-4bcb-a175-5fd8578c7d2a">




Input: Text prompt
1. Text Preprocessing
Description: Splits the text description into individual words or phrases (tokens).
Embedding (CLIP): Converts tokens (or entire description) into numerical representations using CLIP, capturing semantic meaning.
2. Text-to-Image Encoder (Optional)
Description: This block (potentially a pre-trained image generation model) translates the CLIP embedding into an initial latent image representation, capturing some visual aspects based on the text.
3. Video Training Stage
Input:CLIP Embedding (if Text-to-Image Encoder not used): Directly feeds the semantic meaning of the text description.
Latent Image Representation (if Text-to-Image Encoder used): Provides an initial visual representation based on the text.
Temporal Encoder (Description): Processes the input to extract and encode information relevant for video generation, including:
Capturing the essence of the text description or the initial visual representation (objects, actions, relationships).
Adding temporal awareness by analyzing inherent temporal information or relationships.
Output: Encoded representation suitable for video generation.
4. U-Net with Spatial and Temporal Attention
Description: Processes the encoded representation from the temporal encoder, incorporating both:
Spatial information (what's in each frame).
Temporal information (how things change over time) ensured by:
Temporal Convolution & Attention Layers strategically inserted within the U-Net.
CLIP Embedding Integration: Throughout the U-Net, the CLIP embedding is used to condition the video generation process, ensuring the video aligns with the text description.
Output: Latent video representation containing a sequence of encoded frames.
5. Noise Injection (Forward Diffusion) (Training Only)
Description: Progressively adds noise to the latent video representation during training to improve model robustness and ability to generate diverse and realistic videos. (Not used for video generation)
6. Video Decoder
Description: Decodes the final latent video representation back into a sequence of images, forming the video based on the text description.
Output: Generated Video
7. Fine-tuning Stage
Description: Further refines the model for improved video quality using:
High-Quality Video Input: High-quality video data used for fine-tuning.
Fine-tune U-Net: The U-Net with spatial and temporal attention is fine-tuned on the high-quality video data.
Output: Enhanced Model for Video Generation

# Reference code
visit https://github.com/nateraw/stable-diffusion-videos
