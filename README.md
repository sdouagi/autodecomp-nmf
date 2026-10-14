# 🎵 AutoDecomp — séparation de sources audio par NMF

Séparer automatiquement les différentes sources d'un enregistrement audio (instruments, voix, bruits) grâce à la **factorisation en matrices non-négatives (NMF)**, implémentée à la main en **Python**.

> Projet intégratif *Méthodes numériques pour l'ingénieur* × *Traitement du signal*, **INSA Rouen Normandie** (2024-2025)

---

## 🔬 Principe

```
Signal .wav  →  Spectrogramme (STFT)  →  NMF : V ≈ W·H  →  Filtrage par source  →  STFT inverse  →  Sources séparées
```

1. **Spectrogramme** : transformée de Fourier à court terme, codée à la main puis comparée à `scipy`. Les fenêtres (Hanning, Blackman…), leur taille et le saut ont été choisis par expérimentation.
2. **NMF** : on décompose le spectrogramme `V` en deux matrices non-négatives :
   - `W` : le dictionnaire, qui donne la signature fréquentielle de chaque source
   - `H` : les activations, qui disent quand chaque source joue
3. **Optimisation** par mises à jour **multiplicatives alternées**, en minimisant la **β-divergence**. Les trois cas comparés :
   - β = 0 : Itakura-Saito
   - β = 1 : Kullback-Leibler
   - β = 2 : distance euclidienne
4. **Reconstruction** : un masque doux isole chaque source dans le spectrogramme, puis on revient au signal temporel par STFT inverse.

## 📊 Résultats

| Son | Sources | Observation |
|---|---|---|
| Mélange de piano | 3 notes | **β = 0 (Itakura-Saito)** donne la meilleure séparation |
| Cheval + conversation | 2 | Les deux sources sont bien isolées |

**Pistes d'amélioration étudiées :** initialisation par **SVD** plutôt qu'aléatoire, plusieurs initialisations pour éviter les minima locaux, comparaison avec l'**ALS** (*Alternating Least Squares*).

## 📁 Structure

```
code/
├── rapport.ipynb          # Notebook principal : théorie, implémentation, résultats
├── decomposition_*.ipynb  # Expériences sur chaque fichier son
├── ALS.ipynb              # Comparaison avec l'ALS
└── Fichiers_sonores/      # Sons de test (.wav)
ressources/                # Articles de référence
```

## ▶️ Lancer le projet

```bash
pip install numpy scipy matplotlib librosa scikit-learn jupyter
jupyter notebook code/rapport.ipynb
```

La fin du notebook permet de tester la séparation sur **son propre fichier audio** (il faut connaître le nombre de sources).

## 🛠️ Technologies

`Python` · `NumPy` · `SciPy` · `librosa` · `Jupyter` · traitement du signal · optimisation numérique
