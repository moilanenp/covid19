# THL Covid-19 data

Tämä on skripti, joka hakee tautitapaukset sairaanhoitopiireittäin THL:n rajapinnasta ja siirtää sen pandas dataframeen

## Ladataan kirjastot


```python
import pandas
import urllib3
import json
from pandas.io.json import json_normalize

```

## Datojen lataus

Ensiksi haetaan HS.Fi:n THL:n rajapinnasta hakema data tarkempi kuvaus löytyy täältä: https://github.com/HS-Datadesk/koronavirus-avoindata


```python

#HS.fi dataa löytyy täältä
url='https://w3qa5ydb4l.execute-api.eu-west-1.amazonaws.com/prod/finnishCoronaData/v2'

http=urllib3.PoolManager(cert_reqs='CERT_NONE')

r=http.request('GET',url)

stat=json.loads(r.data.decode('utf-8'))
confirmed=stat['confirmed']
confirmed_cases=json_normalize(confirmed)
confirmed_cases.to_excel('thl-confirmed.xlsx')
```

    C:\Users\pasim\anaconda3\lib\site-packages\urllib3\connectionpool.py:1004: InsecureRequestWarning: Unverified HTTPS request is being made to host 'w3qa5ydb4l.execute-api.eu-west-1.amazonaws.com'. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
      InsecureRequestWarning,
    

Sairaanhoitopiirit löytyvät Excelinä osoitteesta: https://www.kuntaliitto.fi/sosiaali-ja-terveysasiat/sairaanhoitopiirien-jasenkunnat

Kuntaliiton sivuilta löytyvät myös kuntien asukasluvut: https://www.kuntaliitto.fi/sites/default/files/media/file/Kuntajaot%20ja%20asukasluvut%20kunnittain%202000-2020_2.xls

Postinumerot kunnittain löytyvät Tilastokeskuksen sivuilta: https://www.stat.fi/static/media/uploads/tup/paavo/alueryhmittely_posnro_2020_fi.xlsx

Tarvittaessa nämäkin tiedot voisi hakea skriptillä, mutta ne eivät päivity niin usein, että olisi tarpeen.
