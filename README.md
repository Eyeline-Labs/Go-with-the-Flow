<!-- # Go-with-the-Flow: Motion-Controllable Video Diffusion Models Using Real-Time Warped Noise -->

<p align="center">
  <img src="assets/Logo.png" alt="Go-with-the-Flow: Motion-Controllable Video Diffusion Models Using Real-Time Warped Noise" width="100%">
</p>

<h1> Accepted to CVPR 2025 as Oral </h1>

[![Project Page](https://img.shields.io/badge/Project-Page-green?logo=googlechrome&logoColor=green)](https://eyeline-research.github.io/Go-with-the-Flow/)
[![Paper](https://img.shields.io/badge/Paper-arXiv-b31b1b?logo=arxiv&logoColor=red)](https://arxiv.org/abs/2501.08331)
[![YouTube Tutorial](https://img.shields.io/badge/YouTube-Tutorial-red?logo=youtube&logoColor=red)](https://www.youtube.com/watch?v=IO3pbQpT5F8)
[![Hugging Face](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Go--with--the--Flow-blue)](https://huggingface.co/Eyeline-Research/Go-with-the-Flow/tree/main)

[Ryan Burgert](https://ryanndagreat.github.io)<sup>1,3</sup>, [Yuancheng Xu](https://yuancheng-xu.github.io)<sup>1,4</sup>, [Wenqi Xian](https://www.cs.cornell.edu/~wenqixian/)<sup>1</sup>, [Oliver Pilarski](https://www.linkedin.com/in/oliverpilarski/)<sup>1</sup>, [Pascal Clausen](https://www.linkedin.com/in/pascal-clausen-a179566a/?originalSubdomain=ch)<sup>1</sup>, [Mingming He](https://mingminghe.com/)<sup>1</sup>, [Li Ma](https://limacv.github.io/homepage/)<sup>1</sup>, 

[Yitong Deng](https://yitongdeng.github.io)<sup>2,5</sup>, [Lingxiao Li](https://scholar.google.com/citations?user=rxQDLWcAAAAJ&hl=en)<sup>2</sup>, [Mohsen Mousavi](www.linkedin.com/in/mohsen-mousavi-0516a03)<sup>1</sup>, [Michael Ryoo](http://michaelryoo.com)<sup>3</sup>, [Paul Debevec](https://www.pauldebevec.com)<sup>1</sup>, [Ning Yu](https://ningyu1991.github.io)<sup>1†</sup>

<sup>1</sup>Netflix Eyeline Studios, <sup>2</sup>Netflix, <sup>3</sup>Stony Brook University, <sup>4</sup>University of Maryland, <sup>5</sup>Stanford University<br>
<sup>†</sup>Project Lead

### Table of Contents
- [Abstract](#abstract) 
- [Quick Start: Cut-and-drag Motion Control](#quick-start-cut-and-drag-motion-control)
  - [Animation Template GUI (Local)](#1-animation-template-gui-local) 
  - [Running Video Diffusion (GPU)](#2-running-video-diffusion-gpu)
- [TODO](#todo)  
- [Citation](#citation)
- [Acknowledgement](#acknowledgement)

<a name="abstract"></a>
## :book: Abstract 

Go-with-the-Flow is an easy and efficient way to control the motion patterns of video diffusion models. It lets a user decide how the camera and objects in a scene will move, and can even let you transfer motion patterns from one video to another.

We simply fine-tune a base model — requiring no changes to the original pipeline or architecture, except: instead of using pure i.i.d. Gaussian noise, we use **warped noise** instead. Inference has exactly the same computational cost as running the base model.

If you create something cool with our model - and want to share it on our website - email rburgert@cs.stonybrook.edu. We will be creating a user-generated content section, starting with whomever submits the first video! 

If you like this project, please give it a ★!

<a name="community-adoption"></a>
## :fire: Community Adoption

A huge thank you to all who contributed - videos to be added here soon!

- [ComfyUI implementation](https://github.com/kijai/ComfyUI-VideoNoiseWarp) by @kijai
- [HuggingFace Space #1](https://huggingface.co/spaces/fffiloni/Go-With-The-Flow) by fffiloni
- [HuggingFace Space #2](https://huggingface.co/spaces/OneOverZero/Go-With-The-Flow) by Ryan Burgert
- [AnimateDiff Implementation](https://huggingface.co/spacepxl/Go-with-the-Flow-AD-converted/tree/main) by spacepxl
- [HunyuanVideo Implementation](https://huggingface.co/spacepxl/HunyuanVideo-GoWithTheFlow-unofficial) by spacepxl
- [Cut-and-drag using SAMv2](https://github.com/Pablerdo/hexaframe-dark) and its [web interface](https://hexaframe-dark.vercel.app/) by Pablo Salamanca
- [Japanese Tutorial](https://youtu.be/n0NT-sltRK0) by Takamasa Tamura

Examples:

<p align="center">
  <img src="assets/cut_and_drag_example_1.gif" width="80%">
  <!-- <img src="assets/cut_and_drag_example_2.gif" width="80%"> -->
  <img src="assets/cut_and_drag_example_3.gif" width="80%">
  <img src="assets/cut_and_drag_example_4.gif" width="80%">
  <img src="assets/cut_and_drag_example_5.gif" width="80%">
</p>

<a name="quick-start-cut-and-drag-motion-control"></a>
## :rocket: Quick Start: Cut-and-drag Motion Control

Cut-and-drag motion control lets you take an image, and create a video by cutting out different parts of that image and dragging them around.

For cut-and-drag motion control, there are two parts: an GUI to create a crude animation (no GPU needed), then a diffusion script to turn that crude animation into a pretty one (requires GPU).

**YouTube Tutorial**: [![YouTube Tutorial](https://img.shields.io/badge/YouTube-Tutorial-red?logo=youtube&logoColor=red)](https://www.youtube.com/watch?v=IO3pbQpT5F8)

<a name="1-animation-template-gui-local"></a>
### 1. Animation Template GUI (Local)

1. Clone this repo, then `cd` into it.  
2. Install local requirements:

    `pip install -r requirements_local.txt`

3. Run the GUI:

    `python cut_and_drag_gui.py`

4. Follow the instructions shown in the GUI.  

After completion, an MP4 file will be generated. You'll need to move this file to a computer with a decent GPU to continue.

<a name="2-running-video-diffusion-gpu"></a>
### 2. Running Video Diffusion (GPU)

1. Clone this repo on the machine with the GPU, then `cd` into it.  
2. Install requirements:

    `pip install -r requirements.txt`

3. Warp the noise (replace `<PATH TO VIDEO OR URL>` accordingly):

    `python make_warped_noise.py <PATH TO VIDEO OR URL> --output_folder noise_warp_output_folder`

4. Run inference:

    ```
    python cut_and_drag_inference.py noise_warp_output_folder \
        --prompt "A duck splashing" \
        --output_mp4_path "output.mp4" \
        --device "cuda" \
        --num_inference_steps 30
    ```

Adjust folder paths, prompts, and other hyperparameters as needed. The output will be saved as `output.mp4`.

<a name="todo"></a>
## :clipboard: TODO

- [x] Upload All CogVideoX Models
- [x] Upload Cut-And-Drag Inference Code  
- [x] Release to Arxiv
- [ ] Depth-Warping Inference Code
- [x] T2V Motion Transfer Code
- [x] I2V Motion Transfer Code (allows for first-frame editing)
- [x] ComfyUI Node
- [ ] Release 3D-to-Video Inference Code + Blender File
- [x] Upload AnimateDiff Model
- [ ] Replicate Instance
- [ ] Fine-Tuning Code

<a name="citation"></a>
## :black_nib: Citation

If you use this in your research, please consider citing:

```
@inproceedings{burgert2025gowiththeflow,
  title={Go-with-the-Flow: Motion-Controllable Video Diffusion Models Using Real-Time Warped Noise},
  author={Burgert, Ryan and Xu, Yuancheng and Xian, Wenqi and Pilarski, Oliver and Clausen, Pascal and He, Mingming and Ma, Li and Deng, Yitong and Li, Lingxiao and Mousavi, Mohsen and Ryoo, Michael and Debevec, Paul and Yu, Ning},
  booktitle={CVPR},
  year={2025},
  note={Licensed under Modified Apache 2.0 with special crediting requirement}
}
```

## :scroll: License

This project is licensed under a Modified Apache License 2.0. While it is based on the standard Apache License, it includes an additional condition (Section 10) that requires anyone using this work to create videos in a motion picture, film, or any production with credits to include all authors of this paper in those credits.

<!-- ``` -->
<!-- @misc{burgert2025gowiththeflowmotioncontrollablevideodiffusion, -->
<!--       title={Go-with-the-Flow: Motion-Controllable Video Diffusion Models Using Real-Time Warped Noise},  -->
<!--       author={Ryan Burgert and Yuancheng Xu and Wenqi Xian and Oliver Pilarski and Pascal Clausen and Mingming He and Li Ma and Yitong Deng and Lingxiao Li and Mohsen Mousavi and Michael Ryoo and Paul Debevec and Ning Yu}, -->
<!--       year={2025}, -->
<!--       eprint={2501.08331}, -->
<!--       archivePrefix={arXiv}, -->
<!--       primaryClass={cs.CV}, -->
<!--       url={https://arxiv.org/abs/2501.08331},  -->
<!-- } -->
<!-- ``` -->

<a name="acknowledgement"></a>
## :thumbsup: Acknowledgement

We express gratitudes to the [CogVideoX](https://github.com/THUDM/CogVideo) and [RAFT](https://github.com/princeton-vl/RAFT) repositories as we benefit a lot from their code.
