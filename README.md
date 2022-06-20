# scraping_and_building_my_db

### 컬럼명
``` python
foreign.columns = ['날짜', '종가', '전일비', '등락률', '거래량', '기관', '외국인', '보유주수', '보유율']
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
