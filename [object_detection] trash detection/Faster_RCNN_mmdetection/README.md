# MMdetection Installation
- 가상환경 생성 및 mmdetection 설치 [Tutorial](https://mmdetection.readthedocs.io/en/latest/get_started.html#prepare-environment)

# 1. MMdetection Config
- config 폴더 `SwinT_fasterRCNN_fpn_Config`

## 1.1 Dataset Settings
- `dataset.py`
- 주어진 문제에 맞는 classes 선언
- augmentation 수정 
- batch_size = samples_per_gpu x gpu개수
- train, valid, test pipeline 수정 

## 1.2 Base Model Settings
- `faster_rcnn_r50_fpn.py`
- 주어진 문제에 맞게 num_classes = 10으로 변경
- detector로 Faster RCNN, neck으로 fpn 이용

## 1.3 Runtime Settings
- `default_runtime.py`
- WandbLoggerHook 추가

## 1.4 Optimizer, Learning Schedule Settings
- `schedule_2x.py`
- optimizer 수정
- lr scheduler 수정 
- epoch 설정 
- Cosine Annealing, Adam, warmup 이용

## 1.5 inal Config
- `final_config.py`
- Base Model의 backbone, neck 수정
- evaluation 설정 : bbox_mAP_50 기준 best 모델 저장, 클래스별 validation AP 확인
- checkpoint 설정 : 모델 저장 주기, 경로 지정 


## 2. Train
- config 폴더는 mmdetection/configs/ 아래에 둔다. 
- mmdetection 디렉토리에서 `python tools/train.py {final_config경로} --seed={고정할 시드}`
- ex) `python tools/train.py configs/swinT_fasterRCNN_fpn_config/final_config.py --seed=2021`

## 3. Inference
- mmdetection 디렉토리에서 `python tools/test.py {final_config경로} {저장된모델경로} --work_dir={inference결과저장할경로} --seed={고정할 시드}`
- ex) `python tools/test.py configs/swinT_fasterRCNN_fpn_config/final_config.py work_dir/fasterRCNN_result/epoch_32.pth --work_dir=work_dir/fasterRCNN_result --seed=2021`
