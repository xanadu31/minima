---
layout: post
mathjax: true
title:  "Awesome Data Visualization"
date:   2017-12-28 15:06:25 +0000
categories: data
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>



$$
\begin{equation*}
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0
\end{vmatrix}
\end{equation*}
$$



```python
#### Note Python
#### L'idée est ici de synthétiser les différente manière de géreer les données.#### Question: Quelles sont les fonctions pratiques pour manipuler les données avec Pandas.
#### Pourquoi:
###### Manipuler les données est essentiel dans ce métier, les données fournies ne sont généralement pas exploitable.###### En effet, elles peuvent être incomplètes, être présente sur plusieurs tables, Etre présente de manière non exploitables###### que ce soient liées à un format ou à une syntaxe.###### La manipulation de données est essentielle. Elle représente en moyenne 80% du temps d'un data miner.
###### Il existe différents outils de manipulation des données, SQL étant le plus connu.###### Nous nous intéresserons ici à Pandas, cette librairies Python, de manipulation des données.###### Quatre concepts seront abordées ici, la concaténation (concat), l'ajout (append), la fusion (merge) et la jointure (join) de données###### Nous verrons des cas d'utilisations de ces fonctions7
```
```python
#### Réponse:
##### Avant de comprendre le role de ces fonctions, il nous faut partir de données. De Data frame exploitable.
## Importation de pandas et création des data frames
import pandas as pd
```

```python
#### Créations de 3 Data frames d'exemples```

```python
#### Test sur les listes
import matplotlib.pyplot as plt
x = [1,5,5,2,7,6,9,10]y =[1,4,7,9,3,9,7,12]
plt.scatter(x,y)plt.show()#### Question: Quel est le type des objets x et ytype(x)# Réponse: Il s'agit d'une liste
```
![png](/ipynb/Images/Note_Python_3_0.png)


```python

import pandas as pdimport datetimeimport pandas_datareader as webfrom pandas_datareader import data, wb
debut = datetime.datetime(2010,1,1)fin = datetime.datetime(2015,8,22)
#### Création de notre data frame sur la valeur "Yahoo" 
df = web.DataReader("XOM", "yahoo", debut, fin)
df.head(20)
#### Importation de la librairie graphique
import matplotlib.pyplot as pltfrom matplotlib import style
style.use('fivethirtyeight')

```
    ---------------------------------------------------------------------------
    OSError                                   Traceback (most recent call last)
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\requests\packages\urllib3\connectionpool.py in urlopen(self, method, url, body, headers, retries, redirect, assert_same_host, timeout, pool_timeout, release_conn, chunked, body_pos, **response_kw)        593             if is_new_proxy_conn:    --> 594                 self._prepare_proxy(conn)        595     
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\requests\packages\urllib3\connectionpool.py in _prepare_proxy(self, conn)        809     --> 810         conn.connect()        811     
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\requests\packages\urllib3\connection.py in connect(self)        293             # self._tunnel_host below.    --> 294             self._tunnel()        295             # Mark this connection as not reusable    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\http\client.py in _tunnel(self)        918             raise OSError("Tunnel connection failed: %d %s" % (code,    --> 919                                                                message.strip()))        920         while True:    
    OSError: Tunnel connection failed: 407 Proxy Authentication Required
        During handling of the above exception, another exception occurred:    
    MaxRetryError                             Traceback (most recent call last)
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\requests\adapters.py in send(self, request, stream, timeout, verify, cert, proxies)        437                     retries=self.max_retries,    --> 438                     timeout=timeout        439                 )    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\requests\packages\urllib3\connectionpool.py in urlopen(self, method, url, body, headers, retries, redirect, assert_same_host, timeout, pool_timeout, release_conn, chunked, body_pos, **response_kw)        648             retries = retries.increment(method, url, error=e, _pool=self,    --> 649                                         _stacktrace=sys.exc_info()[2])        650             retries.sleep()    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\requests\packages\urllib3\util\retry.py in increment(self, method, url, response, error, _pool, _stacktrace)        387         if new_retry.is_exhausted():    --> 388             raise MaxRetryError(_pool, url, error or ResponseError(cause))        389     
    MaxRetryError: HTTPSConnectionPool(host='finance.yahoo.com', port=443): Max retries exceeded with url: /quote/XOM/history (Caused by ProxyError('Cannot connect to proxy.', OSError('Tunnel connection failed: 407 Proxy Authentication Required',)))
        During handling of the above exception, another exception occurred:    
    ProxyError                                Traceback (most recent call last)
    <ipython-input-22-9166dc9c0784> in <module>()          9 #### Création de notre data frame sur la valeur "Yahoo"         10     ---> 11 df = web.DataReader("XOM", "yahoo", debut, fin)         12          13 df.head(20)    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\pandas_datareader\data.py in DataReader(name, data_source, start, end, retry_count, pause, session, access_key)        119                                 adjust_price=False, chunksize=25,        120                                 retry_count=retry_count, pause=pause,    --> 121                                 session=session).read()        122         123     elif data_source == "yahoo-actions":    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\pandas_datareader\yahoo\daily.py in __init__(self, symbols, start, end, retry_count, pause, session, adjust_price, ret_index, chunksize, interval)         80          81         self.interval = '1' + self.interval    ---> 82         self.crumb = self._get_crumb(retry_count)         83          84     @property    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\pandas_datareader\yahoo\daily.py in _get_crumb(self, retries)        156         tu = "https://finance.yahoo.com/quote/{}/history".format(self.symbols)        157         response = self._get_response(tu,    --> 158                                       params=self.params, headers=self.headers)        159         out = str(self._sanitize_response(response))        160         # Matches: {"crumb":"AlphaNumeric"}    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\pandas_datareader\base.py in _get_response(self, url, params, headers)        124             response = self.session.get(url,        125                                         params=params,    --> 126                                         headers=headers)        127             if response.status_code == requests.codes.ok:        128                 return response    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\requests\sessions.py in get(self, url, **kwargs)        529         530         kwargs.setdefault('allow_redirects', True)    --> 531         return self.request('GET', url, **kwargs)        532         533     def options(self, url, **kwargs):    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\requests\sessions.py in request(self, method, url, params, data, headers, cookies, files, auth, timeout, allow_redirects, proxies, hooks, stream, verify, cert, json)        516         }        517         send_kwargs.update(settings)    --> 518         resp = self.send(prep, **send_kwargs)        519         520         return resp    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\requests\sessions.py in send(self, request, **kwargs)        637         638         # Send the request    --> 639         r = adapter.send(request, **kwargs)        640         641         # Total elapsed time of the request (approximately)    
    C:\Users\PGPB1729\AppData\Local\Continuum\Anaconda3\lib\site-packages\requests\adapters.py in send(self, request, stream, timeout, verify, cert, proxies)        498         499             if isinstance(e.reason, _ProxyError):    --> 500                 raise ProxyError(e, request=request)        501         502             raise ConnectionError(e, request=request)    
    ProxyError: HTTPSConnectionPool(host='finance.yahoo.com', port=443): Max retries exceeded with url: /quote/XOM/history (Caused by ProxyError('Cannot connect to proxy.', OSError('Tunnel connection failed: 407 Proxy Authentication Required',)))

```python#### Tracer du graphique 
df['High'].plot()plt.legend()plt.show()```
```pythonimport datetime as dtimport matplotlib.pyplot as plt
from matplotlib import styleimport pandas as pdimport pandas_datareader.data as web```
    ---------------------------------------------------------------------------
    ModuleNotFoundError                       Traceback (most recent call last)
    <ipython-input-3-629e1eb45628> in <module>()          4 from matplotlib import style          5 import pandas as pd    ----> 6 import pandas_datareader.data as web    
    ModuleNotFoundError: No module named 'pandas_datareader'

```python#### 

style.use('ggplot')
debut = dt.datetime(2000,1,1)fin = dt.datetime(2016,12,31)
df = web.DataReader('TSLA', 'yahoo', debut, fin)
print(df.head())


```
```python#### 27 Décembre 2017: 10:18 -- Imporatation des données de la table #### Lemoci.com ou FMI - World Economic Outlook Database#### Note cherche un moyen d'extraire directement les données via python (beautiful soup)#### Ici, on prendra 3 indicteurs économiques pour la Suède#### Le PIB libéllé en Dollars US (Gross domestic product, current prices in Billins U.S. dollars), le Taux de Chomage (Unemployment Rate in percentage) et la dette générale dyu pays (General governement gross debt), Inflation: Prix à la consommation moyen#### De 2000 à 2017
import pandas as pd
df1 = pd.DataFrame({'PIB' : [259.839, 239.957, 263.940, 31.109, 381.743, 389.043, 420.017, 487.818],                    'Taux_de_chomage' : [6.342, 5.825, 5.950, 6.567, 7.375, 7.642, 7.042, 6.117],                     'Dette_du_pays': [50.573, 51.731, 49.805, 48.942, 47.862, 48.190, 43.141, 38.202]},                    index = [2000, 2001, 2002, 2003, 2004, 2005, 2006, 2007])
df2 = pd.DataFrame({'PIB' : [513.966, 429.656, 488.378, 563.110, 543.881, 578.742, 573.818, 495.694, 511.000],                    'Taux_de_chomage' : [6.167, 8.300, 8.575, 7.767, 7.967, 8.000, 7.933, 7.400, 6.950],                    'Dette_du_pays' : [36.759, 40.311, 38.282, 37.529, 37.785, 40.434, 45.233, 43.934, 41.606]}                   , index = [2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016])
df3 = pd.DataFrame({                    'PIB' : [259.839, 239.957, 263.940, 31.109, 381.743, 389.043, 420.017, 487.818],                    'Taux_de_chomage' : [6.342, 5.825, 5.950, 6.567, 7.375, 7.642, 7.042, 6.117],                    'Inflation' : [1.294, 2.680, 1.939, 2.319, 1.022, 0.833, 1.497, 1.676]},                     index = [2000, 2001, 2002, 2003, 2004, 2005, 2006, 2007])
```
```python#### 27/12/2017 11h51:  Test sur les data frame: OK
df1.head(10)df2.head(10)df3.head()```

<div><style>    .dataframe thead tr:only-child th {        text-align: right;    }
    .dataframe thead th {        text-align: left;    }
    .dataframe tbody tr th {        vertical-align: top;    }</style><table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>Inflation</th>      <th>PIB</th>      <th>Taux_de_chomage</th>    </tr>  </thead>  <tbody>    <tr>      <th>2000</th>      <td>1.294</td>      <td>259.839</td>      <td>6.342</td>    </tr>    <tr>      <th>2001</th>      <td>2.680</td>      <td>239.957</td>      <td>5.825</td>    </tr>    <tr>      <th>2002</th>      <td>1.939</td>      <td>263.940</td>      <td>5.950</td>    </tr>    <tr>      <th>2003</th>      <td>2.319</td>      <td>31.109</td>      <td>6.567</td>    </tr>    <tr>      <th>2004</th>      <td>1.022</td>      <td>381.743</td>      <td>7.375</td>    </tr>  </tbody></table></div>

```python#### On remarquera qu'il y a une différence entre le dataframe 1 et dataframe 3, les index ne sont pas les mêmes.
#### Explorons la fonction concat#### 1/ Fonction concat
concat = pd.concat([df1, df2]) #### Ici, les dataframes ont les même champs et ont concatene dans une listeconcat.head(20) #### On obtiens bien le data set en entier```

<div><style>    .dataframe thead tr:only-child th {        text-align: right;    }
    .dataframe thead th {        text-align: left;    }
    .dataframe tbody tr th {        vertical-align: top;    }</style><table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>Dette_du_pays</th>      <th>PIB</th>      <th>Taux_de_chomage</th>    </tr>  </thead>  <tbody>    <tr>      <th>2000</th>      <td>50.573</td>      <td>259.839</td>      <td>6.342</td>    </tr>    <tr>      <th>2001</th>      <td>51.731</td>      <td>239.957</td>      <td>5.825</td>    </tr>    <tr>      <th>2002</th>      <td>49.805</td>      <td>263.940</td>      <td>5.950</td>    </tr>    <tr>      <th>2003</th>      <td>48.942</td>      <td>31.109</td>      <td>6.567</td>    </tr>    <tr>      <th>2004</th>      <td>47.862</td>      <td>381.743</td>      <td>7.375</td>    </tr>    <tr>      <th>2005</th>      <td>48.190</td>      <td>389.043</td>      <td>7.642</td>    </tr>    <tr>      <th>2006</th>      <td>43.141</td>      <td>420.017</td>      <td>7.042</td>    </tr>    <tr>      <th>2007</th>      <td>38.202</td>      <td>487.818</td>      <td>6.117</td>    </tr>    <tr>      <th>2008</th>      <td>36.759</td>      <td>513.966</td>      <td>6.167</td>    </tr>    <tr>      <th>2009</th>      <td>40.311</td>      <td>429.656</td>      <td>8.300</td>    </tr>    <tr>      <th>2010</th>      <td>38.282</td>      <td>488.378</td>      <td>8.575</td>    </tr>    <tr>      <th>2011</th>      <td>37.529</td>      <td>563.110</td>      <td>7.767</td>    </tr>    <tr>      <th>2012</th>      <td>37.785</td>      <td>543.881</td>      <td>7.967</td>    </tr>    <tr>      <th>2013</th>      <td>40.434</td>      <td>578.742</td>      <td>8.000</td>    </tr>    <tr>      <th>2014</th>      <td>45.233</td>      <td>573.818</td>      <td>7.933</td>    </tr>    <tr>      <th>2015</th>      <td>43.934</td>      <td>495.694</td>      <td>7.400</td>    </tr>    <tr>      <th>2016</th>      <td>41.606</td>      <td>511.000</td>      <td>6.950</td>    </tr>  </tbody></table></div>

```python##### De la même façon, on concatene les 3 dataframes
concat_ = pd.concat([df1,df2,df3])print(concat_)#### On remarque deux choses, la premiere est l'apparation de Na sur la variable 'Inflation' pour des valeurs comprisent entre 2000 et 2013#### 2/ L'ajout du dataframe 3 à la fin de l'index 2016 et l'apparition de NA pour la variable "Dette_du_pays"
#### il faudra donc vérifier les dimensions des dataframes à chques utilisation de la fonction "concat"```
          Dette_du_pays  Inflation      PIB  Taux_de_chomage    2000         50.573        NaN  259.839            6.342    2001         51.731        NaN  239.957            5.825    2002         49.805        NaN  263.940            5.950    2003         48.942        NaN   31.109            6.567    2004         47.862        NaN  381.743            7.375    2005         48.190        NaN  389.043            7.642    2006         43.141        NaN  420.017            7.042    2007         38.202        NaN  487.818            6.117    2008         36.759        NaN  513.966            6.167    2009         40.311        NaN  429.656            8.300    2010         38.282        NaN  488.378            8.575    2011         37.529        NaN  563.110            7.767    2012         37.785        NaN  543.881            7.967    2013         40.434        NaN  578.742            8.000    2014         45.233        NaN  573.818            7.933    2015         43.934        NaN  495.694            7.400    2016         41.606        NaN  511.000            6.950    2000            NaN      1.294  259.839            6.342    2001            NaN      2.680  239.957            5.825    2002            NaN      1.939  263.940            5.950    2003            NaN      2.319   31.109            6.567    2004            NaN      1.022  381.743            7.375    2005            NaN      0.833  389.043            7.642    2006            NaN      1.497  420.017            7.042    2007            NaN      1.676  487.818            6.117    
```python#### 2/ Fonction append
"""La fonction append permet également de joindre des données entre elles, il existe une petite différence avec la focntion concat que nous allons voir par la suite"""
df4 = df1.append(df2)print(df4) #### Ici, la fonction append joue le même rôle que concat#### On a bien nos deux dataframe supperposée l'un l'autre```
          Dette_du_pays      PIB  Taux_de_chomage    2000         50.573  259.839            6.342    2001         51.731  239.957            5.825    2002         49.805  263.940            5.950    2003         48.942   31.109            6.567    2004         47.862  381.743            7.375    2005         48.190  389.043            7.642    2006         43.141  420.017            7.042    2007         38.202  487.818            6.117    2008         36.759  513.966            6.167    2009         40.311  429.656            8.300    2010         38.282  488.378            8.575    2011         37.529  563.110            7.767    2012         37.785  543.881            7.967    2013         40.434  578.742            8.000    2014         45.233  573.818            7.933    2015         43.934  495.694            7.400    2016         41.606  511.000            6.950    
```python#### Là ou cela se complique, c'est quanqd on joins nos dataframe 1 et 3
df4 = df1.append(df3)print(df4)#### Par raaport à la fonction concat, il est plus efficace de```
          Dette_du_pays  Inflation      PIB  Taux_de_chomage    2000         50.573        NaN  259.839            6.342    2001         51.731        NaN  239.957            5.825    2002         49.805        NaN  263.940            5.950    2003         48.942        NaN   31.109            6.567    2004         47.862        NaN  381.743            7.375    2005         48.190        NaN  389.043            7.642    2006         43.141        NaN  420.017            7.042    2007         38.202        NaN  487.818            6.117    2000            NaN      1.294  259.839            6.342    2001            NaN      2.680  239.957            5.825    2002            NaN      1.939  263.940            5.950    2003            NaN      2.319   31.109            6.567    2004            NaN      1.022  381.743            7.375    2005            NaN      0.833  389.043            7.642    2006            NaN      1.497  420.017            7.042    2007            NaN      1.676  487.818            6.117    
```python
####
```
