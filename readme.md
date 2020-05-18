# THL Covid-19 data ja Power BI

## Tutustuminen Power BI:hin
Tein pienen tutustumiskierroksen Power BI:hin mallintamalla viime aikoina paljon esillä ollutta dataa Covid-19:sta. Hain malliin dataa muutamasta eri paikasta. Datan oli JSON ja Excel muotoista, Power BI:hin toin kaiken joko Excel -formaatissa tai csv:nä. 

Taulukosta sairaanhoitopiiriä klikkaamalla saa suodatettua dataa. Tämä toimii kohtuullisen hyvin. 

Parannettavaa olisi paljonkin. Karttakuvaan en saanut rajattua sairaanhoitopiirejä, vaan se näyttää vain maakunnat. Kuntien täyttö ei Bingin karttoihin toiminut, se värjäsi vain pienen osan kunnan alueesta.

Viivagraafiin en saanut järkevästi toimiviksi tasoiksi kuin vuoden ja kuukauden. Kalenteritaulu olisi seuraava tehtävä asia.

### Lähdetiedostot

Ensimmäisenä on lyhyt Python skripti, joka hakee HS:n datat, sen jälkeen löytyvät linkit Excel -lähteisiin.

#### Ladataan kirjastot


```python
import pandas
import urllib3
import json
from pandas.io.json import json_normalize

```

#### Datan lataus

Haetaan HS.Fi:n THL:n rajapinnasta hakema data tarkempi kuvaus löytyy täältä: https://github.com/HS-Datadesk/koronavirus-avoindata
Tämän olisi voinut hakea staattisena tiedostona, mutta koska lähde päivittyy skriptillä voi tuoreuttaa datan joko tarvittaessa tai ajastetusti.


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
 
#### Excel -lähteet
Sairaanhoitopiirit löytyvät Excelinä osoitteesta: https://www.kuntaliitto.fi/sosiaali-ja-terveysasiat/sairaanhoitopiirien-jasenkunnat

Kuntaliiton sivuilta löytyvät myös kuntien asukasluvut: https://www.kuntaliitto.fi/sites/default/files/media/file/Kuntajaot%20ja%20asukasluvut%20kunnittain%202000-2020_2.xls

Postinumerot kunnittain löytyvät Tilastokeskuksen sivuilta: https://www.stat.fi/static/media/uploads/tup/paavo/alueryhmittely_posnro_2020_fi.xlsx

Tarvittaessa nämäkin tiedot voisi hakea skriptillä, mutta ne eivät päivity niin usein, että olisi tarpeen.
