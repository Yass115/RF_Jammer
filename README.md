# Laboratoire RF - Brouilleur (RF Jammer)

Rapport de conception d'un brouilleur radiofrequence (RF Jammer), realise dans le cadre d'un laboratoire d'electronique RF.

> Les schemas ci-dessous sont des SVG animes embarques directement dans ce document. Ils s'affichent avec leurs animations dans un navigateur, VS Code (previsualisation Markdown) ou Typora. Sur GitHub, l'apercu peut ne montrer que la version statique du schema selon la politique de rendu SVG de la plateforme ; une version HTML complete et garantie animee est disponible dans `rf-jammer-chaine.html`.

## Objectif

Concevoir et realiser une chaine RF complete capable de generer un signal a une frequence donnee et de le rayonner avec une puissance suffisante pour perturber (brouiller) les communications radio dans une bande cible. La chaine couvre l'ensemble du parcours du signal : generation de l'oscillation, amplification de puissance et rayonnement par antenne, avec les etages d'adaptation d'impedance necessaires entre chaque bloc.

## Architecture generale

<p align="center">
<svg viewBox="0 0 720 130" xmlns="http://www.w3.org/2000/svg" width="100%" style="max-width:720px">
  <rect x="0" y="0" width="720" height="130" fill="#0E1520"/>
  <g font-family="monospace" font-size="12" fill="#E7ECF1">
    <rect x="10"  y="45" width="120" height="46" rx="4" fill="#111823" stroke="#33445A"/>
    <text x="70" y="63" text-anchor="middle" fill="#F5A623" font-size="10">01</text>
    <text x="70" y="78" text-anchor="middle">Colpitts</text>

    <rect x="160" y="45" width="120" height="46" rx="4" fill="#111823" stroke="#33445A"/>
    <text x="220" y="63" text-anchor="middle" fill="#F5A623" font-size="10">02</text>
    <text x="220" y="78" text-anchor="middle">Adaptation Z</text>

    <rect x="310" y="45" width="120" height="46" rx="4" fill="#111823" stroke="#33445A"/>
    <text x="370" y="63" text-anchor="middle" fill="#F5A623" font-size="10">03</text>
    <text x="370" y="78" text-anchor="middle">Ampli puissance</text>

    <rect x="460" y="45" width="120" height="46" rx="4" fill="#111823" stroke="#33445A"/>
    <text x="520" y="63" text-anchor="middle" fill="#F5A623" font-size="10">04</text>
    <text x="520" y="78" text-anchor="middle">Adaptation Z</text>

    <rect x="610" y="45" width="100" height="46" rx="4" fill="#111823" stroke="#33445A"/>
    <text x="660" y="63" text-anchor="middle" fill="#F5A623" font-size="10">05</text>
    <text x="660" y="78" text-anchor="middle">Antenne</text>
  </g>
  <path d="M10 68 L130 68 M160 68 L280 68 M310 68 L430 68 M460 68 L580 68 M610 68 L710 68"
        stroke="#33445A" stroke-width="1.5" fill="none"/>
  <circle r="5" fill="#F5A623">
    <animateMotion dur="6s" repeatCount="indefinite"
      path="M10 68 L130 68 L160 68 L280 68 L310 68 L430 68 L460 68 L580 68 L610 68 L710 68"/>
  </circle>
</svg>
</p>

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

<p align="center">
<svg viewBox="0 0 320 220" xmlns="http://www.w3.org/2000/svg" width="100%" style="max-width:360px">
  <rect x="0" y="0" width="320" height="220" fill="#0E1520"/>
  <g stroke="#33445A" stroke-width="1.5" fill="none">
    <line x1="160" y1="10" x2="160" y2="40"/>
    <line x1="130" y1="40" x2="190" y2="40"/>
    <line x1="160" y1="104" x2="160" y2="120"/>
    <line x1="160" y1="120" x2="160" y2="128"/>
    <line x1="182" y1="140" x2="230" y2="140"/>
    <line x1="138" y1="140" x2="90" y2="140"/>
    <line x1="160" y1="162" x2="160" y2="190"/>
    <line x1="90" y1="120" x2="90" y2="180"/>
    <line x1="80" y1="90" x2="100" y2="90"/>
    <line x1="90" y1="90" x2="90" y2="120"/>
    <line x1="90" y1="180" x2="90" y2="196"/>
    <line x1="60" y1="140" x2="90" y2="140"/>
    <line x1="230" y1="140" x2="230" y2="206"/>
    <line x1="210" y1="206" x2="250" y2="206"/>
  </g>
  <path d="M160 40 q -12 8 0 16 q 12 8 0 16 q -12 8 0 16 q 12 8 0 16" fill="none" stroke="#C97C4B" stroke-width="1.6"/>
  <circle cx="160" cy="140" r="22" fill="none" stroke="#C97C4B" stroke-width="1.8"/>
  <rect x="80" y="76" width="20" height="10" fill="none" stroke="#C97C4B" stroke-width="1.6"/>
  <rect x="80" y="196" width="20" height="10" fill="none" stroke="#C97C4B" stroke-width="1.6"/>
  <circle cx="60" cy="140" r="2" fill="#C97C4B"/>
  <g font-family="monospace" font-size="10" fill="#8A97A5">
    <text x="176" y="70">L</text>
    <text x="150" y="145" fill="#E7ECF1">Q</text>
    <text x="46" y="84">C1</text>
    <text x="46" y="204">C2</text>
    <text x="10" y="144">base</text>
    <text x="234" y="120">sortie osc.</text>
  </g>
  <path d="M240 20 q6 -14 12 0 q6 14 12 0 q6 -14 12 0 q6 14 12 0" fill="none" stroke="#F5A623" stroke-width="1.8">
    <animate attributeName="d" dur="1.2s" repeatCount="indefinite"
      values="
      M240 20 q6 -14 12 0 q6 14 12 0 q6 -14 12 0 q6 14 12 0;
      M240 20 q6 14 12 0 q6 -14 12 0 q6 14 12 0 q6 -14 12 0;
      M240 20 q6 -14 12 0 q6 14 12 0 q6 -14 12 0 q6 14 12 0"/>
  </path>
</svg>
</p>

### 2. Adaptation d'impedance entre l'oscillateur et l'etage d'amplification

- Adaptation de l'impedance de sortie de l'oscillateur vers l'impedance d'entree de l'etage de puissance
- Objectif : maximiser le transfert de puissance et limiter les pertes/reflexions entre les deux etages
- Dimensionnement du reseau d'adaptation (topologie en L, Pi ou T selon le rapport d'impedances a transformer)

| Grandeur | Valeur |
|---|---|
| Zout oscillateur | ~ 200 ohms |
| Zin amplificateur | ~ 50 ohms |
| Topologie | reseau en L (Pi/T selon Q) |

<p align="center">
<svg viewBox="0 0 320 160" xmlns="http://www.w3.org/2000/svg" width="100%" style="max-width:360px">
  <rect x="0" y="0" width="320" height="160" fill="#0E1520"/>
  <g stroke="#33445A" stroke-width="1.5" fill="none">
    <line x1="40" y1="76" x2="80" y2="76"/>
    <line x1="144" y1="76" x2="180" y2="76"/>
    <line x1="180" y1="76" x2="180" y2="60"/>
    <line x1="164" y1="106" x2="196" y2="106"/>
    <line x1="164" y1="112" x2="196" y2="112"/>
    <line x1="180" y1="76" x2="180" y2="106"/>
    <line x1="180" y1="112" x2="180" y2="140"/>
    <line x1="40" y1="140" x2="180" y2="140"/>
    <line x1="180" y1="76" x2="280" y2="76"/>
    <line x1="280" y1="76" x2="280" y2="140"/>
  </g>
  <path d="M80 76 q6 -8 12 0 q6 8 12 0 q6 -8 12 0 q6 8 12 0" fill="none" stroke="#C97C4B" stroke-width="1.6"/>
  <rect x="20" y="40" width="30" height="100" fill="none" stroke="#33445A"/>
  <rect x="270" y="40" width="30" height="100" fill="none" stroke="#33445A"/>
  <g font-family="monospace" font-size="10" fill="#8A97A5">
    <text x="14" y="34">Zosc</text>
    <text x="86" y="56">Ls</text>
    <text x="200" y="112">Cp</text>
    <text x="284" y="34">Zin ampli</text>
  </g>
  <circle r="4" fill="#F5A623">
    <animateMotion dur="2s" repeatCount="indefinite"
      path="M40 76 L 80 76 Q86 68 92 76 Q98 84 104 76 Q110 68 116 76 Q122 84 128 76 Q134 68 140 76 L 180 76 L 280 76"/>
  </circle>
</svg>
</p>

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

<p align="center">
<svg viewBox="0 0 320 180" xmlns="http://www.w3.org/2000/svg" width="100%" style="max-width:360px">
  <rect x="0" y="0" width="320" height="180" fill="#0E1520"/>
  <path d="M0 40 q10 -14 20 0 q10 14 20 0 q10 -14 20 0 q10 14 20 0" fill="none" stroke="#8A6420" stroke-width="1.4"/>
  <g stroke="#33445A" stroke-width="1.5" fill="none">
    <line x1="80" y1="40" x2="120" y2="40"/>
    <line x1="120" y1="40" x2="120" y2="90"/>
    <line x1="150" y1="60" x2="150" y2="20"/>
    <line x1="130" y1="20" x2="170" y2="20"/>
    <line x1="180" y1="90" x2="220" y2="90"/>
    <line x1="150" y1="120" x2="150" y2="150"/>
    <line x1="130" y1="150" x2="170" y2="150"/>
  </g>
  <circle cx="150" cy="90" r="30" fill="none" stroke="#C97C4B" stroke-width="1.8"/>
  <g font-family="monospace" font-size="10" fill="#8A97A5">
    <text x="4" y="20">entree</text>
    <text x="140" y="94" fill="#E7ECF1">Q_pa</text>
    <text x="176" y="16">Vcc</text>
    <text x="176" y="154">gnd</text>
    <text x="230" y="56">sortie amplifiee</text>
  </g>
  <path d="M220 90 q12 -34 24 0 q12 34 24 0 q12 -34 24 0 q12 34 24 0" fill="none" stroke="#4FD1A5" stroke-width="1.8">
    <animate attributeName="stroke-width" values="1.8;2.8;1.8" dur="1s" repeatCount="indefinite"/>
  </path>
</svg>
</p>

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

<p align="center">
<svg viewBox="0 0 320 160" xmlns="http://www.w3.org/2000/svg" width="100%" style="max-width:360px">
  <rect x="0" y="0" width="320" height="160" fill="#0E1520"/>
  <g stroke="#33445A" stroke-width="1.5" fill="none">
    <line x1="20" y1="76" x2="80" y2="76"/>
    <line x1="80" y1="76" x2="80" y2="60"/>
    <line x1="64" y1="106" x2="96" y2="106"/>
    <line x1="64" y1="112" x2="96" y2="112"/>
    <line x1="80" y1="76" x2="80" y2="106"/>
    <line x1="80" y1="112" x2="80" y2="140"/>
    <line x1="184" y1="76" x2="240" y2="76"/>
    <line x1="20" y1="140" x2="240" y2="140"/>
    <line x1="240" y1="76" x2="240" y2="140"/>
    <line x1="240" y1="108" x2="290" y2="108"/>
  </g>
  <path d="M120 76 q6 -8 12 0 q6 8 12 0 q6 -8 12 0 q6 8 12 0" fill="none" stroke="#C97C4B" stroke-width="1.6"/>
  <line x1="80" y1="76" x2="120" y2="76" stroke="#33445A" stroke-width="1.5"/>
  <g font-family="monospace" font-size="10" fill="#8A97A5">
    <text x="14" y="34">Zout amp.</text>
    <text x="126" y="56">Ls</text>
    <text x="100" y="112">Cp</text>
    <text x="250" y="100">50 ohms</text>
  </g>
  <circle r="4" fill="#4FD1A5">
    <animateMotion dur="2s" repeatCount="indefinite"
      path="M20 76 L 80 76 L 120 76 Q126 68 132 76 Q138 84 144 76 Q150 68 156 76 Q162 84 168 76 L 240 76 L 240 108 L 290 108"/>
  </circle>
</svg>
</p>

### 5. Antenne

- Choix de l'antenne adaptee a la frequence et a la bande de brouillage visee
- Verification de l'adaptation globale de la chaine (mesure ou simulation du ROS/S11 en sortie)

**Formule :** `lambda = c / f0`, longueur quart d'onde = `lambda / 4`

| Grandeur | Valeur (exemple a 433 MHz) |
|---|---|
| lambda | ~ 69 cm |
| Quart d'onde | ~ 17.3 cm |
| Impedance antenne | ~ 50 ohms |

<p align="center">
<svg viewBox="0 0 320 200" xmlns="http://www.w3.org/2000/svg" width="100%" style="max-width:320px">
  <rect x="0" y="0" width="320" height="200" fill="#0E1520"/>
  <g stroke="#33445A" stroke-width="1.5" fill="none">
    <line x1="160" y1="150" x2="160" y2="60"/>
    <line x1="130" y1="150" x2="190" y2="150"/>
  </g>
  <circle cx="160" cy="60" r="10" fill="none" stroke="#F5A623" stroke-width="1.6">
    <animate attributeName="r" values="6;70" dur="2.4s" repeatCount="indefinite"/>
    <animate attributeName="opacity" values="0.9;0" dur="2.4s" repeatCount="indefinite"/>
  </circle>
  <circle cx="160" cy="60" r="10" fill="none" stroke="#F5A623" stroke-width="1.6">
    <animate attributeName="r" values="6;70" dur="2.4s" begin="0.8s" repeatCount="indefinite"/>
    <animate attributeName="opacity" values="0.9;0" dur="2.4s" begin="0.8s" repeatCount="indefinite"/>
  </circle>
  <circle cx="160" cy="60" r="10" fill="none" stroke="#F5A623" stroke-width="1.6">
    <animate attributeName="r" values="6;70" dur="2.4s" begin="1.6s" repeatCount="indefinite"/>
    <animate attributeName="opacity" values="0.9;0" dur="2.4s" begin="1.6s" repeatCount="indefinite"/>
  </circle>
  <text x="176" y="100" font-family="monospace" font-size="10" fill="#8A97A5">quart d'onde</text>
</svg>
</p>

## Outils utilises

- Simulation/dimensionnement des reseaux d'adaptation d'impedance (abaque de Smith)
- Mesures a l'oscilloscope et/ou a l'analyseur de spectre pour la caracterisation de l'oscillateur et de la chaine complete

## Note sur les valeurs

Les valeurs numeriques affichees (L, C, gain, frequence, dimensions d'antenne) sont des exemples illustratifs a la bande 433 MHz, a remplacer par les valeurs reellement calculees et mesurees pour ce projet.

## Auteur

Yassine - etudiant M1 electronique, ISIB (HE2B, Bruxelles)
