# Sora 

* generate videos up to a minute long while maintaining casual quality and adherence to the users prompt.
* generate complex scenes with multiple characters, specific types of motion, and accurate details of the subject and background.
* either forward or backward in time.
* can produce a seamless infinite loop.

# how is sora trained
* Transformer diffusion model
* video compression network
                    input-raw video
                     output- latent representation that is compressed both temporally and spatially.
* turning visual data into patches
* Decoder model-maps generated latents back to pixel space.
* different durations, resolutions and aspect ratios.
* training on data at its native size
              improves compostion and framing
*  Re-captioning from DALLE-3
                     GPT to turn short user prompts into longer detailed captions that are sent to the video model
                      enables sora to generate high quality videos that accurately follow user prompts.

# limitations
* Struggling to accurately simulate complex space
* Understand some instances of cause and effect
* Confuse spatial details of a prompt
* Precise descriptions of events over time



<img width="921" alt="Screenshot 2024-06-25 at 13 17 11" src="https://github.com/usha3211-coder/Research-Development/assets/150019156/e79770a3-91fc-4e81-9891-eec35ff74ca8">

Explanation of Blocks:

1. Text Input: User provides a textual description of the desired video.
2. Embedding Model:The text is processed by an embedding model (e.g., GPT-4) that converts it into a numerical representation (embedding).
3. Diffusion Transformer:The text embedding is fed into the diffusion transformer model, specifically designed for video generation. This model uses the embedding to guide the video generation process.
4. Diffusion Process:
  (a) Latent Noise:An initial latent noise vector is introduced as the starting point for the diffusion process.
  (b) Progressive Denoising: The diffusion transformer progressively refines the video content by denoising the latent noise vector, incorporating the text embedding and attention mechanisms to achieve temporal coherence and frame prediction throughout the sequence.
5.Output Video:The final generated video corresponding to the user's text prompt.
