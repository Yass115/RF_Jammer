# Laboratoire RF - Brouilleur (RF Jammer)

Rapport de conception d'un brouilleur radiofrequence (RF Jammer), realise dans le cadre d'un laboratoire d'electronique RF.

> Les schemas ci-dessous sont des images SVG animees (fichiers separes dans `diagrams/`). Elles s'affichent avec leurs animations dans un navigateur ou un visualiseur Markdown qui charge les images externes (GitHub, VS Code, Typora...). Une version HTML autonome et interactive est egalement disponible dans `rf-jammer-chaine.html`.

## Objectif

Concevoir et realiser une chaine RF complete capable de generer un signal a une frequence donnee et de le rayonner avec une puissance suffisante pour perturber (brouiller) les communications radio dans une bande cible. La chaine couvre l'ensemble du parcours du signal : generation de l'oscillation, amplification de puissance et rayonnement par antenne, avec les etages d'adaptation d'impedance necessaires entre chaque bloc.

## Architecture generale

![Schema bloc de la chaine RF](diagrams/00-overview.svg)

Chaque etage est concu et caracterise separement avant integration de la chaine complete.

## Contenu du projet

### 1. Oscillateur Colpitts

- Generation du signal RF a la frequence de brouillage visee
- Choix de la topologie Colpitts pour la stabilite en frequence et la simplicite de mise en oeuvre avec un diviseur capacitif
- Dimensionnement du reseau LC (inductance et diviseur capacitif) pour fixer la frequence d'oscillation
- Verification des conditions de demarrage et d'entretien des oscillations (gain de boucle, condition de Barkhausen)

**Formule :** `f0 = 1 / (2*pi*sqrt(L*Ceq))`, avec `Ceq = (C1*C2)/(C1+C2)`

| Grandeur | Valeur (exemple, a ajuster) |
|---|---|
| L (tank) | 220 nH |
| C1 | 47 pF |
| C2 | 100 pF |
| f0 estimee | ~ 433 MHz |

![Oscillateur Colpitts](diagrams/01-colpitts.svg)

### 2. Adaptation d'impedance entre l'oscillateur et l'etage d'amplification

- Adaptation de l'impedance de sortie de l'oscillateur vers l'impedance d'entree de l'etage de puissance
- Objectif : maximiser le transfert de puissance et limiter les pertes/reflexions entre les deux etages
- Dimensionnement du reseau d'adaptation (topologie en L, Pi ou T selon le rapport d'impedances a transformer)

| Grandeur | Valeur |
|---|---|
| Zout oscillateur | ~ 200 ohms |
| Zin amplificateur | ~ 50 ohms |
| Topologie | reseau en L (Pi/T selon Q) |

![Adaptation d'impedance d'entree](diagrams/02-adapt-in.svg)

### 3. Etage d'amplification de puissance

- Amplification du signal genere par l'oscillateur afin d'obtenir la puissance de sortie necessaire au brouillage
- Choix et dimensionnement du transistor/composant actif de puissance
- Points de fonctionnement (polarisation) et classe de fonctionnement retenue

**Formule :** `Pout(dBm) = Pin(dBm) + Gain(dB)`

| Grandeur | Valeur (exemple) |
|---|---|
| Gain | ~ 13 dB |
| Pin | 0 dBm |
| Pout visee | ~ 13 dBm |
| Vcc | 12 V |

![Etage d'amplification de puissance](diagrams/03-amplifier.svg)

### 4. Adaptation d'impedance de sortie

- Adaptation entre la sortie de l'etage de puissance et l'impedance caracteristique de l'antenne (typiquement 50 ohms)
- But : transferer un maximum de puissance vers l'antenne et limiter le ROS (rapport d'ondes stationnaires)
- Dimensionnement du reseau d'adaptation final

**Formule :** `ROS = (1 + |Gamma|) / (1 - |Gamma|)`

| Grandeur | Valeur |
|---|---|
| Zout ampli | ~ 15 ohms |
| Zantenne | 50 ohms |
| ROS cible | < 1.5:1 |

![Adaptation d'impedance de sortie](diagrams/04-adapt-out.svg)

### 5. Antenne

- Choix de l'antenne adaptee a la frequence et a la bande de brouillage visee
- Verification de l'adaptation globale de la chaine (mesure ou simulation du ROS/S11 en sortie)

**Formule :** `lambda = c / f0`, longueur quart d'onde = `lambda / 4`

| Grandeur | Valeur (exemple a 433 MHz) |
|---|---|
| lambda | ~ 69 cm |
| Quart d'onde | ~ 17.3 cm |
| Impedance antenne | ~ 50 ohms |

![Antenne quart d'onde](diagrams/05-antenna.svg)

## Outils utilises

- Simulation/dimensionnement des reseaux d'adaptation d'impedance (abaque de Smith)
- Mesures a l'oscilloscope et/ou a l'analyseur de spectre pour la caracterisation de l'oscillateur et de la chaine complete

## Note sur les valeurs

Les valeurs numeriques affichees (L, C, gain, frequence, dimensions d'antenne) sont des exemples illustratifs a la bande 433 MHz, a remplacer par les valeurs reellement calculees et mesurees pour ce projet.

## Structure du dossier

```
├── README.md
├── rf-jammer-chaine.html      Version HTML interactive (animations garanties)
└── diagrams/                  Schemas SVG utilises dans ce README
    ├── 00-overview.svg
    ├── 01-colpitts.svg
    ├── 02-adapt-in.svg
    ├── 03-amplifier.svg
    ├── 04-adapt-out.svg
    └── 05-antenna.svg
```

## Auteur

Yassine - etudiant M1 electronique, ISIB (HE2B, Bruxelles)
