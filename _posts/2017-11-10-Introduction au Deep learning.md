# Deep learning: Application des méthodes d'apprentissages profonds


# Large CNN 

> import numpyfrom keras.datasets
> import cifar10from keras.models 
> import Sequentialfrom keras.layers 



```python import Densefrom keras.layers 
import Dropoutfrom keras.layers ```



```python
####Chargement des librairies 
import pandas as pdfrom pandas 
import read_csvfrom sklearn.model_selection
import KFoldfrom sklearn.model_selection 
import cross_val_scorefrom sklearn.linear_model 
import LogisticRegression```
```python####Importation de la table a étudiée
nom_fichier = 'pima-indians-diabetes.data.csv'nom = ['preg', 'plas','pres', 'skin','test', 'mass', 'pedi', 'age', 'class']table = read_csv(nom_fichier, names = nom) #Transformation du fichier importé en "dataframe"tableau = table.values #On récupere les valeurs de la tableX = tableau[:, 0:8] #On prend toute les colonnes (ici de 0 jusqu'à la huitième incluse (pas la 9 qui correspond à la 8 ici)). On définie ainsi les variables explicativesY = tableau[:,8] #On prend la neuvième colonnes pour définir la variable à expliquer
print(Y) #Ici, la variable est binaire: Il faut donc choisir un algorithme adaptée
?head()

```
    [ 1.  0.  1.  0.  1.  0.  1.  0.  1.  1.  0.  1.  0.  1.  1.  1.  1.  1.      0.  1.  0.  0.  1.  1.  1.  1.  1.  0.  0.  0.  0.  1.  0.  0.  0.  0.      0.  1.  1.  1.  0.  0.  0.  1.  0.  1.  0.  0.  1.  0.  0.  0.  0.  1.      0.  0.  1.  0.  0.  0.  0.  1.  0.  0.  1.  0.  1.  0.  0.  0.  1.  0.      1.  0.  0.  0.  0.  0.  1.  0.  0.  0.  0.  0.  1.  0.  0.  0.  1.  0.      0.  0.  0.  1.  0.  0.  0.  0.  0.  1.  1.  0.  0.  0.  0.  0.  0.  0.      0.  1.  1.  1.  0.  0.  1.  1.  1.  0.  0.  0.  1.  0.  0.  0.  1.  1.      0.  0.  1.  1.  1.  1.  1.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  1.      0.  0.  0.  0.  0.  0.  0.  0.  1.  0.  1.  1.  0.  0.  0.  1.  0.  0.      0.  0.  1.  1.  0.  0.  0.  0.  1.  1.  0.  0.  0.  1.  0.  1.  0.  1.      0.  0.  0.  0.  0.  1.  1.  1.  1.  1.  0.  0.  1.  1.  0.  1.  0.  1.      1.  1.  0.  0.  0.  0.  0.  0.  1.  1.  0.  1.  0.  0.  0.  1.  1.  1.      1.  0.  1.  1.  1.  1.  0.  0.  0.  0.  0.  1.  0.  0.  1.  1.  0.  0.      0.  1.  1.  1.  1.  0.  0.  0.  1.  1.  0.  1.  0.  0.  0.  0.  0.  0.      0.  0.  1.  1.  0.  0.  0.  1.  0.  1.  0.  0.  1.  0.  1.  0.  0.  1.      1.  0.  0.  0.  0.  0.  1.  0.  0.  0.  1.  0.  0.  1.  1.  0.  0.  1.      0.  0.  0.  1.  1.  1.  0.  0.  1.  0.  1.  0.  1.  1.  0.  1.  0.  0.      1.  0.  1.  1.  0.  0.  1.  0.  1.  0.  0.  1.  0.  1.  0.  1.  1.  1.      0.  0.  1.  0.  1.  0.  0.  0.  1.  0.  0.  0.  0.  1.  1.  1.  0.  0.      0.  0.  0.  0.  0.  0.  0.  1.  0.  0.  0.  0.  0.  1.  1.  1.  0.  1.      1.  0.  0.  1.  0.  0.  1.  0.  0.  1.  1.  0.  0.  0.  0.  1.  0.  0.      1.  0.  0.  0.  0.  0.  0.  0.  1.  1.  1.  0.  0.  1.  0.  0.  1.  0.      0.  1.  0.  1.  1.  0.  1.  0.  1.  0.  1.  0.  1.  1.  0.  0.  0.  0.      1.  1.  0.  1.  0.  1.  0.  0.  0.  0.  1.  1.  0.  1.  0.  1.  0.  0.      0.  0.  0.  1.  0.  0.  0.  0.  1.  0.  0.  1.  1.  1.  0.  0.  1.  0.      0.  1.  0.  0.  0.  1.  0.  0.  1.  0.  0.  0.  0.  0.  0.  0.  0.  0.      1.  0.  0.  0.  0.  0.  0.  0.  1.  0.  0.  0.  1.  0.  0.  0.  1.  1.      0.  0.  0.  0.  0.  0.  0.  1.  0.  0.  0.  0.  1.  0.  0.  0.  1.  0.      0.  0.  1.  0.  0.  0.  1.  0.  0.  0.  0.  1.  1.  0.  0.  0.  0.  0.      0.  1.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  1.  0.  0.  0.  1.      1.  1.  1.  0.  0.  1.  1.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.      0.  0.  1.  1.  0.  0.  0.  0.  0.  0.  0.  1.  0.  0.  0.  0.  0.  0.      0.  1.  0.  1.  1.  0.  0.  0.  1.  0.  1.  0.  1.  0.  1.  0.  1.  0.      0.  1.  0.  0.  1.  0.  0.  0.  0.  1.  1.  0.  1.  0.  0.  0.  0.  1.      1.  0.  1.  0.  0.  0.  1.  1.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.      1.  0.  0.  0.  0.  1.  0.  0.  1.  0.  0.  0.  1.  0.  0.  0.  1.  1.      1.  0.  0.  0.  0.  0.  0.  1.  0.  0.  0.  1.  0.  1.  1.  1.  1.  0.      1.  1.  0.  0.  0.  0.  0.  0.  0.  1.  1.  0.  1.  0.  0.  1.  0.  1.      0.  0.  0.  0.  0.  1.  0.  1.  0.  1.  0.  1.  1.  0.  0.  0.  0.  1.      1.  0.  0.  0.  1.  0.  1.  1.  0.  0.  1.  0.  0.  1.  1.  0.  0.  1.      0.  0.  1.  0.  0.  0.  0.  0.  0.  0.  1.  1.  1.  0.  0.  0.  0.  0.      0.  1.  1.  0.  0.  1.  0.  0.  1.  0.  1.  1.  1.  0.  0.  1.  1.  1.      0.  1.  0.  1.  0.  1.  0.  0.  0.  0.  1.  0.]    Object `head` not found.    
    
```python
####Lancement de l'algorithme (ici, il s'agit de LogicticRegression)
nombre_echantillon = 10kEchantillon = KFold(n_splits = 10, random_state = 7)#On crée 10 echantillons icimodel = LogisticRegression()
resultats = cross_val_score(model,X,Y,cv = kEchantillon) #On applique ces echantillons à notre jeu de données
print(resultats.mean())

```
    0.76951469583    
```python### Lancement de l'algorithme (RandomForest)from sklearn.model_selection import cross_val_scorefrom sklearn.ensemble import RandomForestClassifier  #Importation du classifier RandomForest de l'emsemble des classifiers
#### Paramètres et échantillonsnombre_arbres = 100maximum_var = 3KEchantillon = KFold(n_splits = 10, random_state = 7)
####Initialisation du modelmodel = RandomForestClassifier(n_estimators = nombre_arbres, max_features = maximum_var) # Model RandomForest avec les paramètres associéesresultats = cross_val_score(model, X,Y, cv = KEchantillon)
print(resultats.mean())
```
    0.776025290499    
```python#### Lancement de l'algorithme (ExtraTreesClassifier)
##### 1/ Importation des librairies utiles (intéressantes)from pandas import read_csvfrom sklearn.model_selection import KFoldfrom sklearn.model_selection import cross_val_scorefrom sklearn.ensemble import ExtraTreesClassifier
#### 2/ Paramètres et echantillions
nombre_arbres = 100maximum_var = 7kEchantillon = KFold(n_splits = 10, random_state = 7)
#### 3/ Initialisation du model et affichage des résultats
model = ExtraTreesClassifier (n_estimators = nombre_arbres, max_features = maximum_var)resultats = cross_val_score(model,X,Y, cv = kEchantillon)print(resultats.mean())```
    0.75517771702    
```python#### Grille de recherche et reglage des paramètres
##### 1/ Importation des librairies utiles iciimport numpy from pandas import read_csvfrom sklearn.linear_model import Ridgefrom sklearn.model_selection import GridSearchCV
#### Initialisation des paramètres du model et de la grille de recherche et création d'un tableau de valeur pour la grille de recherchealif = numpy.array([1,0.1,0.01,0.001,0.0001,0])parametre_grille = dict(alpha = alif)
#### Définition du model et affichage des résultats model = Ridge()grille = GridSearchCV(estimator = model, param_grid = parametre_grille)grille.fit(X,Y)print(grille.best_score_)print(grille.best_estimator_.alpha)
```
    0.279617559313    1.0    
```python#### Méthode1: Reglage de l'algorithme pour la méthode "Aléatoire"
#a/ Application#b/ Compréhension
##### Importation des packagesimport numpy from pandas import read_csvfrom scipy.stats import uniformfrom scipy.stats import normfrom sklearn.linear_model import Ridgefrom sklearn.model_selection import RandomizedSearchCV
##### Définition de la grille de recherche
param_grille_1 = {'alpha': uniform()}param_grille_2 = {'beta': norm()}
####Initialisation du modèle et affichage des résultats
model = Ridge()Recherche_param = RandomizedSearchCV(estimator=model, param_distributions = param_grille_1, n_iter = 100, random_state = 7)Recherche_param.fit(X,Y)
print(Recherche_param.best_score_) #### Comprendre le "Best_score"print(Recherche_param.best_estimator_.alpha)#### Attention à bien écrire le nom des variable (Ici, oubli d'un underscore)
```
    0.279617127031    0.977989511997    
```python#### Méthode2: Reglage de l'algorithme pour la méthode "de la grille de recherche"
##### Importation des packages pour la grille de recherche
import numpyfrom pandas import read_csvfrom sklearn.linear_model import Ridgefrom sklearn.model_selection import GridSearchCV
#### Initialisation des paramètresalifs = numpy.array([1,0.1,0.01,0.001,0.0001,0])param_grille = dict(alpha = alif)
####Choix du model, intenciation et affichage des resultats
model = Ridge()grille = GridSearchCV(estimator = model, param_grid = param_grille )grille.fit(X,Y) ####Application du modèle
print(grille.best_score_) #### Affichage du meilleur scoreprint(grille.best_estimator_.alpha) ####Affichage du meilleur estimateur```
    0.279617559313    1.0    
```python#### Projet Machine Learning: Classification IRIS
######################## 1/ Importation des packages###################################################
#Importation des packages de data managementfrom pandas import read_csvfrom pandas.tools.plotting import scatter_matrix
#Imporatation de packages de visualisationfrom matplotlib import pyplot
#Importation de packages de conformation à l'utilisation des modeles
from sklearn.model_selection import train_test_splitfrom sklearn.model_selection import KFoldfrom sklearn.model_selection import cross_val_score
#Importations des métriques d'évaluation des modèlesfrom sklearn.metrics import classification_reportfrom sklearn.metrics import confusion_matrixfrom sklearn.metrics import accuracy_score
#Importation des modèlesfrom sklearn.linear_model import LogisticRegressionfrom sklearn.tree import DecisionTreeClassifierfrom sklearn.neighbors import KNeighborsClassifierfrom sklearn.discriminant_analysis import LinearDiscriminantAnalysisfrom sklearn.naive_bayes import GaussianNBfrom sklearn.svm import SVC
```
```python########### 2/ Chargement des données de la base IRIS   ############################################
nom_fichier = 'iris.data.csv'nom = ['sepal-longueur', 'sepal-largeur', 'petal-longueur', 'petal-largeur', 'class']base_donnees = read_csv(nom_fichier, names = nom)
###print(base_donnees)
type(base_donnees)
x = base_donnees.values[:,0:1]
```
```python
```
