# **Tacotron English Dataset_COLAB_Server_with_GPU_no_gdrive** 

# 1. COLAB 환경 구축
  
  1-1. 런타임 유형 변경
  
  런타임 -> 런타임 유형 변경 -> GPU
  
  1-2. 텐서플로우 버전 확인 및 텐서플로우-GPU 테스트 코드 실행
  
      import tensorflow as tf
      tf.__version__
      
      import timeit
      # See https://www.tensorflow.org/tutorials/using_gpu#allowing_gpu_memory_growth
      config = tf.ConfigProto()
      config.gpu_options.allow_growth = True

      with tf.device('/cpu:0'):
        random_image_cpu = tf.random_normal((100, 100, 100, 3))
        net_cpu = tf.layers.conv2d(random_image_cpu, 32, 7)
        net_cpu = tf.reduce_sum(net_cpu)

      with tf.device('/gpu:0'):
        random_image_gpu = tf.random_normal((100, 100, 100, 3))
        net_gpu = tf.layers.conv2d(random_image_gpu, 32, 7)
        net_gpu = tf.reduce_sum(net_gpu)

      sess = tf.Session(config=config)

      # Test execution once to detect errors early.
      try:
        sess.run(tf.global_variables_initializer())
      except tf.errors.InvalidArgumentError:
        print(
            '\n\nThis error most likely means that this notebook is not '
            'configured to use a GPU.  Change this in Notebook Settings via the '
            'command palette (cmd/ctrl-shift-P) or the Edit menu.\n\n')
        raise

      def cpu():
        sess.run(net_cpu)

      def gpu():
        sess.run(net_gpu)

      # Runs the op several times.
      print('Time (s) to convolve 32x7x7x3 filter over random 100x100x100x3 images '
            '(batch x height x width x channel). Sum of ten runs.')
      print('CPU (s):')
      cpu_time = timeit.timeit('cpu()', number=10, setup="from __main__ import cpu")
      print(cpu_time)
      print('GPU (s):')
      gpu_time = timeit.timeit('gpu()', number=10, setup="from __main__ import gpu")
      print(gpu_time)
      print('GPU speedup over CPU: {}x'.format(int(cpu_time/gpu_time)))

      sess.close()
  
  1-3. requirements 설치
  
        !pip install falcon==1.2.0
        !pip install inflect==0.2.5
        !pip install librosa==0.5.1
        !pip install matplotlib==2.0.2
        !pip install numpy==1.14.3
        !pip install scipy==0.19.0
        !pip install tqdm==4.11.2
        !pip install Unidecode==0.4.20
  
  1-4. Colab 서버로 Git clone [개인 레파지토리](https://github.com/elephamaximu/tacotest.git)
  
        !git clone https://github.com/elephamaximu/tacotest.git
        
        # 코드 clone 하기 전, process.py/tarin.py 속에서 디렉토리 경로를 cloab 서버 상황에 맞게 변경해주어야 함
        # 위 레파지토리를 클론하면 폴더명이 tacotest로 생성되므로 코드 속 디렉토리 경로는
        
        # process.py : os.path.expanduser 경로 수정!
          def main():
      	    parser.add_argument('--base_dir', default=os.path.expanduser('/content/tacotest/'))

        # train.py : os.path.expanduser 경로 수정!
          def main():
      	    parser.add_argument('--base_dir', default=os.path.expanduser('/content/tacotest/'))
        
      
  1-5 시스템 path에 파이썬 모듈 및 파이썬 파일 위치 추가
  
      import sys
      sys.path.insert(0, '/content/tacotest/')
      
  1-6 파이썬 스크립트 실행을 위해 현재 경로 변경 및 확인
  
      import os
      os.chdir("/content/tacotest/")
      
      !pwd
      !ls -al
      
   1-7 wget으로 현위치(/content/tacotest/)에 LJSpeech tar.bz2 파일 다운로드
   
        !wget https://data.keithito.com/data/speech/LJSpeech-1.1.tar.bz2
        
   1-8 tar로 현위치(/content/tacotest/)에 LJSpeech tar.bz2 파일 압축 해제
        
        !tar -xvf LJSpeech-1.1.tar.bz2      
      
# 2. Preprocessing
 
  2-1. Preprocess the data : 
	 
	!python preprocess.py --dataset ljspeech
       
# 3. Training

      !python train.py
    
# 4. Colab에서 tensorboard 활용 및 트레인 모델 Synthesizing 하기

  4-1 tensorboard 활용
  
  	LOG_DIR = './logs-tacotron'
  	get_ipython().system_raw(
    		'tensorboard --logdir {} --host 0.0.0.0 --port 6006 &'
    		.format(LOG_DIR)
	)
	
	!wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
	
	!unzip ngrok-stable-linux-amd64.zip
	
	get_ipython().system_raw('./ngrok http 6006 &')
    
  4-2 Synthesize

# 5. Colab GPU 런타임 12시간 경과후 파일 소멸 관련 해결책

  5-1 트레인 모델 깃허브 레파지토리에 푸시하기
  
  	# 개인 레파지토리에서 코드를 git clone한 상태로 진행
	
	# git 상태 확인
	!git status
	
	# log파일 스테이지에 올리기
	!git add --all
	
	# 스테이지 올라간 파일 중에서 커밋 안할 파일 빼기
	!git reset HEAD 해당폴더명/해당파일명
	
	# 용량 많은 파일 github에 분산 전송할 프로그램(git-lfs) 설치하기
	!apt-get install git-lfs
	
	# 현재 로컬 레파지토리를 git-lfs로 initialize 하기
	!git lfs install
	
	# 용량 많을 파일 git-lfs추적하기
	!git lfs track '폴더명/**'
	
	# 커밋하기전 colab 로컬 서버에 사용자 등록하기
	!git config --global user.name "plato"
	!git config --global user.email "elephamaximu@gmail.com"
	
	# 커밋하기
	!git commit -m "커밋메시지"
	
	# colab 로컬 서버에 깃허브 리모트 레파지토리 계정 정보 넣어주기
	!git remote -v set-url origin https://아이디:패스워드@github.com/elephamaximu/tacotest.git
	
	# 깃허브 리모트 레파지토리에 푸시하기
	!git push -u origin master

# 6. 체크포인트에서 train 이어하기

	!python train.py --restore_step=9000
