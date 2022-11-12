
# Web scrapping 
In this project we loaded data from this website https://www.imdb.com/chart/top/?ref_=nv_mv_250
## excel file 
First, we install packages 
```Python
from bs4 import BeautifulSoup
import requests, openpyxl

```

Then, we create the excel file where we will store the data 

```Python
excel = openpyxl.Workbook()
print(excel.sheetnames)
sheet = excel.active
sheet.title = 'Top Rated Movies'
sheet.append(['Movie Rank','Movie Name','Year of Release','IMDB Rating'])


```

## Exracting data 

We used this python code to get data and loaded in the excel file 
```Python
try:
    source = requests.get('https://www.imdb.com/chart/top/?ref_=nv_mv_250')
    source.raise_for_status()

    soup = BeautifulSoup(source.text,'html.parser')
    movies = soup.find('tbody', class_='lister-list').find_all('tr')
    
    for movie in movies:

        name = movie.find('td',class_="titleColumn").a.text
        rank = movie.find('td',class_="titleColumn").get_text(strip=True).split('.')[0]
        year = movie.find('td', class_="titleColumn").span.text.strip('()')
        rating = movie.find('td', class_="titleColumn").strong.text

        print(name,rank,year,rating)
        sheet.append([name,rank,year,rating])




except Exception as e :
    print(e)

```

## Saving the excel file 

Finally we save the data into the excel file 
```Python
excel.save('IMDB_Movie_Ratings.xlsx')

```

