## :railway_car: TRAM 
Official implementation for the paper: \
**TRAM: Global Trajectory and Motion of 3D Humans from in-the-wild Videos**  
[Yufu Wang](https://yufu-wang.github.io), [Ziyun Wang](https://ziyunclaudewang.github.io/), [Lingjie Liu](https://lingjie0206.github.io/), [Kostas Daniilidis](https://www.cis.upenn.edu/~kostas/)\
[[Project Page](https://yufu-wang.github.io/tram4d/)]

<img src="data/teaser.jpg" width="700">

## Installation
1. Clone this repo with the `--recursive` flag.
```Bash
git clone --recursive https://github.com/yufu-wang/tram
```
2. Creating a new anaconda environment.
```Bash
conda create -n tram python=3.10 -y
conda activate tram
bash install.sh
```
3. Compile DROID-SLAM. If you encountered difficulty in this step, please refer to its [official release](https://github.com/princeton-vl/DROID-SLAM) for more info. In this project, DROID is modified to support masking. 
```Bash
cd thirdparty/DROID-SLAM
python setup.py install
cd ../..
```

## Prepare data
Register at [SMPLify](https://smplify.is.tue.mpg.de) and [SMPL](https://smpl.is.tue.mpg.de), whose usernames and passwords will be used by our script to download the SMPL models. In addition, we will fetch trained checkpoints and an example video. Note that thirdparty models have their own licenses. 

Run the following to fetch all models and checkpoints to `data/`
```Bash
bash scripts/download_models.sh
```

## Run demo on videos
This project integrates the complete 4D human system, including tracking, slam, and 4D human capture in the world space. We separate the core functionalities into different scripts, which should be run **sequentially**. Each step will save its result to be used by the next step. All results will be saved in a folder with the same name as the video.

```bash
# 1. Run detection, segmentation and multi-person tracking
python scripts/detect_track_video.py --video "./example_video.mov" --visualization

# 2. Run Masked DROID-SLAM. 
python scripts/slam_video.py --video "./example_video.mov" --img_focal 600  # if you know the focal (e.g. 600)
# -- or
python scripts/slam_video.py --video "./example_video.mov"  # it will estimate a focal length

# 3. Run 4D human capture with VIMO.
python scripts/vimo_video.py --video "./example_video.mov"

# 4. Put everything together. Render the output video.
python scripts/tram_video.py --video "./example_video.mov"
```
For example, running the above four scripts on the provided video `./example_video.mov` will create a folder `./exapmle_video` and save all results in it.



## Training and evaluation
Code will come soon ...
</br><br>


## Acknowledgements
We benefit greatly from the following open source works, from which we adapted parts of our code.
- [WHAM](https://github.com/yohanshin/WHAM): visualization and evaluation
- [HMR2.0](https://github.com/shubham-goel/4D-Humans): baseline backbone
- [DROID-SLAM](https://github.com/princeton-vl/DROID-SLAM): baseline SLAM
- [ZoeDepth](https://github.com/isl-org/ZoeDepth): metric depth prediction
- [BEDLAM](https://github.com/pixelite1201/BEDLAM): large-scale video dataset
- [EMDB](https://github.com/eth-ait/emdb): evaluation dataset

In addition, the pipeline includes [Detectron2](https://github.com/facebookresearch/detectron2), [Segment-Anything](https://github.com/facebookresearch/segment-anything), and [DEVA-Track-Anything](https://github.com/hkchengrex/Tracking-Anything-with-DEVA).


  
## Citation
```bibtex
@article{wang2024tram,
  title={TRAM: Global Trajectory and Motion of 3D Humans from in-the-wild Videos},
  author={Wang, Yufu and Wang, Ziyun and Liu, Lingjie and Daniilidis, Kostas},
  journal={arXiv preprint arXiv:2403.17346},
  year={2024}
}
```

