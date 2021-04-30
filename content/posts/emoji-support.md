---
author: Miki
date: "2021-04-30"
title: Scraped Google Custom Search API
---

:relaxed:

```{python}
from googleapiclient.discovery import build

my_api_key = " " #The API_KEY you acquired
my_cse_id = " " #The search-engine-ID you created

def google_search(search_term, api_key, cse_id, date):
    service = build("customsearch", "v1", developerKey=api_key )
    res = service.cse().list(q=search_term, cx=cse_id, sort=date).execute()
    if 'items' in res:
      return res['items']
    else:
      return 'N' # return N as missing value
```

Then searched key words...
```{python}
save_result=[]
list_of_bigtechs= ['facebook','google','amazon','twitter','apple','microsoft','big tech','giant tech']
list_of_keywords=['politics','power','election','influence']
# list_of_keywords=['pandemic','covid19']
my_date="date:r:20210101:20210430"
for i in list_of_bigtechs:
  for j in list_of_keywords:
    query=i+j
    results = google_search(query, my_api_key, my_cse_id,my_date)
    for result in results:
      print(result)
      save_result.append(result)
```
Added Year column...
```{python}
import pandas as pd
savedresult = [x for x in save_result if x != 'N']
my_file=pd.DataFrame(savedresult,columns=['kind',	'title',	'htmlTitle',	'link',	'displayLink',	'snippet',	'htmlSnippet',	'formattedUrl',	'htmlFormattedUrl',	'pagemap',	'cacheId'])
my_file=my_file[['title','displayLink','snippet']]
my_file['Year'] = my_date[-8:-4]
```
