# scraping_and_building_my_db

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
