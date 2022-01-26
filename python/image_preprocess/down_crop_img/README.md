
# 1. 기능

### 1) 이미지의 url 주소를 받아와 png 파일로 로컬에 저장
- 파일명 : csv 파일에 기재된 순서가 이름이 됩니다
- 예시 : 0.png, 1.png

### 2) 원본 이미지파일의 중심을 기준으로 정방형으로 자른 후  새로 저장
- 파일명 : 원본 파일명 앞에 ‘cropped_’가 붙습니다
- 예시 : cropped_0.png, cropped_1.png
---
# 2. 사용방법


    🚨 python 버전 3 이상으로 실행해야 합니다!!


### 1) 이미지 url들만 담겨있는 csv 파일을 준비합니다.
- 열과 행에 대한 header도 index아무것도 없이 그냥 url만 담겨있는 파일이어야 합니다.
    
    
### 2) 파이썬 라이브러리 2개를 먼저 설치합니다.
- `pip install pandas`
- `pip install PIL`

### 3) 터미널에서 이미지를 저장할 디렉토리로 이동합니다.
### 4) 3번 디렉토리로 `down_crop_img.py` 모듈을 옮깁니다.
### 5) 터미널에서 `python down_crop_img.py` 를 실행합니다.
### 6) 실행 직후 입력 안내 메시지가 뜨면, 1번의 csv파일 경로를 입력해줍니다.
 > 
### 7) 실행완료 메시지를 보며 전부 처리 완료될 때까지 기다립니다.
- 완료메시지 예시 : `----- just saved 1/2 ------`
- 앞숫자 : 처리 완료한 파일 수
- 뒷숫자 : 처리 해야할 전체 파일 수
---

# 3. 코드 & 주석

```python
import pandas as pd
import urllib.request
from PIL import Image

# 1. csv 파일의 경로를 받아와 변수에 저장
urllist_csv = input("이미지 url 리스트가 담긴 csv 파일 경로를 입력해주세요")

# 2. csv 파일의 데이터를 list형으로 변환
urllist_df = pd.read_csv(urllist_csv, header=None)
urllist = urllist_df[0].values.tolist()

"""
=======test용 url===========
url1 = "https://images.unsplash.com/photo-1512621776951-a57141f2eefd?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2670&q=80"
url2 = "https://images.unsplash.com/photo-1607532941433-304659e8198a?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1556&q=80"
urllist = [url1, url2]
===========================
"""

# 3. url로 img를 다운 받고, 'index.png' 파일로 저장하는 함수
# 반드시 enumerate로 돌린 for문 안에서 써야 index값을 이름으로 넣을 수 있음

def url_to_img(url, idx):
  img = urllib.request.urlretrieve(url, "./{}.png".format(idx))
  return img

# 4. url_to_img()로 저장된 원본 이미지 파일을 불러와 정방형으로 crop한 뒤
# 'cropped_index.png' 이름으로 저장하는 함수
# 반드시 enumerate로 돌린 for문 안에서 써야 이름을 받아 올 수 있음
def crop_center(img, name):
  x, y = img.width, img.height
  sx = x // 2-(min(x, y) // 2)
  sy = y // 2-(min(x, y) // 2)
  img = img.crop((sx, sy, sx+min(x, y), sy+min(x, y)))
  
  return img.save('./{}.png'.format(name), 'png')

## 5. 위 두 함수를 사용해서 urllist에 있는 이미지를 처리하는 for문
for url in enumerate(urllist):
  url_to_img(url[1], url[0])
  name = ('{}.png'.format(url[0]))
  img = Image.open('./{}'.format(name))
  crop_center(img, 'cropped_{}'.format(name))
  print('----{0}/{1} have saved!!----'.format(url[0]+1, len(urllist)))
```
