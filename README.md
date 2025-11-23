# 🌍 Development Dashboard

  

## Analyse du développement mondial : économie, population et impact environnemental 
*Période : 2015 – 2022*

---

  

## 1️⃣ Présentation de l'équipe

  

-  *Équipe :* Bad Planets

-  *Membres :* Zineb ET-TAHERY, Lahcen Assmira, Hajar Ait Lachgar , Mawada Ennaciri, Fatimazahra Bellala

-  *Organisation :* collaboration continue ; chaque membre participe aux étapes techniques, à la documentation et à la validation des KPI.

  

---

  

## 2️⃣ Contenu du Projet

  

Le projet doit fournir :

  

### ✔ 1. Fichier Power BI complet : Breif3.pbix

Avec *4 pages* :
-  *Vue Mondiale*
-  *Vue Régionale*
-  *Vue Pays*
-  *Corrélation*

  

---

  

### ✔ 2. Documentation (README.md) comprenant :

- Présentation de l'équipe
-  Sources & APIs
- Limites des données & gestion des valeurs manquantes
- Journal de bord quotidien
- Tableau des transformations Power Query
- Liste complète des KPI et  des mesures DAX avec explications
- Schéma du modèle de données (modèle en étoile)
- Outils utilisés
---

### ✔ 3. Suivi du projet Trello
-  [Lien Trello ](https://trello.com/invite/b/691f14d994315e97fac8c94b/ATTIdd894fbeb62e61c06bc68a23be4876779F8640F0/global-development-dashboard)
- Toutes les tâches ont été organisées et suivies dans Trello, chaque carte annotée avec le statut “terminé” pour indiquer que chaque membre a validé ses responsabilités.
---

### ✔ 4. Éléments complémentaires

-  Power BI (pages, modèle, KPI)

- Notes sur anomalies détectées dans les API

- Documentation API (World Bank + REST Countries)

- Lien Google Drive

  

---

  

## 3️⃣ Sources & APIs
### - World Bank API — Données économiques, démographiques et environnementales
-   **Indicateurs utilisés :**
    -   PIB : `NY.GDP.MKTP.CD`
    -   Population totale : `SP.POP.TOTL`
    -   Émissions de CO₂ : `EN.GHG.CO2.MT.CE.AR5`
-   **Méthode d’extraction :** requêtes **GET JSON**, suivies d'une **expansion des séries temporelles** (un enregistrement par _pays × année_).
-   **Période couverte :** 2015 → 2022
-   **Structure des données obtenues :**
    -   **2128 lignes** (après fusion PIB + Pop + CO₂)
    -   **7 colonnes** principales (pays, IISO codes (ISO2, ISO3), indicateur, valeur, année…)
   
### -REST Countries API — Métadonnées géographiques et administratives**
-   **Attributs utilisés :**  
   ISO3, nom officiel, région, sous-région, langues, superficie, statut d’indépendance.
-   **Rôle :** construction de la **dimension Pays**.
-   **Résultat de l’API :**
    -   **250 lignes** (correspondant aux pays et territoires recensés)
    -   **9 colonnes** structurées (nom, zone, codes ISO3, langue principale, surface…)
        

-  *Remarque* : utiliser ISO3 comme clé de jointure principale.

---
## 4️⃣ Limites des données & gestion des valeurs manquantes

### - Données manquantes (PIB, CO₂, Population)

Plusieurs pays présentent des valeurs nulles dans les indicateurs issus de la **World Bank API**.

### **Causes principales :**

**1- Conflits armés / instabilité politique**  
Certains pays n’ont pas transmis de données (ex : Syrie, Yémen, Somalie…).  
L’administration n’est plus en mesure de produire des indicateurs fiables.

**2- Territoires non souverains ou partiellement reconnus**  
Greenland, Gibraltar, Western Sahara…  
La World Bank ne publie pas de séries économiques complètes pour ces territoires.

**3- Micro-États ou territoires insulaires**  
Données non publiées ou non mesurées : Tokelau, Montserrat, Saint Helena…  
Population très faible → absence ou instabilité des données.

**4- Décision volontaire de non-reporting**  
Certains pays ne transmettent pas toutes les années.

### ✔ **Notre gestion des valeurs nulles**
-   Aucun remplacement artificiel (pas d’imputation) pour préserver la fiabilité.
-   Les visuels Power BI ignorent automatiquement les années / pays sans données.
-   Une mesure DAX permet de suivre ces cas pour contrôle qualité.
    

----------

### - Pays avec données totalement manquantes

Dans le cadre de notre projet **“World Progress 2030”**, certaines séries d’indicateurs de la World Bank API restent **complètement vides pour la période 2015–2022**. Ces cas sont documentés pour assurer la transparence et la fiabilité du tableau de bord.

### 1- PIB totalement manquant

Les pays / territoires pour lesquels **toutes les valeurs PIB sont nulles** :

-   **VEN** – Venezuela
-   **PRK** – Corée du Nord
-   **GIB** – Gibraltar
-   **ERI** – Érythrée
-   **VGB** – Îles Vierges britanniques
    

**Justification :**

-   Conflits internes ou instabilité politique (VEN, PRK, ERI)
-   Petits territoires ou dépendances non couverts par la Banque Mondiale (GIB, VGB)
-   Absence de reporting officiel 
    

### 2-  CO₂ totalement manquant

Les pays / territoires pour lesquels **toutes les valeurs CO₂ sont nulles** :

-   **PSE** – Palestine
-   **MAF** – Saint-Martin
-   **SSD** – Soudan du Sud
-   **SXM** – Saint-Martin (partie néerlandaise)
-   **SRB** – Serbie (certaines années historiques)
-   **SMR** – Saint-Marin
-   **MNE** – Monténégro
-   **MCO** – Monaco
-   **IMN** – Île de Man
-   **CUW** – Curaçao
-   **AND** – Andorre

**Justification :**

-   Pays récents ou partiellement reconnus (SSD, PSE, SXM, MAF, CUW)
-   Micro-États avec émissions très faibles ou non mesurées (SMR, MCO, LIE, IMN, AND)
    -   Données historiques manquantes ou partielles (SRB)
    
    
### 3- Traitement appliqué dans le projet

-   Les pays totalement vides sur un indicateur sont **exclus des visualisations correspondantes** pour ne pas fausser les KPI.
    
-   Pour le PIB, lorsqu’au moins une année est renseignée, une **moyenne conservatrice** est appliquée pour stabiliser les valeurs.
    
-   Les mesures DAX sont configurées pour **ignorer automatiquement les lignes avec valeurs nulles**, assurant ainsi la cohérence et la fiabilité des indicateurs globaux.
    

Cette section permet de **documenter de façon transparente les limitations des données**, conformément aux bonnes pratiques d’analyse pour des projets internationaux.



## 5️⃣ Journal de bord 


| Jour | Activités | Décisions & Justification |
| ---- | ----------------------- | --------------------------------------------------------------- |
| J1 | Analyse APIs & déf. KPI | Sélection indicateurs : PIB, Pop, CO₂; période 2015–2022; Countries |
| J3 | Import Power Query | Nettoyage JSON, conversion types, gestion des null |
| J3 | Conception modèle | Modèle en étoile (FactIndicateurs, DimPays, DimDate, DimRégion) |
| J4 | Mesures DAX | Implémentation Croissance, KPI composites: Densité, Intensité carbone, Ratio PIB/CO₂ ... |
| J4 | Construction dashboard | Pages : Monde / Région / Pays / Corrélation |
| J5 | Analyse & recommandations |Sélection de 3 pays (Inde, Nigeria, USA) pour l'analyse finale.|
|J5 | Livraison |Export PBIX, fichier README |

**J2: Jour férié**


  

---

  

## 6️⃣ Tableau des transformations

  

| Étape | Colonne / Table | Action | Justification |
| ---------- | ----------------- | --------------------------------------------------------------- | ------------------------------------------------ |
| Import | WB JSON (series) | Expansion → table plate (Country, ISO3, Year, Indicator, Value) | Rendre les séries indexables et filtrables |
| Import | REST Countries | Extraction langue principale, surface, statut | Enrichir la dimension pays |
| Nettoyage | PIB / Pop / CO2 | Conversion texte → nombre, gestion des valeurs null, conversion types  | Assurer calculs DAX corrects |
| Correction | Superficie null | Remplacer par moyenne régionale si absent | Permet calcul densité |
| Calcul | Mesure | Croissance du PIB (%), Croissance démographique (%), Densité de population ... | KPI|
| Fusion | WB + REST | Merge sur ISO3  | Conserver indicateurs passés et métadonnées pays |

  

---

  

## 7️⃣ Modèle de données (Design)

  

-  *FactIndicateurs* :  Indicator_ID(PK),ISO3(FK), Date_ID(FK), Value (PIB, POP, CO2)

-  *DimPays* : ISO3 (PK), name_common, name_official, Region_ID(FK), SubRegion, Language, area

-  *DimDate* : Date_ID(PK), Date

-  *DimRégion* : Region_ID(PK), RegionName,

*Relations principales :*

- FactIndicateurs[ISO3] → DimPays[ISO3] (Many-to-One)

- FactIndicateurs[Date_ID] → DimDate[Date_ID] (Many-to-One)

 - DimPays[Region_ID] → DimRégion[Region_ID] (Many-to-One)
 
 
*Modèle relationnel de Data Warehouse  :*
 ```mermaid
erDiagram

    DimPays {
        string area
        string ISO3
        string name_common
        string name_official
        int Region_ID
        string subregion
        string languages
    }

    DimRegion {
        string Region
        int Region_ID
    }

    FactIndicateurs {
        float CO2
        int Date_ID
        int Interaction_ID
        string ISO3
        float PIB
        float POP
    }

    DimDate {
        date Date
        int Date_ID
    }

    %% Relations
    DimPays ||--|{ FactIndicateurs : "ISO3 → pays"
    DimRegion ||--|{ DimPays : "Region_ID"
    DimDate ||--|{ FactIndicateurs : "Date_ID"
 
```
---
## 8️⃣ Liste des KPI et les mesures 

| Thème | KPI | Formule / Description |
| ------------- | ------------------------- | ----------------------------------- |
| Économie | PIB total | SUM(FactIndicateurs[GDP]) |
| | Croissance PIB (%) | ((PIB_t - PIB_t-1) / PIB_t-1) × 100 |
| | PIB/habitant | PIB / Population |
| Population | Population totale | SUM(Population) |
| | Croissance population (%) | ((Pop_t - Pop_t-1) / Pop_t-1) × 100 |
| | Densité | Population / Superficie |
| Environnement | CO₂ total (kt) | SUM(CO2) |
| | CO₂ / habitant | CO2 / Population |
| | Intensité carbone | CO2 / PIB |
| Durabilité | PIB / CO2 | PIB / CO2 |
| Comparatifs | Classement PIB | RANKX sur PIB total |

  
### Les  mesures DAX (prêtes à copier)

**- Ratios simples**
 PIB Par Habitant =` DIVIDE([PIB Total], [Population Totale], 0) `
CO2 Par Habitant = `DIVIDE([CO2 Total], [Population Totale], 0)`
Intensite Carbone = `DIVIDE([CO2 Total], [PIB Total], 0)`
PIB_par_CO2 =` DIVIDE([PIB Total], [CO2 Total], 0)`
Densité population =` DIVIDE( SUM(Table[Population]), SELECTEDVALUE(Table[Superficie]))`
PIB_CO2_Ratio =`DIVIDE( SUM(Table[PIB]), SUM(Table[CO2]))`

  

**-Croissance annuelle du PIB (%)**
Croissance PIB % = 
 VAR Curr = [PIB Total]
VAR Prev = CALCULATE([PIB Total], DATEADD('DimDate'[Date], -1, YEAR))
RETURN IF(Prev = 0, BLANK(), DIVIDE(Curr - Prev, Prev, 0) * 100)`

**-Croissance démographique (%)**
Croissance Population (%) =
VAR Pop_Actuelle = SELECTEDVALUE(Table[Population])
VAR Pop_Prec =
    CALCULATE(
        SELECTEDVALUE(Table[Population]),
        DATEADD(Table[Date], -1, YEAR)
    )
RETURN
DIVIDE(Pop_Actuelle - Pop_Prec, Pop_Prec) * 100


**-Évolution cumulée depuis 2015 (%)**
CroissanceDepuis2015 (%) =
VAR PIB2015 = CALCULATE([PIB Total], FILTER(ALL('DimDate'), 'DimDate'[Year] = 2015))
VAR PIB2022 = CALCULATE([PIB Total], FILTER(ALL('DimDate'), 'DimDate'[Year] = 2022))
RETURN IF(PIB2015 = 0, BLANK(), DIVIDE(PIB2022 - PIB2015, PIB2015, 0) * 100)


## 🔟 Création du Tableau de Bord – Documentation (2 pages Power BI)
---

PAGE 1 — Vue Mondiale (Direction Générale):
Objectif :
« Comprendre la situation mondiale et les grandes tendances 2015–2022. »

KPIs : CO₂ Total 2022, PIB Total 2022, Population Totale 2022, Densité mondiale, Croissance CO₂ (2015–2022), Croissance Population (2015–2022), Croissance PIB (2015–2022)

<img width="1044" height="586" alt="Capture1" src="https://github.com/user-attachments/assets/36c89c70-a4a7-45bb-b68c-b1da91c50d38" />


PAGE 2 — Vue Régionale (Comparaison Continents) :
Objectif :
« Comparer les régions du monde et identifier celles contribuant le plus à la croissance ou aux émissions. »

KPIs : CO₂ total par région, PIB total par région, Population totale, Croissance régionale CO₂, Croissance régionale PIB

<img width="1033" height="584" alt="Capture2" src="https://github.com/user-attachments/assets/660d3a6d-c3b9-4bd2-a861-429baf0ae541" />


PAGE 3 — Vue Pays (Analyse Micro):
Objectif :
« Étudier un pays individuellement : économie, démographie, environnement. »

KPIs :Superficie, Langue principale, PIB par habitant, PIB total, Population totale, CO₂ total

<img width="1035" height="581" alt="Capture3" src="https://github.com/user-attachments/assets/37f39a68-560f-4404-a71f-b3e8707f1f48" />


PAGE 4 — PAGE 4 — Impact Environnemental & Corrélation:
Objectif :
« Comprendre comment PIB, CO₂ et Population interagissent. »

KPIs : Intensité carbone (CO₂ / PIB), Productivité écologique (PIB / CO₂), Total PIB, Total CO₂, Croissance PIB vs CO₂

<img width="1036" height="585" alt="Capture4" src="https://github.com/user-attachments/assets/cda8e9ec-8799-44f5-ab77-5e60a4240a3a" />









---
## 9️Outils utilisés
-   **Power BI Desktop** : extraction, transformation (Power Query), modélisation en étoile, mesures DAX, visualisations interactives.
    
-   **Trello** : organisation et suivi des tâches, validation par chaque membre.
    
-  **Google Drive** : stockage, partage et versionning des fichiers et documentation.
    
-   **StackEdit** : rédaction et édition collaborative de la documentation Markdown.
    
-   **APIs** : World Bank (PIB, Population, CO₂) et REST Countries (métadonnées pays).
    

