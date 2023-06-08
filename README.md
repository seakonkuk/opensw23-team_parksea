# opensw23 team project

# Team Introduction
- 박세아 (201911253)
  - 역할 : 전부




# Topic Introduction
### Swin-Conv-UNet과 데이터 합성을 통한 실용적인 블라인드 노이즈 제거

입력: SCUNet의 입력은 노이즈가 있는 이미지입니다. 이 이미지는 네트워크에 통과되어 노이즈가 제거되는 과정을 거칩니다.

출력: 네트워크를 통과한 후, 출력은 노이즈가 제거된 이미지입니다.

### Swin-Conv-UNet (SCUNet) denoising network

![arch_scunet](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/7239167f-e38b-4797-bd65-0d80fca61ca3)
제안된 Swin-Conv-UNet (SCUNet) 노이즈 제거 네트워크의 구조는 Swin-Conv (SC) 블록을 UNet 구조의 주요 구성 요소로 활용합니다.

각 SC 블록에서, 입력은 먼저 1x1 합성곱에 통과시키고, 그 다음 두 개의 피쳐 맵 그룹으로 균등하게 분할합니다. 이 분할된 각 그룹은 Swin Transformer (SwinT) 블록과 잔여 3x3 합성곱 (RConv) 블록으로 각각 전달됩니다.

그 이후에, SwinT 블록과 RConv 블록의 출력들은 결합되고, 다시 1x1 합성곱을 통과하여 입력의 잔여값을 생성합니다.

"SConv"와 "TConv"는 각각 stride 2를 가진 2x2 strided convolution과 stride 2를 가진 2x2 transposed convolution을 나타냅니다.

간단히 말해서, 이 네트워크는 UNet의 구조를 사용하면서도, 각 블록에서는 Swin Transformer와 residual convolution을 이용하여 입력 피쳐를 더욱 효과적으로 처리하고 변환하는 구조를 가지고 있습니다.

### New data synthesis pipeline for real image denoising
![pipeline_scunet](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/8df2104d-4044-431f-90bf-ce116ff67da0)
제안된 페어드 훈련 패치 합성 파이프라인(paired training patch synthesis pipeline)은 고품질 이미지에 무작위로 섞인 훼손 시퀀스(degradation sequence)를 적용하여 노이즈가 있는 이미지를 생성하고, 크기를 조정하고 역-정방향 톤 매핑(reverse-forward tone mapping)을 적용하여 대응하는 클린 이미지를 생성합니다. 

이 후 노이즈가 있는 이미지와 클린 이미지의 페어드 트레이닝 패치를 생성하여 심층 블라인드 노이즈 제거 모델(deep blind denoising model) 훈련에 사용합니다. 

클린 이미지는 포아송 노이즈(Poisson noise)를 생성하고, 색상 이동 문제(color shift issue)를 해결하기 위해 역-정방향 톤 매핑이 수행됩니다.

![data_scunet](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/fc478851-3c80-439a-90a7-95a9f29ad79a)

제안된 훈련 데이터 합성 파이프라인을 통해 합성된 노이즈가 있는/클린 패치 쌍에 대해 설명합니다. 

고품질 이미지 패치의 크기는 544x544이며, 노이즈가 있는/클린 패치의 크기는 128x128입니다.




# Results
### Gaussian denoising - grayscale image
#### input image
![sample1](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/c208ce88-0ced-4c21-bdd1-71133343712b)

#### output image
![sample1](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/1e0047fb-c910-4472-8b1b-453fdf3f8a87)


### Gaussian denoising - color images
#### input image
![sample6](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/3f5ae7bb-a70b-4e9c-b45b-e5fe474b0980)

#### output image
![sample6](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/8eb4a481-7fd8-477f-bd7c-8ecdc0d9300d)


### Blind real image denoising
#### input image
![sample3](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/5f4d2cb9-4e82-47cc-9b05-500bc6a64c26)


#### output image
![sample3](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/8edcbb4c-3326-4095-bfe8-cae9df5cf880)





# Analysis/Visualization
## Gaussian Denoising - gray scale image
### 성공 사례
#### input image
![sample1](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/cd3ae481-68a7-470a-8a16-a1e8b183eb21)

#### output image
![sample1](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/f5c01591-1d74-4d0c-9145-a42d97e5ecdf)


### 실패 사례
#### input image
![sample2](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/1df5815c-8e14-43e9-b704-d2b7574e9bf8)

#### output image
![sample2](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/21f5e956-4572-48ff-8b10-31024068f5d9)

### 결과 평가 
가우시안 노이즈 및 휘도 노이즈가 추가된 이미지에서는 노이즈가 잘 제거된다.
하지만, 이외의 Salt-and-pepper 노이즈가 추가된 이미지에서는 노이즈가 제거되지 않는 결과를 볼 수 있다.


## Gaussian Denoising - color image
### 성공 사례
#### input image
![sample6](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/0b65f3ab-add5-4953-bab1-aa8cd0bfc4be)

#### output image
![sample6](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/3935c2d9-7b37-4ee5-8bc8-c3852fe156cf)


### 실패 사례
#### input image
![sample8](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/4f6242d0-8669-4e12-b56a-4896abd73c18)


#### output image
![sample8](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/b5f2ae71-4147-4f55-8b51-8a201b3c3a58)


### 결과 평가 
컬러 이미지에서도 흑백 이미지와 동일하게 가우시안 노이즈 및 휘도, 컬러 노이즈가 추가된 이미지에서는 노이즈가 잘 제거된 결과를 볼 수 있다.
하지만, 이외의 Salt-and-pepper 노이즈, 샷 노이즈, 광도 이미지가 추가된 이미지에서는 노이즈가 제거되지 않는 결과를 볼 수 있다.



## Blind real image denoising

#### input image
![sample5](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/591bb8fe-624a-4107-8fbd-42cb8498e25b)


#### output image
![sample5](https://github.com/seakonkuk/opensw23-team_parksea/assets/57178250/7dfcb2f6-ed55-41ec-badb-e6c231cdb203)


### 결과 평가
해당 모델은 대부분의 샘플 이미지에서 좋은 결과를 나타낸다.
다양한 노이즈 제거는 물론, 저화질이 고화질로 잘 향상되는 결과를 볼 수 있다. 
작은 사이즈의 이미지는 물론, 큰 사이즈의 이미지에서도 결과가 잘 나오는 것을 알 수 있다. 


# Installation
세팅 환경 : 맥 os 

### Clone
해당 깃 repository를 clone한 후, 해당 폴더로 이동한다.
<pre>
<code>git clone https://github.com/seakonkuk/opensw23-team_parksea.git
cd opensw23-team_parksea</code>
</pre>

### Download SCUNet models
<pre>
<code>python3 main_download_pretrained_models.py --models "SCUNet" --model_dir "model_zoo"</code>
</pre>

### Gaussian denoising

  #### grayscale images
  처리하고 싶은 이미지들을 testsets 폴더 내의 set12 폴더 안에 저장한다.
<pre>
<code>python3 main_test_scunet_gray_gaussian.py --model_name scunet_gray_25 --noise_level_img 25 --testset_name set12</code>
</pre>
  
  #### Gaussian denoising (grayscale images) output
  처리된 결과 이미지는 results 내의 set12_scunet_gray_25 폴더 내에 저장된다.
  
  #### color images
  처리하고 싶은 이미지들을 testsets 폴더 내의 bsd68 폴더 안에 저장한다.
<pre>
<code>python3 main_test_scunet_color_gaussian.py --model_name scunet_color_25 --noise_level_img 25 --testset_name bsd68</code>
</pre>

  #### Gaussian denoising (color images) output
  처리된 결과 이미지는 results 내의 bsd68_scunet_color_25 폴더 내에 저장된다.

  #### Blind real image denoising
  처리하고 싶은 이미지들을 testsets 폴더 내의 real3 폴더 안에 저장한다.
<pre>
<code>python3 main_test_scunet_real_application.py --model_name scunet_color_real_psnr --testset_name real3</code>
</pre>

  #### Blind real image denoising output
  처리된 결과 이미지는 results 내의 real3_scunet_color_real_psnr 폴더 내에 저장된다.

  
  
  
  
# Presentation
