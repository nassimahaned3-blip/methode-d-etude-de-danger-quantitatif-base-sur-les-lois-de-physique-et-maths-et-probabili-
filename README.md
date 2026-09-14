# methode-d-etude-de-danger-quantitatif-base-sur-les-lois-de-physique-et-maths-et-probabili-
\documentclass[12pt,a4paper]{article}

% ============================================================
% PACKAGES
% ============================================================
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage[french]{babel}
\usepackage{amsmath, amssymb, amsthm}
\usepackage{mathtools}
\usepackage{geometry}
\usepackage{booktabs}
\usepackage{array}
\usepackage{multirow}
\usepackage{graphicx}
\usepackage{float}
\usepackage{hyperref}
\usepackage{xcolor}
\usepackage{tikz}
\usetikzlibrary{shapes.geometric, arrows.meta, positioning, fit, backgrounds}

\geometry{margin=2.5cm}

% ============================================================
% TITRE
% ============================================================
\title{
    \textbf{Méthode d'analyse de risque industriel}\\
    \large Modèle physique en 9 étapes\\
    \large De l'identification des dangers à la décision
}
\author{
    \textbf{Nassima HANED}\\
    \small Ingénieur en Planification et Statistique
}
\date{\today}

% ============================================================
\begin{document}
\maketitle

\noindent\textit{Ce document a été créé le 14-09-2026.}
\vspace{1cm}

% ============================================================
\begin{abstract}
Ce document formalise la méthode d'analyse quantitative de risque
industriel construite étape par étape. Contrairement aux approches
qui ajustent des exposants par régression statistique sur des
données limitées, cette méthode s'appuie exclusivement sur des
briques physiquement ou empiriquement fondées~: lois physiques
établies pour la gravité des scénarios, données de fiabilité du
matériel pour les probabilités de défaillance, et facteurs
correctifs calibrés sur des accidents industriels réels documentés
pour l'erreur résiduelle. La méthode produit deux sorties
complémentaires et non mélangées~: un risque physique (en mètres,
pour la conformité réglementaire) et un coût de risque (en unité
monétaire, pour la décision économique). Le document se termine par
la structure prête à être remplie avec les données réelles de
l'installation étudiée.
\end{abstract}

\tableofcontents
\newpage

% ============================================================
\section{Principe général}
% ============================================================

\subsection{Ce que ce modèle n'est pas}

Ce modèle n'est \textbf{ni} une régression statistique dont les
exposants seraient ajustés sur un petit nombre de données observées,
\textbf{ni} un réseau de neurones qui apprendrait une relation
globale entre la matière, la probabilité et la gravité. Ces deux
approches ont été examinées et écartées~: avec peu de données
réelles, elles produisent une précision apparente qui ne reflète pas
une réalité physique, et elles perdent la traçabilité nécessaire à
la défense d'un dossier de sécurité industrielle.

\subsection{Ce que ce modèle est}

Chaque brique du modèle vient d'une source défendable~:

\begin{itemize}
    \item les lois physiques établies (Taylor/TNT, flux thermique,
    dispersion gaussienne de Pasquill-Gifford) pour la gravité des
    scénarios,
    \item les données de fiabilité du matériel (bases type OREDA)
    pour les probabilités de défaillance,
    \item des facteurs correctifs identifiés et calibrés sur des
    accidents industriels réels documentés pour ce que le modèle
    physique de base ne capture pas.
\end{itemize}

Aucun paramètre n'est laissé libre à un ajustement statistique
opaque.

% ============================================================
\section{Vue d'ensemble des 9 étapes}
% ============================================================

\begin{figure}[H]
\centering
\begin{tikzpicture}[
    node distance=0.4cm and 0.5cm,
    step/.style={rectangle, rounded corners, draw, fill=blue!10,
                 minimum width=2.6cm, minimum height=1.3cm, align=center,
                 font=\scriptsize},
    arrow/.style={-{Latex[length=2mm]}, thick}
]

\node[step] (s1) {1. Identification\\des dangers};
\node[step, right=of s1] (s2) {2. Masse\\équivalente $M_k$};
\node[step, right=of s2] (s3) {3. Gravité\\physique $G_k$};
\node[step, right=of s3, fill=green!10] (s4) {4. Probabilité\\par équipement};
\node[step, right=of s4] (s5) {5. Probabilité\\globale $P_k$};

\node[step, below=1.3cm of s3] (s6) {6. Erreur\\résiduelle $\varepsilon_k$};
\node[step, right=of s6, fill=orange!15] (s7) {7. Risque\\$R_k$ (mètres)};
\node[step, right=of s7, fill=orange!15] (s8) {8. Coût\\$C_k$ (argent)};
\node[step, right=of s8, fill=red!15] (s9) {9. Décision};

\draw[arrow] (s1) -- (s2);
\draw[arrow] (s2) -- (s3);
\draw[arrow] (s3) -- (s4);
\draw[arrow] (s4) -- (s5);
\draw[arrow] (s5.south) -- ++(0,-0.3) -| (s6.north);
\draw[arrow] (s3.south) -- ++(0,-0.3) -| (s6.north);
\draw[arrow] (s6) -- (s7);
\draw[arrow] (s7) -- (s8);
\draw[arrow] (s8) -- (s9);

\end{tikzpicture}
\caption{Les 9 étapes de la méthode}
\end{figure}

% ============================================================
\section{Étape 1 --- Identification des dangers}
% ============================================================

\subsection{Objectif}
Savoir quoi regarder~: dresser la liste complète des dangers
présents sur l'installation, avant tout calcul.

\subsection{Entrées}
\begin{itemize}
    \item Inventaire complet des produits présents, y compris les
    produits secondaires souvent oubliés~: produits de traitement
    d'eau, produits de nettoyage/passivation, catalyseurs,
    inhibiteurs de corrosion, combustibles auxiliaires, additifs de
    procédé.
    \item Fiches de données de sécurité (FDS) réelles de chaque
    produit.
    \item Identification des paires de produits incompatibles
    $(j_1, j_2)$ pouvant réagir dangereusement en cas de mélange
    accidentel.
\end{itemize}

\subsection{Sortie}
\begin{equation}
\mathbf{K} = \{\text{explosion}, \text{incendie}, \text{toxique}\}
\end{equation}
avec, pour chaque scénario $k$, la liste des produits impliqués et
des équipements concernés.

% ============================================================
\section{Étape 2 --- Masse équivalente par scénario}
% ============================================================

\subsection{Objectif}
Quantifier le danger porté par la matière elle-même, séparément pour
chaque scénario.

\subsection{Formule}
\begin{equation}
\boxed{
M_k = \sum_{j=1}^{n_p} w_{jk} \, m_j
}
\end{equation}

où~:
\begin{itemize}
    \item $m_j$ : masse réelle du produit $j$ (mesurée, kg),
    \item $w_{jk} = w_j \times s_{jk}$ : poids du produit $j$ pour le
    scénario $k$,
    \item $w_j$ : danger intrinsèque du produit (toxicité,
    inflammabilité, explosivité, réactivité --- tiré des FDS),
    \item $s_{jk} \in [0,1]$ : contribution du produit $j$ au
    scénario $k$ (un même produit ne contribue pas de la même façon
    à l'explosion, l'incendie, ou la toxicité).
\end{itemize}

\subsection{Sortie}
$M_{\text{expl}}, M_{\text{inc}}, M_{\text{tox}}$

% ============================================================
\section{Étape 3 --- Gravité physique}
% ============================================================

\subsection{Objectif}
Déterminer jusqu'où va l'effet du danger si le scénario se produit
--- par la physique, pas par ajustement statistique.

\subsection{Explosion (Taylor / TNT équivalent)}
\begin{equation}
\boxed{
G_{\text{expl}} = \left( \frac{E(M_{\text{expl}})}{P_{\text{seuil}}} \right)^{1/3}
}
\end{equation}
L'exposant $1/3$ vient de la loi d'échelle cubique de l'énergie
d'explosion --- il n'est pas ajusté.

\subsection{Incendie (flux thermique)}
\begin{equation}
\boxed{
G_{\text{inc}} = \sqrt{\frac{\dot{Q}(M_{\text{inc}})}{\pi \, q_{\text{seuil}}}}
}
\end{equation}

\subsection{Toxique (dispersion gaussienne, Pasquill-Gifford)}
\begin{equation}
C(x) = \frac{Q(M_{\text{tox}})}{2\pi \sigma_y \sigma_z u}
\exp\left( -\frac{x^2}{2\sigma_y^2} \right)
\end{equation}
\begin{equation}
\boxed{
G_{\text{tox}} = x \text{ tel que } C(x) = C_{\text{létal}}
}
\end{equation}

\subsection{Sortie}
$G_{\text{expl}}, G_{\text{inc}}, G_{\text{tox}}$ --- en mètres.

% ============================================================
\section{Étape 4 --- Probabilité de défaillance par équipement}
% ============================================================

\subsection{Objectif}
Quantifier la chance que chaque équipement tombe en panne, et relier
ce mode de défaillance au scénario qu'il peut déclencher.

\subsection{Taux de défaillance de base}
Pour chaque équipement $i$, un taux de défaillance $\lambda_i^{\text{mode}}$
par mode de défaillance (fuite, rupture, blocage...), tiré d'une
base de fiabilité reconnue (OREDA ou équivalent) --- pas inventé.

\subsection{Conversion en probabilité}
\begin{equation}
p_i^{\text{mode}}(t) = 1 - e^{-\lambda_i^{\text{mode}} \, t}
\end{equation}

\subsection{Lien mode de défaillance $\to$ scénario}
\begin{equation}
p_{ik} = \sum_{\text{mode}} p_i^{\text{mode}}(t) \times c_{\text{mode},k}
\end{equation}
où $c_{\text{mode},k} \in [0,1]$ est la probabilité conditionnelle
que ce mode de défaillance mène au scénario $k$.

\subsection{Correction par le niveau de sécurisation}
\begin{equation}
\boxed{
p_{ik}^{\text{corrigé}} = p_{ik} \times f_{\text{sécu}}(S_i)
}
\end{equation}
où $S_i \in [0,1]$ résume les mesures de sécurité en place sur
l'équipement $i$ (formation, maintenance, redondance, détection...),
et $f_{\text{sécu}}(S_i) \in [0.5, 1]$ diminue quand $S_i$ augmente.

% ============================================================
\section{Étape 5 --- Probabilité globale par scénario}
% ============================================================

\subsection{Objectif}
Agréger le risque de tous les équipements en une seule probabilité
par scénario.

\subsection{Formule}
\begin{equation}
\boxed{
P_k = 1 - \prod_{i=1}^{n_e} \left( 1 - p_{ik}^{\text{corrigé}} \right)^{n_i}
}
\end{equation}

\subsection{Sortie}
$P_{\text{expl}}, P_{\text{inc}}, P_{\text{tox}}$

% ============================================================
\section{Étape 6 --- Erreur résiduelle}
% ============================================================

\subsection{Objectif}
Corriger ce que le modèle physique de base ($G_k \times P_k$) ne
capture pas --- sans ajouter une boîte noire.

\subsection{Formule}
\begin{equation}
\boxed{
\varepsilon_k = \sum_{j=1}^{n_f} \beta_{jk} \, X_j
}
\end{equation}

où $X_j \in \{0,1\}$ indique la présence d'un facteur non modélisé
sur l'installation étudiée.

\subsection{Facteurs identifiés}

\begin{table}[H]
\centering
\caption{Facteurs correctifs de $\varepsilon_k$}
\begin{tabular}{@{}clc@{}}
\toprule
Code & Facteur & Scénario(s) concerné(s) \\
\midrule
$X_1$ & Conditions météo défavorables (inversion, vent faible) & Toxique \\
$X_2$ & Effets dominos (proximité d'autres réservoirs) & Explosion, incendie \\
$X_3$ & Écart entre masse déclarée et masse réelle stockée & Tous \\
$X_4$ & Topographie / confinement (cuvette, zone encaissée) & Toxique, explosion \\
$X_5$ & Facteur organisationnel (maintenance, formation) & Tous \\
$X_6$ & Mélange / réaction croisée entre substances incompatibles & Toxique, explosion \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Méthode de calibration}

\begin{enumerate}
    \item Choisir 4 à 6 accidents industriels réels documentés,
    si possible sur des installations similaires (chimie de
    l'ammoniac, engrais).
    \item Coder $X_j$ pour chaque accident (présence ou non du
    facteur).
    \item Calculer l'écart observé entre le rayon réel et le rayon
    calculé par le modèle physique de base~:
    \begin{equation}
    \varepsilon_{\text{total}} = \ln\!\left( \frac{R_{\text{réel}}}{R_{\text{calc}}} \right)
    \end{equation}
    \item Estimer $\beta_{jk}$ par jugement physique, en vérifiant
    que la somme des facteurs présents retombe proche de l'écart
    observé sur chaque accident de calibration.
    \item Appliquer les $\beta_{jk}$ calibrés à l'installation
    étudiée, en fonction des facteurs $X_j$ réellement présents.
\end{enumerate}

% ============================================================
\section{Étape 7 --- Risque physique}
% ============================================================

\subsection{Objectif}
Produire une sortie directement comparable à un seuil réglementaire.

\subsection{Formule}
\begin{equation}
\boxed{
R_k = G_k(M_k) \times P_k \times e^{\varepsilon_k}
}
\quad [\text{mètres}]
\end{equation}

La forme exponentielle de $\varepsilon_k$ garantit $R_k > 0$ quelle
que soit la valeur de l'erreur, positive ou négative.

\subsection{Décision de conformité}
\begin{equation}
R_k \lessgtr R_{\text{seuil}} \quad \Rightarrow \quad \text{conforme / non conforme}
\end{equation}

% ============================================================
\section{Étape 8 --- Coût de risque}
% ============================================================

\subsection{Objectif}
Produire une sortie en unité monétaire, pour comparer les scénarios
entre eux et prioriser les investissements de sécurité --- sans
mélanger cette sortie avec la précédente.

\subsection{Formule}
\begin{equation}
\boxed{
C_k = P_k \times K_k(G_k) \times e^{\varepsilon_k}
}
\quad [\text{argent}]
\end{equation}

où $K_k(G_k)$ convertit la zone d'effet physique en coût de dommages
(biens, production, santé, légal) --- fonction à construire à partir
de la densité de valeur réelle autour de l'installation.

% ============================================================
\section{Étape 9 --- Décision}
% ============================================================

\subsection{Objectif}
Conclure et recommander.

\subsection{Grille de décision}

\begin{table}[H]
\centering
\caption{Grille de décision}
\begin{tabular}{@{}ll@{}}
\toprule
Sortie & Usage \\
\midrule
$R_k$ vs $R_{\text{seuil}}$ & Conformité réglementaire (autoriser / refuser) \\
$C_k$ entre scénarios & Priorisation des investissements de sécurité \\
\bottomrule
\end{tabular}
\end{table}

% ============================================================
\section{Modèle complet --- récapitulatif}
% ============================================================

\begin{equation}
M_k = \sum_j w_{jk}\, m_j
\qquad\qquad
G_k(M_k) \text{ (physique)}
\end{equation}

\begin{equation}
p_{ik}^{\text{corrigé}} = p_{ik} \times f_{\text{sécu}}(S_i)
\qquad\qquad
P_k = 1 - \prod_i \left(1-p_{ik}^{\text{corrigé}}\right)^{n_i}
\end{equation}

\begin{equation}
\varepsilon_k = \sum_j \beta_{jk} X_j
\end{equation}

\begin{equation}
\boxed{R_k = G_k(M_k) \times P_k \times e^{\varepsilon_k}}
\qquad\qquad
\boxed{C_k = P_k \times K_k(G_k) \times e^{\varepsilon_k}}
\end{equation}

% ============================================================
\section{Passage aux données réelles --- Étape 2 en pratique}
% ============================================================

\subsection{Objectif de cette section}

La structure ci-dessus est maintenant complète. La suite du travail
consiste à la remplir avec les données réelles de l'installation,
en commençant par l'\textbf{Étape 2 (masse équivalente)}, brique la
plus en amont du modèle.

\subsection{Données réelles collectées --- premiers produits documentés}

Contrairement aux valeurs génériques ou inventées rencontrées dans
les versions précédentes du modèle, les données ci-dessous
proviennent de fiches de données de sécurité (FDS/SDS) et de sources
réglementaires reconnues (NIOSH, OSHA, ACGIH, EIGA).

\begin{table}[H]
\centering
\caption{Inventaire matière --- produits documentés par sources réelles}
\small
\begin{tabular}{@{}clllll@{}}
\toprule
Code & Produit & Danger dominant & $w_{j,\text{expl}}$ & $w_{j,\text{inc}}$ & $w_{j,\text{tox}}$ \\
\midrule
M1 & NH$_3$ liquide & Toxique & Faible & Faible-modéré & \textbf{Élevé} \\
M2 & CO$_2$ liquide & Physique (pression) & Modéré & Nul & Quasi nul \\
M8 & H$_2$ & \textbf{Explosion/incendie} & \textbf{Très élevé} & \textbf{Très élevé} & Nul \\
M9 & N$_2$ & Asphyxiant simple & Nul & Nul & Faible \\
M10 & O$_2$ & Aggravant (comburant) & Facteur multiplicatif & Facteur multiplicatif & Nul \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Justification --- seuils et propriétés réels}

\begin{table}[H]
\centering
\caption{Seuils et propriétés physiques réels, par produit}
\small
\begin{tabular}{@{}cll@{}}
\toprule
Produit & Propriété & Valeur \\
\midrule
NH$_3$ & IDLH (NIOSH, révisé) & 300 ppm \\
NH$_3$ & ERPG-3 (AIHA, risque létal, 1h) & 1\,000 ppm $\approx 0.000697$ kg/m$^3$ \\
NH$_3$ & Plage d'inflammabilité (LEL--UEL) & 15\% -- 28\% \\
NH$_3$ & Énergie minimale d'inflammation & 380 -- 680 mJ \\
CO$_2$ & IDLH & 40\,000 ppm $\approx 0.072$ kg/m$^3$ \\
CO$_2$ & Inflammabilité & Non inflammable \\
H$_2$ & Plage d'inflammabilité (LEL--UEL) & 4\% -- 74--77\% \\
H$_2$ & Énergie minimale d'inflammation & $\approx 0.02$ mJ \\
O$_2$ & Seuil d'atmosphère enrichie & 23.5\% en volume \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Note de correction méthodologique}

Ces données réelles corrigent une hypothèse implicite des versions
précédentes du modèle, qui attribuaient des poids de danger
génériques ou uniformes aux produits. Deux corrections importantes~:

\begin{itemize}
    \item Le NH$_3$, souvent traité comme le produit le plus
    dangereux au sens large, est avant tout un \textbf{risque
    toxique} --- sa plage d'inflammabilité étroite et son énergie
    d'inflammation élevée en font un risque explosion/incendie
    secondaire, sauf en cas de confinement prolongé ou de mélange
    avec des huiles.
    \item L'H$_2$, présent en faible masse (500~kg dans l'inventaire
    de départ), est au contraire le produit le \textbf{plus critique
    pour l'explosion et l'incendie} de toute l'installation, en
    raison de son énergie d'inflammation extrêmement basse et de sa
    plage d'inflammabilité très large.
\end{itemize}

\subsection{Tableau à compléter --- produits restants}

\begin{table}[H]
\centering
\caption{Inventaire matière --- structure prête à remplir}
\small
\begin{tabular}{@{}clccccc@{}}
\toprule
Code & Produit & $m_j$ (kg) & $w_j$ & $s_{j,\text{expl}}$ & $s_{j,\text{inc}}$ & $s_{j,\text{tox}}$ \\
\midrule
M1 & NH$_3$ liquide & \ldots & voir ci-dessus & \ldots & \ldots & \ldots \\
M2 & CO$_2$ liquide & \ldots & voir ci-dessus & \ldots & \ldots & \ldots \\
M3 & Urée solution & \ldots & \ldots & \ldots & \ldots & \ldots \\
M4 & Urée solide & \ldots & \ldots & \ldots & \ldots & \ldots \\
M5 & Carbamate & \ldots & \ldots & \ldots & \ldots & \ldots \\
M6 & Huile & \ldots & \ldots & \ldots & \ldots & \ldots \\
M7 & Vapeur & \ldots & \ldots & \ldots & \ldots & \ldots \\
M8 & H$_2$ & \ldots & voir ci-dessus & \ldots & \ldots & \ldots \\
M9 & N$_2$ & \ldots & voir ci-dessus & \ldots & \ldots & \ldots \\
M10 & O$_2$ & \ldots & voir ci-dessus & \ldots & \ldots & \ldots \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Sources à mobiliser pour compléter ce tableau}

\begin{itemize}
    \item $m_j$ : relevé réel sur site (capacité de stockage,
    inventaire de procédé) --- seule donnée que ni la recherche
    documentaire ni un modèle ne peuvent fournir à la place de
    l'exploitant.
    \item Urée, carbamate, huile, vapeur : le carbamate d'ammonium
    en particulier est un intermédiaire de procédé sans fiche de
    sécurité publique standardisée --- fiche technique interne du
    procédé requise.
    \item $s_{jk}$ : expertise HSE de l'installation --- quel
    produit contribue à quel scénario, et dans quelle mesure.
\end{itemize}

\subsection{Interaction critique identifiée}

Le mélange H$_2$ + O$_2$ (mélange oxhydrique) constitue un cas
concret du facteur $X_6$ (mélange/réaction croisée entre substances
incompatibles) défini à l'Étape 6 --- à intégrer explicitement dans
$\varepsilon_{\text{expl}}$ pour cette installation.

\subsection{Prochaine étape}

Une fois les masses réelles $m_j$ relevées et $M_k$ calculé pour les
trois scénarios, l'Étape 3 (gravité physique $G_k$) pourra être
instanciée avec les constantes physiques réelles ci-dessus,
notamment $C_{\text{létal}}^{\text{NH}_3} \approx 0.000697$ kg/m$^3$
pour le scénario toxique.

\end{document}
