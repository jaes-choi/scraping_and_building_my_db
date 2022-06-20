# scraping_and_building_my_db

### 뉴스
``` python
import requests
import re
from bs4 import BeautifulSoup
import pandas as pd

url = 'https://search.naver.com/search.naver?where=news&ie=utf8&sm=nws_hty&query=%EC%82%BC%EC%84%B1%EC%A0%84%EC%9E%90'
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')

for anchor in soup.find_all('a'):
    print(anchor.text, anchor['href'])

for anchor in soup.find_all('a', href=re.compile('n.news.naver.com')):
    print(anchor.text, anchor['href'])
```

### 외인/기관 투자 정보 merge 및 분석
``` python
foreign.columns = ['날짜', '종가', '전일비', '등락률', '거래량', '기관', '외국인', '보유주수', '보유율']

df_sec = pd.read_pickle('../working/df_sec.pkl')
df = df_sec.merge(foreign)

# 전일 종가 컬럼만들기
df['전일'] = df['종가'].shift(-1)

## 파생변수 만들기
# 변동율 데이터 만들기
df['전일비'] = df['종가']/df['전일'] - 1

# 외인과 기관의 거래량 합하기     
df['외인등'] = df['기관_순매매량']+df['외국인_순매매량']

# 외인과 기관의 2일간 거래량 합하기     
df['외인등_2'] = df['외인등'].rolling(2).sum().shift(-1)
df

## DB 테이블로 저장하기

import sqlite3
conn = sqlite3.connect('../working/prices.db')

# if_exists={'replace', 'append'}
foreign.to_sql('f_invest', con=conn, if_exists='replace', index=False)

pd.read_sql("SELECT * FROM sec limit 2", conn)

pd.read_sql("SELECT * FROM f_invest limit 2", conn)

query_txt = '''SELECT * 
               FROM sec a, f_invest b 
               WHERE a.날짜=b.날짜''' 

pd.read_sql(query_txt, conn)
```

```python
bear_song = pd.DataFrame(link_list).iloc[1,2].replace('가','').replace('은','')

for word in bear_song.split():
    if '곰' in word:
        print(word)

bear_song = pd.DataFrame(link_list).iloc[1,2]

import re
re.findall('(\w*[곰]\*?)[가|은]?\W', bear_song)
```

```python
url = 'https://jaes-choi.github.io/Scraping/sample/index.html'
df_k_pop = pd.read_html(requests.get(url).text)[0]
soup = BeautifulSoup(requests.get(url).text, 'html.parser')

df_k_link = pd.DataFrame([anchor['href'] 
                          for anchor in soup.find('table').find_all('a')], 
                         columns=['link'])

pd.concat([df_k_pop,df_k_link], axis=1)
```

```python
url_kids_songs = 'https://jaes-choi.github.io/Scraping/sample/sample_kids_song.html'
pd.read_html(url_kids_songs) 
```

```
import pandas as pd
url = 'https://jaes-choi.github.io/Scraping/sample/index.html'
pd.read_html(url)[0]
```

```
url_kr = 'https://jaes-choi.github.io/Scraping/sample/sample_lang_kr.html'
pd.read_html(url_kr)
```
