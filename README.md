*__Here is the Markdown version of the probability and statistics practical work. A more dynamic QMD version is also available, but it requires R to render and execute the embedded code and to setup a project.__*

# INTRODUCTION

Ce TP a pour principal objectif de nous initier au langage R en utilisant le logiciel RStudio. Il est indéniable que ces deux outils sont extrêmement puissants pour l'analyse de vastes ensembles de données. RStudio se distingue par sa convivialité, car une fois que nous aurons compris les bases, il nous suffira de copier, coller et ajuster les formules pour les adapter à nos données spécifiques. Cette approche nous permettra d'explorer les multiples possibilités offertes par ces outils pour analyser nos données.

Nos objectifs dans ce TP seront de maîtriser la manipulation de vecteurs, d'apprendre à rechercher des données et de les modifier à notre convenance. Une fois que nous aurons acquis ces compétences, nous serons en mesure de synthétiser nos données en utilisant différents types de graphiques. Ensuite, nous pourrons les analyser en vue de repérer diverses caractéristiques de leur distribution, telles que la dispersion, les valeurs atypiques, la moyenne, la médiane, l'écart-type, et bien d'autres encore.

En somme, ce travail pratique nous permettra de développer des compétences essentielles en matière d'analyse de données à l'aide de R et RStudio, tout en nous familiarisant avec les différentes étapes de traitement et de visualisation des données.

# **EXERCICE 1**

## [Chargement et visualisation des données :]{.underline}

## Exe

```{r}

cpus <-scan("./TP1 - données/cpus.txt")
examen <- read.table("./TP1 - données/examen.txt", header = TRUE)
cpus
examen

```

## [Accéder aux différentes composantes du vecteur]{.underline}

```{r}
cpus[12]
cpus[3:19]
cpus[cpus>190]

examen$note
examen$note[7]
```

## [Création d'un vecteur et manipulation des composantes]{.underline}

```{r}
mesdonnees <- c(2.9, 3.4, 3.4, 3.7, 3.7, 2.8, 2.1, 2.5, 2.6)
mesdonnees
couleurs <- c ("bleu", "vert", "blanc", "noir", "jaune")
mesdonnees[-c(3:5)]
```

## [Afficher le contenu de l'environnement de travail]{.underline}

```{r}
ls()
```

# EXERCICE 2

## Diagramme branche-et-feuilles, histogramme, boîte à moustache

```{r}
stem(cpus)

par(mfrow=c(1,2), pty="s")
hist(cpus, xlab="performance relative", ylab = "fréquence", main = "", 
     col = "darkslategray4")
boxplot(cpus, xlab = "performance relative", col =  "darkslategray4", 
        horizontal = T)
rug(cpus)
par(mfrow=c(1,1))
```

1.  

-   La commande par(mfrow=c(1,2)) permet de mettre l'histiogramme et la boîte à moustache côte-à-côte.
-   c(1,2) détermine qu'il y aura 1 ligne et 2 colonnes.
-   Le paramètre pty = "s", permet de spécifier la forme du graphique (s ou m). Dans notre cas, il sera d'une forme carré au lieu de maximale.
-   En soi la commande par(mfrow=c(1,2), pty="s") permet alors de mettre les graphiques l'un à côté de l'autre avec une forme carrée. Cela permet une meilleure vision d'ensemble et une bonne visibilité des données.

2.  

-   La commande rug(cpus) permet d'ajouter un rug plot sur l'axe x, il s'agit d'un graphique de données pour une seule variable quantitative, affiché sous forme de marques le long d'un axe. Cela permet de visualiser la distribution des données.

## Analyse des données

### Valeurs atypiques et asymétrie

On remarque une forte asymétrie à gauche (asymétrie négative), il y a énormément de fréquences inférieures à 100.

```{r}
length(cpus[cpus<101])
length(cpus[cpus>100])
```

On retrouve 40 fréquences inférieures ou égales à 100 et seulement 10 qui sont supérieures à 100.

On peut ainsi voir des valeurs atypiques sur l'histogramme, en effet quelques fréquences semblent être très éloignées des autres (à partir de 200). Une valeur est même supérieure à 900

```{r}
cpus[cpus>200]
```

### Médiane et Moyenne

```{r}
median(cpus)
mean(cpus)
```

La fréquence moyenne est de 86.88, alors que la médiane est à 42. Au vu de l'asymétrie de la distribution il serait plus adéquat d'utiliser la médiane dans le cas présent.

### Modes des valeurs observées

```{r}
n.cpus <- table(cpus)
as.numeric(names(n.cpus)[n.cpus==max(n.cpus)])
```

Les modes des valeurs observées (valeurs dont l'effectif est le plus élevé dans une distribution de données) sont 24, 36 et 66. On voit donc que beaucoup de processeurs ont une performance relative qui se situe à 24, 36 et 66.

### Commande summary(cpus)

```{r}
summary(cpus)
```

La fonction summary permet d'obtenir les informations résumées de la distribution. Elle nous permet d'apercevoir le minimum, le maximum, les quartiles, la moyenne et la médiane.

### Modification des valeurs et effet sur la médiane et la moyenne

1.  Si on ajoute un processeur de performance relative 43 à l'ensemble des valeurs, la moyenne et la médiane ne changeraient presque pas.
2.  Si on soustrait 9 à chaque valeur observée, la moyenne et la médiance diminueraient de 9 aussi.
3.  Si on divise par 3 les observations, la moyenne et la médiane seraient aussi divisées par 3.

### Écart-type

#### Avec valeurs atypiques

```{r}
sd(cpus)
```

L'écart-type est de 148.4294

#### Sans valeurs atypiques

```{r}
cpuBoxplot<-boxplot(cpus, plot = FALSE)
cpuBoxplot
```

Les valeurs atypiques sont : 510, 915, 185, 370

```{r}
cpusnonat <- c(46, 110,  38,  22,  11,  38,  76,  21,  92,
            44,  66,  24,  10,  25, 26,  56,  40,   7, 
            141,  14,  24,  19,  24,  32,  33,  58, 
            12,  66,  62,  12,  45, 133,  64, 144,  36, 130, 
            16,  36,  65, 136,  60,  18,  66,  30, 100,  36)
sd(cpusnonat)
```

L'écart-type passe de 148.4294 à 38.5864 une fois que l'on enlève les valeurs atypiques. L'écart-type mesure la dispersion d'un ensemble de valeurs autours de leur moyenne, plus ce dernier est petit plus la population est homogène. Étant donné que lorsque l'on enlève nos valeurs atypiques, l'écart-type diminue drastiquement, on ne peut donc pas considérer qu'il soit un indicateur robuste, de plus il est encore très grand. On peut donc en conclure que la distribution est tout de même peu homogène.

# EXERCICE 3

## Boîte à moustache

```{r}
examen
lblue<-"#528B8B"
par(pty="s")
boxplot(note~groupe, data=examen, ylim=c(1,6), xlab="groupe",
varwidth=T, col=lblue, main="examen")
abline(h=4, lty=2)
```

### Bâtonnets des notes

```{r}
lblue<-"#528B8B"
par(pty="s")
boxplot(note~groupe, data=examen, ylim=c(1,6), xlab="groupe",
varwidth=T, col=lblue, main="examen")
abline(h=4, lty=2)
note.A<-split(examen$note, examen$groupe)$A
note.B<-split(examen$note, examen$groupe)$B
rug(note.A, side = 2)
rug(note.B, side = 4)
```

## Analyse des boîtes à moustache

En se basant sur les boîtes à moustache, une différence semble apparaître. En effet, la distribution du groupe A est inférieure à celle du groupe B. La différence se remarque aussi à-travers les médiances des deux groupes. On peut alors conclure ici que la classe A a une médiane plus basse que la classe B. Cependant il est difficile de savoir si elle est significative juste à l'aide des boîtes à moustache.

### Dispersions

Les dispersions des groupes est différentes ; le groupe A est asymétrique négativement, car il y a plus de valeurs basses (la médiane se situe plus contre le haut de la boîte, indiquant une proportion plus élevée de notes basses), alors que le groupe B est symétrique (la médiane se situe plus ou moins au centre). On peut alors remarquer que la classe A semble avoir eu plus de mauvaises notes que la classe B.

### Écarts-types

```{r}
by(examen$note,examen$groupe, sd)
```

D'après les deux écarts-types, la distribution des deux groupes est homogène, bien que celle du groupe A est légèrement moins homogène que celle du groupe B.

### Conclusion

D'après ce que nous avons pu observer avant, les deux distributions ont une dispersion homogène. Nous pouvons aussi remarquer, en nous basant sur la médiane, que en moyenne la classe B a de meilleurs résultats et une meilleure moyenne de classe que la classe A. Les performances moyennes de la classes A se situe entre le 3 et le 4, alors que celles de la classe B sont entre 4 et 5. De manière générale, la classe A a la majeure partie de ses notes en-dessous de 4. Les étudiants de la classe A semble alors, en regardant leurs notes, moins performants en statistiques que ceux de l'autre classe. Cependant, afin de déterminer si la différence est significative, il nous faudrait procéder à un test de Student de comparaison des moyennes.

## Diagramme de densité

![](Ressources/plot_ex3.png){fig-align="center"}

Ici, le graphique de densité serait plus pertinent pour voir plus précisément la distribution de chaque groupe. Ce dernier permet une meilleure vision des performances des élèves lors de leurs examens, on voit que les valeurs extrêmes de chaque groupe à un fort impact sur la médiane des groupes. Les boîtes à moustache sont pertinentes pour regarder la dispersion des groupes, mais ne montrent pas le nombre d'élèves ayant obtenu la note. En regardant les boîtes à moustache, on dirait que les groupes diffèrent énormément, mais en se basant sur les diagrammes de densité, on peut se rendre compte qu'ils ont énormément d'étudiants qui ont des notes similaires entre les deux classes. On remarque effectivement que ce qui change les médianes ici ce sont les valeurs extrêmes qui tendent vers le bas pour la classe A et vers le haut pour la classe B. Dans la classe A les étudiants avec des notes tournant autours de 2 font drastiquement baisser la médiance, et à l'inverse les meilleurs étudiants de la classe l'augmente.

# EXERCICE 4

# Données

```{r}
#install.packages("arules")
library(arules)
data("AdultUCI")
dframe<-AdultUCI[, c("education", "hours-per-week")]
colnames(dframe)<- c("education", "hours_per_week")
str(dframe)
```

Le changement de nom de variable permet de déterminer quelles informations on cherche à voir, en les précisant dans le vecteur c, ici "education" et "hours-per-week". Étant donné que "AdultUCI" comporte énormément d'information, le fait de passer à la variable "dframe" permet de sélectionner uniquement les données qui nous intéressent, ici l'éducation et le temps de travail par semaine.

```{r}
#install.packages("ggplot2")
library(ggplot2)
ggplot(dframe, aes(x=hours_per_week, y=education)) +
geom_point(colour="lightblue", alpha=0.1, position="jitter") +
geom_boxplot(outlier.size=0, alpha=0.2)
```

Dans l'ensemble, on peut remarquer que, en général, plus la personne a un niveau élevé d'étude, plus ses heures de travail seront élevées. Cette augmentation des heures commence à partir de "HS-grad" et semble constante pour les grades au-dessus. Cependant on remarque que les personnes situées dans la catégorie "some college" ont un temps de travail moins élevé. On peut alors hypothétiser que plus la personne a un degré d'étude élevée plus elle aura d'heures de travail dans la semaine.

# Valeurs manquantes

```{r}
dim(AdultUCI)
nrows<-nrow(AdultUCI)
n.missing<-rowSums(is.na(AdultUCI))
sum(n.missing>0)/nrows
```

# Analyse

#### Dispersion

On remarque que la formation "11th" est celle qui possède la plus grande dispersion du lot

#### Médianes

Dans l'ensemble on peut voir que presque toutes les formations ont une médiane qui s'aligne sur 43 ou 44h par semaine environ, sauf pour les catégories "prof school" et "doctorate", qui ont une médiane plus grande (49h environ pour l'école professionnelle et 45h environ pour les doctorants). On pourrait donc dire qu'il n'y a pas de différence flagrante de médiane entre les types de formation sauf pour "prof school" et "doctorate" qui ont tendance à travailler plus.

# Temps maximal

```{r}
nx<-by(dframe$hours_per_week, dframe$education, max, na.rm=T)
nx
```

#### Formation avec le temps maximal

```{r}
max(nx)
names(nx)[nx==max(nx)]
```

On remarque que beaucoup de types de formation ont des valeurs maximales de 99 heures par semaines. Ce résultat est à la fois surprenant, mais compréhensible de par le fait que les valeurs trouvées ici sont des valeurs atypiques se situant en-dehors de l'écart-type. Il s'agit donc de quelques individus (40 en tout) qui estiment passer 99 heures par semaine à travailler (une moyenne de 14h par jour). Cependant, ce résultat n'est pas représentatif de l'ensemble de l'échantillon/de la population. Le fait qu'autant de personnes avec tout type de formation estiment travailleur autant d'heures par semaine est tout de même surprenant. Il faudrait pour savoir précisément quelle formation engendre le plus d'heures de travail par semaine, il faudrait regarder les formations qui possèdent le plus de personnes ayant répondu la valeur maximale.

```{r}
nsd<-by(dframe$hours_per_week, dframe$education, sd, na.rm=T)
nsd
min(nsd)
names(nsd)[nsd==min(nsd)]
```

On remarque que l'éducation avec le plus petit écart-type est "Assoc-voc" avec un écart-type de 10.94331. C'est cette formation qui a la distribution la plus homogène, bien qu'elle ne soit pas très homogène en soi.

#### Étendue interquartile

```{r}
nIQR <- by(dframe$hours_per_week, dframe$education, IQR, na.rm=T)
nIQR
min(nIQR)
names(nIQR)[nIQR==min(nIQR)]
```

Non, en utilisant l'intervalle interquartile, on remarque que l'éducation qui a le plus petit IQR est "5th-6th". Cela se remarque déjà dans les boîtes à moustache, car on remarque que c'est celle qui est la plus petite. Sur la boîte à moustache, l'intervalle interquartile est représenté par la boîte en elle-même. On remarque alors que l'IQR correspond à l'étendue de la distribution une fois que l'on a enlevé les 25% les plus petits et les 25% les plus grand.

# EXERCICE 5

## Coefficients de corrélation

#### 1^~er~^ graphique :

![](Ressources/Figure10.png){fig-align="center"}

Sur ce graphique de nuage de point, on peut déjà remarquer que la corrélation est linéaire et positive. La ligne de régression tracée en rouge indique que si x augmente, alors y augmente aussi. On peut estimer que le coefficient de corrélation est d'environ 1, car à chaque augmentation de 1 sur x, une augmentation de 1 sur y peut être visualisée.

#### 2^ème^ graphique

![](Ressources/Figure11.png){fig-align="center"}

Sur ce graphique, on peut voir que la corrélation est nulle, ou neutre. On peut donc estimer que le coefficient de corrélation est de 0, car il n'existe pas de lien entre les deux variables.

#### 3^ème^ graphique

![](Ressources/Figure12.png){fig-align="center"}

Ici, on peut voir que la corrélation est non-linéaire non-monotone, elle n'est ni que décroissante, ni que croissante.

#### 4^ème^ graphique

![](Ressources/Figure13.png){fig-align="center"}

On peut voir ici que nous avons une corrélation linéaire négative qui tend vers -1, car lorsque x augmente de 1, y diminue d'environ 1.

# EXERCICE 6

## Téléchargement des données

```{r}
iris
library(ggforce)
```

## Nuage de points

```{r}
pCol <- c('#057076', '#ff8301', '#bf5ccb')
plot.iris<-ggplot(iris, aes(x=Petal.Length, y=Petal.Width, col=Species)) +
scale_color_manual(values=pCol) +
scale_x_continuous(breaks=seq(0.5, 7.5, by=1), limits=c(0.5, 7.5)) +
  scale_y_continuous(breaks=seq(-0.5, 3, by=0.5), limits=c(-0.5, 3)) +
labs(title="Edgar Anderson's Iris Data",
x="Petal Length",
y="Petal Width") +
theme(plot.title=element_text(size=12, hjust=.5),
axis.title=element_text(size=10, vjust=-2),
axis.text=element_text(size=10, vjust=-2)) +
geom_point(aes(color=Species), alpha=.6, size=3) +
theme_minimal()
plot.iris +
ggforce::geom_mark_ellipse(
aes(fill=Species, label=Species),
alpha=.15, show.legend=FALSE
)
```

### Analyses

#### Relation largeur-longueur des pétales

On peut remarquer une relation linéaire positive entre la largeur et la longueur des pétales. Lorsque la longueur augmente, la largeur à tendance à augmenter aussi.

#### Observations inhabituelles

Il ne semble pas y avoir d'observations inhabituelles dans la représentation sous forme de nuage de points.

## Corrélation

#### Calcul

```{r}
x=iris$Petal.Length
y=iris$Petal.Width
cor(x = x, y = y)
```

La corrélation étant de 0.963, elle est très forte et positive.

## Différencier les types d'iris

```{r}
setosas <- iris$Petal.Length[iris$Species=='setosa']
min(setosas)
max(setosas)
```

Une longueur comprise entre 1 et 1.9

## Animations

```{r}
#| cache: false
#| echo: false
library(tidyverse)
library(gganimate)
library(gifski)
library(av)
```

```{r}

anim<-plot.iris+
  transition_states(Species,
                    transition_length = 2,
                    state_length = 1)
anim

anim +
  enter_fade() +
  exit_shrink() +
  ggtitle('Now showing {closest_state}',
          subtitle = 'Frame {frame} of {nframes}')
```

```{r}
#| cache: false
#| echo: false
library(reticulate)

```

## Bonus
```{R}
#install.packages("reticulate")

library(reticulate)
```

```{python}
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt


iris_py = r.iris

versicolor = iris_py[iris_py.Species =="versicolor"]
setosa = iris_py[iris_py.Species == "setosa"]
virginica = iris_py[iris_py.Species == "virginica"]

plt.figure(figsize=(8, 8))

plt.scatter(setosa["Petal.Length"], setosa["Petal.Width"], label="Setosa", facecolor="red")
plt.scatter(virginica["Petal.Length"], virginica["Petal.Width"], label="Virginica", facecolor="blue")
plt.scatter(versicolor["Petal.Length"], versicolor["Petal.Width"], label="Versicolor",facecolor="green")

plt.xlabel("Petal Length (cm)")
plt.ylabel("Petal Width (cm)")
plt.title("Iris Dataset")
plt.show()

```

# CONCLUSION

\
Ce TP nous a donc permis de nous familiariser avec R et RStudio, deux outils puissants qui rendent l'analyse de données accessible et facile pour un large public. Au cours de ce TP, nous avons acquis la capacité de manipuler divers vecteurs, comprenant leur structure, l'accès à leurs variables et l'utilisation de la fonction summary() pour obtenir des informations essentielles.

Nous avons également découvert la polyvalence de R en matière de création de graphiques, notamment des histogrammes, des boîtes à moustaches et des nuages de points, chacun ayant ses propres caractéristiques et utilités spécifiques. Cette diversité d'outils nous a permis d'explorer la lecture des données à partir de graphiques et de comparer différents groupes.

De plus, nous avons pu créer des visualisations dynamiques en utilisant les fonctions ggplot et gganime, ce qui a amélioré la lisibilité et la compréhension de nos données. Enfin, nous avons noté les différences entre R et Python, R étant souvent considéré comme plus lisible et intuitif, ce qui en fait un choix attractif pour ceux qui ne sont pas familiers avec la programmation.

En résumé, R se révèle être un langage extrêmement polyvalent et captivant pour l'analyse de données. Sa facilité d'accès en fait un outil largement utilisé dans divers domaines, ce qui en renforce l'attrait.

