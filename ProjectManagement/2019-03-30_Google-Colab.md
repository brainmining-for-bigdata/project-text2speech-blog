### 런타임 유형변경 메뉴에서 GPU선택(한개의 VM 12시간 사용)
### GPU확인 (11.4giga)
```
!nvidia-smi
Mon Mar 25 11:42:28 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.43       Driver Version: 410.79       CUDA Version: 10.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla K80           Off  | 00000000:00:04.0 Off |                    0 |
| N/A   34C    P0    57W / 149W |      0MiB / 11441MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```
### cpu확인/메모리 확인 /디스크 확인/OS확인 
* 메모리 13G/cpu 2.3GHz/disk 21G/Ubuntu 18.04

```
!cat /proc/meminfo
!cat /proc/cpuinfo
!df -h
!cat /etc/issue.net
```

### colab에서 google driver 사용법
* 아래 커맨드를 치면 링크가 나온다. 링크를 클릭하면 auth key를 copy하여 붙여넣으면 된다. 
```
from google.colab import drive
drive.mount('/content/gdrive')
```
* gdrive가 mount되었는 지 확인 
```
!ls
gdrive	sample_data
```
* gdrive에서 파일을 읽는다. 
```
petitions = pd.read_csv('gdrive/My Drive/Colab Notebooks/petitionWrangling/data/petition.csv', 
                        index_col=0,
                        parse_dates=['start', 'end'])
```
### local에서 파일 읽어오기
```
# google colaboratory가 아닌 다른 python 프로그램이라면, 바로 read_csv

from google.colab import files
uploaded = files.upload() # 파일 업로드 기능 실행

for fn in uploaded.keys(): # 업로드된 파일 정보 출력
    print('User uploaded file "{name}" with length {length} bytes'.format(
        name=fn, length=len(uploaded[fn])))
```
파일선택하는 메뉴에서 파일 선택
```
sal = pd.read_csv(io.StringIO(uploaded['petition.csv'].decode('utf-8')))
```
### 한글 사용법
```
!apt-get update -qq
!apt-get install fonts-nanum* -qq

# 그래프를 노트북 안에 그리기 위해 설정
%matplotlib inline

# 필요한 패키지와 라이브러리를 가져옴
import matplotlib as mpl
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm

fm._rebuild()
# 그래프에서 마이너스 폰트 깨지는 문제 처리 
mpl.rcParams['axes.unicode_minus'] = False
plt.rc('font', family='NanumGothic Eco')
```
### jupyter notebook을 markdown으로 변환
```
!jupyter nbconvert --to markdown 'gdrive/My Drive/Colab Notebooks/crawling_ex_gas_station.ipynb'
```
### version확인
```
import tensorflow as tf
tf.__version__

import sys
sys.version
```


###  참조 
https://zzsza.github.io/data/2018/08/30/google-colab/
