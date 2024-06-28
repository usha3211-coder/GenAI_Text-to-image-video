# Lumiere
Lumiere is a diffusion model that was developed by Google AI. It is capable of generating realistic videos from a variety of text prompts, including prompts that are complex and describe abstract concepts. However, it can be slow to generate videos and can sometimes produce artifacts.

* what are the challenges in video generation
   * Existing text2video models
                 * base model generates distant keyframes
                 * subsequent temporal super resolution(TSR) models generate the missing data between the keyframes in 
non-overlapping segments.
* Temporal inconsistency
                * imagen video struggles to generate globally coherent repetitive motion.
                * TSR modules are constrained to fixed, small temporal context windows, and thus cannot consistently resolve aliasing ambiguities across full video.
# To slove this problem
 * Lumiere is a T2V diffusion model for realistic, diverse and coherent motion.
 * space-time U-Net architecture.
 * generates the entire temporal duration of the video at once.
   
# STUNet architecture
  * inflates a pre-trained T2i U-net architecture.
  * Down and up sample the video in both space and time
  * performs majority of its computation on compact space-time representation.
  * cheap temporal CONVs except for most granular layer which has expensive temporal attention.
  * Train newly added parameters and keep the weights of the pre-trained T2i fixed.
  * multi-diffusion along the temporal axis to feed segments of the video to the spatial SR network.
  * temporal windows with overlap of 2 frames and at each denoising step we average the predictions of overlapping pixels, per-segment SSR predictions, Avoids temporal boundary artifacts .

# How is Lumiere model trained

# stylized generation

   Winterpolate = α·Wstyle+(1−α)·Worig
   orig is pre-trained T2I weights.
* Trained on 30M videos along with their text caption.
* 80 frames long at 16fps(2 seconds)
* base model= 128×128
* SSR outputs 1024×1024 frames

# conditional Generation

   * conditioned on image or mask or text
               * Model inputs
                        noisy video J ∈ R T ×H×W×3
                         text prompts
                          masked conditioning video C ∈ R  T ×H×W×3
                           binary mask M ∈ R T ×H×W×1
                * image to video
                           c=first frame followed by blank frames.
                           M=ones for first frame and 0s for the rest of the video
                * Inpainting (object replacement or insertion,l ocalized editing)
                              c=user provided video and a mask M
                *  Cinemagraphs
                               Animating the content of an image only within a specific user-provided region
                               c=input image dupilcated across the entire video,M=ones for first frame and for the other frames, the mask contains ones only outside the user-provided region.


  <img width="976" alt="Screenshot 2024-06-25 at 12 37 20" src="https://github.com/usha3211-coder/Research-Development/assets/150019156/3357a1be-3a45-41d6-854c-6f0f28807967">
                    
Input: Text Prompt
Text Encoder:
  Process: Takes the text prompt and converts it into a latent embedding (numerical representation) capturing the semantic meaning.
  Output: Latent Embedding
Diffusion Model:
    Process:The diffusion model is fine-tuned on a dataset of videos and their corresponding text captions. This dataset is used to train the model to generate low-resolution videos that are temporally coherent and aligned with the text prompts.
  
   * Starts with a noisy video representation.
   * Iteratively refines the video using the latent embedding from the text encoder.
   * Gradually removes noise to create a video aligned with the text prompt.
   Output: Low-resolution video with temporal coherence

Space-Time U-Net (STUNet):
  Process:If high-resolution video is desired, the low-resolution video from the diffusion model is fed into the STUNet.
The STUNet processes the video in a compressed space-time representation.
It downsamples the video in both space (resolution) and time (frame rate) for efficient processing.
Performs computations on the compressed representation.
Upsamples the processed representation back to the original resolution and frame rate.
 Output: High-resolution video with temporal coherence.
 
Spatial Super-Resolution (SSR):
   Process:If even higher resolution is desired, the low-resolution video (from Diffusion Model) or high-resolution video (from STUNet) is fed into the SSR model.
The SSR model refines the video frame-by-frame to achieve even higher resolution.
It applies the refinement on overlapping windows to avoid inconsistencies between segments.
   Output: Very high-resolution video with temporal coherence
   
MultiDiffusion:
  Process:If SSR is used, the video might have inconsistencies at segment boundaries due to separate processing.
MultiDiffusion addresses this by splitting the noisy input video into overlapping segments before the Diffusion Model.
It reconciles the predictions from the SSR model for each segment to ensure a smooth and coherent final video.
 Output: High-quality or very high-resolution video with temporal coherence
 
Final Output: Video clip that adheres to the description provided in the text prompt.

# Reference code
visit https://github.com/lucidrains/lumiere-pytorch

