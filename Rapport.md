# INFO1 - Projet Base de Données
## Partie 1 : Etude des relations déchet-exutoire à partir du fichier 'relation_dechets_exutoire.csv' avec la base de données Cassandra.
### Question 1 : Nombre d'exutoires pour les déchets "Dématérialisation"

```sql
SELECT COUNT(*) FROM data.ex WHERE label = 'Dématérialisation' allow filtering;
```
> count : 14


### Question 2 : Type d'exutoire pour les déchets "Peinture à l'eau"
```sql
SELECT label FROM data.ex WHERE type = 'Exutoire' and nom_dechet = 'Peinture à l''eau' allow filtering;
```

> label : Poubelle noire


### Question 3 : Déchets associés à l'exutoire 3
```sql
SELECT nom_dechet as Dechet FROM data.ex WHERE id_exutoire = 3;
```

> dechet : Bocal en verre, Bouteille en verre, Flacon en verre, Pot de yaourt en verre, Pot en verre


### Question 4 : Déchets appartenants à la famille "Verre"
```sql
SELECT nom_dechet FROM data.ex WHERE famille_dechet = 'Verre' allow filtering;
```

> nom_dechet : Pot de yaourt en verre, Pot en verre, Bocal en verre, Pot en verre, Bocal en verre, Bouteille en verre, Flacon en verre,  de yaourt en verre, Pot en verre

> Nb: Impossible d'utiliser DISTINCT with the WHERE condition. Would require another table architecture, which we will not do.


### Question 5 : Description de l'exutoire 6
```sql
SELECT description FROM data.ex WHERE id_exutoire = 6 limit 1;
```

> description : Ce déchet peut être composté. Il deviendra alors une ressource !!


### Question 6 : Nombre de type de déchets concernés par les exutoires de type "Conseil"
```sql
SELECT COUNT(DISTINCT famille_dechet) FROM data.ex WHERE type = 'conseil';
```

Pour les exutoires de type "Conseil", on trouve 4 famille_dechet différentes : Equipements de la maison, Textiles d''habillement, ligne de maison et chaussures, Objets en plastiques et Médical




### Question 7 : Type d'exutoire contenant le plus de déchets différents

```sql
CREATE INDEX IF NOT EXISTS type_index ON data.ex (type);
SELECT COUNT(DISTINCT "nom_dechet") AS dechet_nb, type FROM data.ex GROUP BY type ORDER BY dechet_nb DESC LIMIT 1; 
```

Résultat : Déchetterie, 22 familles de déchet différentes



### Question 8 : Type d'exutoire le plus courant
```sql
SELECT label, COUNT(*) FROM data.ex WHERE type = 'exutoire' GROUP BY label ORDER BY COUNT(*) DESC LIMIT 1;
```

Résultat: Déchetterie, 715 occurences (déchets)



### Question 9 : Type d'exutoire le plus courant pas famille de déchet
```sql
SELECT famille_dechet, type, label, COUNT(*) AS nb_occurrences FROM data.ex GROUP BY famille_dechet, type, label ORDER BY famille_dechet, nb_occurrences DESC;
```

Résultat:
| famille_dechet | type_exutoire | nb_occurence |
|---------------:|--------------:|-------------:|
|Alimentation    |        2      |118330|
|Bijoux          |        2      |3428|
|Bois           |        4      |1685|
|Culture loisirs |        4      |1938|
|Divers          |        2      |3762|
|Electrique et Electronique| 4    | 198040|
|Emballages en métal| 1| 8846|
|Emballage en plastique |1| 24014|
|Equipement de maison |4| 874|
|Instrument de musique |4 |16716|
|Médical |4| 3740|
|Matériaux |4| 99654|
|Objets en plastique| 2| 9572|
|Outils manuel |4| 27201|
|Papier carton |1| 41395|
|Produits chimiques| 4| 34468|
|Sport |4| 21768|
|Textile |5| 80938|
|Transport |4| 13410|
|Végétaux |4| 20426|
|Vaisselle |4| 11533|
|Verre |3| 3293|
|Vie domestique| 2| 141555|


### Question 10 Labels d'exutoires associés à chaque famille de déchets
```sql
SELECT famille_dechet, type, label COUNT(*) AS nb_occ FROM project GROUP BY famille_dechet, type, label ORDER BY famille_dechet, nb_occ DESC;
```

Résultat:
| famille_dechet | type_exutoire |
|---------------:|--------------:|
|Alimentation| Poubelle noire|
|Bijoux| Poubelle noire|
|Bois| DÃ©chetterie|
|Culture loisirs| Poubelle noire|
|Divers| Poubelle noire|
|Electrique et électronique|Déchetterie|
|Emballages en métal| Poubelle verte / jaune|
|Emballages en plastique| Poubelle verte / jaune|
|Equipement de la maison| Déchetterie|
|Instrument de musique| Déchetterie|
|Médical| Pharmacie|
|Matériaux| Déchetterie|
|Objets en Plastique| Poubelle noire|
|Outils manuel| Déchetterie|
|Papier carton| Poubelle verte / jaune|
|Produits chimiques| Déchetterie|
|Sport| Déchetterie|
|Textiles d'habillement, linge de maison et chaussures| Borne textile|
|Transport| Déchetterie|
|Végétaux| Déchetterie|
|Vaisselle| Déchetterie|
|Verre| Borne à verre|
|Vie domestique| Poubelle noire|

Alimentation
Composteur
DÃ©chetterie
Poubelle noire
Bijoux
DÃ©chetterie
Poubelle noire
Bois
Composteur
DÃ©chetterie
Poubelle noire
Culture loisirs
DÃ©chetterie
Poubelle noire
Poubelle verte / jaune
Divers
Composteur
DÃ©chetterie
DÃ©chetterie professionnelle
Ferrailleur agrÃ©Ã©
Police / Gendarmerie
Poubelle noire
Revendeur ou Installateur
Electrique et Ã©lectronique
Bornes de collecte
DÃ©chetterie
DÃ©chetteries spÃ©cifiques
Ferrailleur agrÃ©Ã©
Revendeur ou Installateur
Emballages en mÃ©tal
Bornes de collecte
DÃ©chetterie
Poubelle verte / jaune
Emballages en plastique
DÃ©chetterie
Poubelle verte / jaune
Equipement de la maison
DÃ©chetterie
Instrument de musique
Borne textile
DÃ©chetterie
Poubelle noire
Revendeur ou Installateur
MÃ©dical
DÃ©chetterie
DÃ©chetterie professionnelle
Pharmacie
Poubelle noire
Revendeur ou Installateur
MatÃ©riaux
DÃ©chetterie
DÃ©chetterie professionnelle
Ferrailleur agrÃ©Ã©
Poubelle noire
Revendeur ou Installateur
Objets en Plastique
DÃ©chetterie
Poubelle noire
Outils manuel
DÃ©chetterie
Ferrailleur agrÃ©Ã©
Poubelle noire
Papier carton
Composteur
DÃ©chetterie
DÃ©chetterie professionnelle
Poubelle noire
Poubelle verte / jaune
Produits chimiques
Bornes de collecte
DÃ©chetterie
DÃ©chetterie professionnelle
Police / Gendarmerie
Poubelle noire
Revendeur ou Installateur
Sport
Borne textile
DÃ©chetterie
Poubelle noire
Textiles d'habillement, linge de maison et chaussures
Borne textile
DÃ©chetterie
Poubelle noire
Transport
DÃ©chetterie
DÃ©chetterie professionnelle
Ferrailleur agrÃ©Ã©
Poubelle noire
Revendeur ou Installateur
VÃ©gÃ©taux
Composteur
DÃ©chetterie
Points de collecte provisoires
Vaisselle
DÃ©chetterie
Poubelle noire
Verre
Borne Ã  verre
Vie domestique
Borne textile
Bornes de collecte
Composteur
DÃ©chetterie
DÃ©chetterie professionnelle
Ferrailleur agrÃ©Ã©
Police / Gendarmerie
Poubelle noire
Poubelle verte / jaune
Revendeur ou Installateur


### Question 11: Clé permettant d'assurer une répartition uniforme des données sur un cluster distribué
Réponse: On pourrait utiliser les clé "type", car elle contient exactement 50% de "Exutoire" et 50% de "Conseil".


## Partie 2 : Etude d'indicateurs atmosphériques avec localisation avec MongoDB
### Question 1 : Communes ayant un indice de qualité de l'air "Moyen" 
```java
db.atmo_indices.distinct("properties.lib_zone", {"properties.lib_qual": "Moyen"})
```

Résultat: 
[
    "Aubière",
    "Aulnat",
    "Beaumont",
    "Blanzat",
    "Ceyrat",
    "Chamalières",
    "Châteaugay",
    "Clermont-Ferrand",
    "Cournon-d'Auvergne",
    "Cébazat",
    "Durtol",
    "Gerzat",
    "Le Cendre",
    "Lempdes",
    "Nohanent",
    "Orcines",
    "Pont-du-Château",
    "Pérignat-lès-Sarliève",
    "Romagnat",
    "Royat",
    "Saint-Genès-Champanelle"
]


### Question 2 : Code de qualité de l'air maximal à Saint-Genès-Champanelle
```java
db.atmo_indices.find( { "properties.lib_zone": "Saint-Genès-Champanelle" }, {"_id":0, "properties.code_qual":1} ).sort( { "properties.code_qual": -1 } ).limit(1)
```

Résultat:
{
    "properties" : {
        "code_qual" : NumberInt(3)
    }
}


### Question 3 : Différentes communes observées
```java
db.atmo_indices.distinct("properties.lib_zone", {"properties.type_zone": "commune"})
```

Résultat:
```
[
    "Aubière",
    "Aulnat",
    "Beaumont",
    "Blanzat",
    "Ceyrat",
    "Chamalières",
    "Châteaugay",
    "Clermont-Ferrand",
    "Cournon-d'Auvergne",
    "Cébazat",
    "Durtol",
    "Gerzat",
    "Le Cendre",
    "Lempdes",
    "Nohanent",
    "Orcines",
    "Pont-du-Château",
    "Pérignat-lès-Sarliève",
    "Romagnat",
    "Royat",
    "Saint-Genès-Champanelle"
]
```

### Question 4 : Code qualité moyen par commune
```java
db.atmo_indices.aggregate([{$group:{_id:"$properties.lib_zone", avg_quality:{$avg:"$properties.code_qual"}}}, {$project: {_id: 0, avg_quality: 1, lib_zone: "$_id"}}])
```

Résultat:
{
    "avg_quality" : 2.0,
    "lib_zone" : "Beaumont"
}
{
    "avg_quality" : 2.111111111111111,
    "lib_zone" : "Durtol"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Le Cendre"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Cébazat"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Aulnat"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Châteaugay"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Gerzat"
}
{
    "avg_quality" : 2.111111111111111,
    "lib_zone" : "Chamalières"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Pont-du-Château"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Cournon-d'Auvergne"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Lempdes"
}
{
    "avg_quality" : 2.111111111111111,
    "lib_zone" : "Orcines"
}
{
    "avg_quality" : 2.111111111111111,
    "lib_zone" : "Royat"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Nohanent"
}
{
    "avg_quality" : 2.111111111111111,
    "lib_zone" : "Saint-Genès-Champanelle"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Clermont-Ferrand"
}
{
    "avg_quality" : 2.111111111111111,
    "lib_zone" : "Ceyrat"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Pérignat-lès-Sarliève"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Blanzat"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Romagnat"
}
{
    "avg_quality" : 2.0,
    "lib_zone" : "Aubière"
}


### Question 5 : Qualité de l'air la plus récente dans un rayon de 1km de la position (45.77441539864761, 3.0890499134717686)
```java 
db.atmo_indices.createIndex({ "geometry": "2dsphere" })
db.atmo_indices.find({"geometry":{$near:{$geometry:{type:"Point", coordinates:[3.0890499134717686, 45.77441539864761]}, $maxDistance:1000}}}, {"_id":0, "properties.code_qual":1, "properties.lib_zone":1}).sort({"properties.date_ech":-1}).limit(1)
```

Résultat
{
    "properties" : {
        "code_qual" : NumberInt(2),
        "lib_zone" : "Clermont-Ferrand"
    }
}


### Question 6 : Nombre de relevés d'indice différent pour les communes dont le nom commence par C
```java
db.atmo_indices.aggregate([
  {
    $match: {"properties.lib_zone": /^C/}
  },
  {
    $group: {_id: null, uniqueLibQuals: { $addToSet: "$properties.lib_qual" }}
  },
  {
    $project: {nb_of_different_indices: { $size: "$uniqueLibQuals" }}
  }
])
```

Résultat:
{
    "_id" : null,
    "nb_of_different_indices" : NumberInt(2)
}


### Question 7 : Communes par qualité d'air
```java
db.atmo_indices.aggregate([{$group:{_id:"$properties.lib_qual", lib_zones:{$addToSet:"$properties.lib_zone"}}}])
```

Résultat:
{
    "_id" : "Dégradé",
    "lib_zones" : [
        "Royat",
        "Ceyrat",
        "Chamalières",
        "Orcines",
        "Saint-Genès-Champanelle",
        "Durtol"
    ]
}
{
    "_id" : "Moyen",
    "lib_zones" : [
        "Lempdes",
        "Aulnat",
        "Beaumont",
        "Ceyrat",
        "Royat",
        "Nohanent",
        "Romagnat",
        "Gerzat",
        "Aubière",
        "Chamalières",
        "Pont-du-Château",
        "Cournon-d'Auvergne",
        "Clermont-Ferrand",
        "Orcines",
        "Le Cendre",
        "Blanzat",
        "Durtol",
        "Châteaugay",
        "Saint-Genès-Champanelle",
        "Pérignat-lès-Sarliève",
        "Cébazat"
    ]
}


### Question 8 : Concentration moyenne de PM10 en Auvergne-Rhône-Alpes le 22 octobre 2023
```java
db.atmo_indices.aggregate([
  {
    $match: {
      "properties.date_ech": "2023-10-22T00:00:00Z",
      "properties.source": "Atmo Auvergne-Rhône-Alpes"
    }
  },
  {
    $group: {
      _id: null,
      avg_PM10_Auvergne_Rhone_Alpes: { $avg: "$properties.conc_pm10" }
    }
  },
  {
      $project:{
          _id:0,
          avg_PM10_Auvergne_Rhone_Alpes:1
      }
  }
]) 
```

Résultat:
{
    "avg_PM10_Auvergne_Rhone_Alpes" : 7.739757142857142
}


### Question 9 : Evolution de la concentration de N02 à Clermont-Ferrand
```java
db.atmo_indices.aggregate([{$match:{"properties.lib_zone":"Clermont-Ferrand"}}, {$group:{_id:{$dateToString: { format: "%d/%m/%Y", date: { $toDate: "$properties.date_ech" } }}, avg_NO2:{$avg:"$properties.conc_no2"}}}, {$sort:{_id:1}}, {$project:{date:"$_id", avg_NO2:1, _id:0}}]);
```

Résultat:
{
    "avg_NO2" : 86.3478,
    "date" : "17/10/2023"
}
{
    "avg_NO2" : 44.4089,
    "date" : "18/10/2023"
}
{
    "avg_NO2" : 56.8136,
    "date" : "19/10/2023"
}
{
    "avg_NO2" : 48.7794,
    "date" : "20/10/2023"
}
{
    "avg_NO2" : 10.6188,
    "date" : "21/10/2023"
}
{
    "avg_NO2" : 55.7,
    "date" : "22/10/2023"
}
{
    "avg_NO2" : 48.505,
    "date" : "23/10/2023"
}
{
    "avg_NO2" : 51.4811,
    "date" : "24/10/2023"
}
{
    "avg_NO2" : 49.5019,
    "date" : "25/10/2023"
}


### Question 10 : Commune avec la concentration d'ozone la plus faible
```java
db.atmo_indices.aggregate([{$match:{"properties.code_o3":3}}, {$sort:{"properties.conc_o3":1}}, {$limit:1}, {$project:{_id:0, Location:"$properties.lib_zone", O3_Concentration:"$properties.conc_o3"}}])
```

Résultat:
{
    "Location" : "Durtol",
    "O3_Concentration" : 100.4635
}

