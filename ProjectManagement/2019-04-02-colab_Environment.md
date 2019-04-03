# **Tacotron English Dataset_COLAB_Environment** 

# 1. COLAB/Google Drive 환경 구축

  1-1. Git fork and clone [tacotron](https://github.com/keithito/tacotron)

  1-2. COLAB 런타임 유형 -> 하드웨어 가속기 = GPU
  
  1-3. tensorflow 버전확인(1.13.1 사용)
  
      import tensorflow as tf
      tf.__version__

  1-4. tensorflow GPU 사용 확인 코드
 
      device_name = tf.test.gpu_device_name()
      if device_name != '/device:GPU:0':
        raise SystemError('GPU device not found')
      print('Found GPU at : {}'.format(device_name))
	
  1-5. 필요 모듈 재설치
  
      !pip install falcon==1.2.0
      
      !pip install inflect==0.2.5
      
      !pip install librosa==0.5.1
      
      !pip install tqdm==4.11.2
      
      !pip install Unidecode==0.4.20

  1-6. 구글 드라이브 홈/Colab Notebooks/tacotron/에 소스코드 및 음성파일 업로드
 	
  1-7. COLAB에 구글 드라이브 마운트
  
      from google.colab import drive
      drive.mount('/content/gdrive')
      
  1-8 시스템 path에 파이썬 모듈 및 파이썬 파일 위치 추가
  
      import sys
      sys.path.insert(0, '/content/gdrive/My Drive/Colab Notebooks/tacotron/')
      
  1-9 파이썬 스크립트 실행을 위해 현재 경로 변경 및 확인
  
      import os
      os.chdir("/content/gdrive/My Drive/Colab Notebooks/tacotron/")
      
      !pwd
  
# 2. Preprocessing
 
  2-1. Download a speech dataset. 
   The following are supported out of the box: <br>
   [LJ Speech (Public Domain)](https://keithito.com/LJ-Speech-Dataset/)

  2-2. Unpack the dataset into ~/tacotron

  2-3. Preprocess the data : 
	 
		!python3 preprocess.py --dataset ljspeech
       
※ FileNotFoundError : No such file or directory 에러 발생 시:
  
- process.py : os.path.expanduser 경로 수정!
    
    	def main():
      	parser.add_argument('--base_dir', default=os.path.expanduser('/content/gdrive/My Drive/Colab Notebooks/tacotron/'))

- train.py : os.path.expanduser 경로 수정!
    
    	def main():
      	parser.add_argument('--base_dir', default=os.path.expanduser('/content/gdrive/My Drive/Colab Notebooks/tacotron/'))


	
