# Talent Tree (skills2)

## ⭐ Version stable de référence

**La version fonctionnelle de référence est la branche `claude/pull-latest-updates-etuXp` (commit `31a6543`), aussi marquée par le tag `v1.0-stable`.**

Cette version est validée : les bonus de talents (ENDU, VIE, DÉGÂTS, PV) s'appliquent
correctement aux caractéristiques du personnage. C'est l'état à conserver comme point
de retour si une future branche pose problème.

### Revenir à la version stable en cas de pépin

```bash
# Voir la version stable sans rien modifier
git checkout v1.0-stable

# Ou repartir proprement d'une nouvelle branche basée sur la version stable
git checkout -b ma-nouvelle-branche v1.0-stable
```

Le tag `v1.0-stable` pointe **toujours** vers le commit exact `31a6543`, même si les
branches évoluent ou sont supprimées.

---

## Description de l'application

**Talent Tree (skills2)** est un éditeur/simulateur d'arbres de talents pour jeu de
rôle, conçu pour permettre aux joueurs de construire visuellement leur personnage en
débloquant des talents organisés en graphe. L'application repose sur une architecture
React autonome (Babel standalone pour compilation in-browser, aucun build pipeline)
avec persistance complète en `localStorage` et partage d'URL via compression LZString.

### Fonctions principales

1. Édition visuelle par drag-and-drop des talents et leurs prérequis
2. Système de calcul de caractéristiques (ENDU, VIE, DÉGÂTS, PV) basé sur des effets
   spéciaux configurables (`effetsSpeciaux[]`)
3. Gestion de 8 classes de personnages avec leurs arbres distincts
4. Marqueurs personnalisés dynamiques
5. Export/import JSON et partage via URL

### Philosophie de codage

La philosophie privilégie la **simplicité de déploiement** (un seul fichier HTML
statique sur GitHub Pages) et la **robustesse des données** (format JSON lisible,
backward compatibility).

### Risques majeurs de modification

1. **Format de données** : toute refonte du format peut briser les sauvegardes
   `localStorage` des utilisateurs existants (comme le conflit `effetSpecial` /
   `effetsSpeciaux` résolu dans cette version).
2. **Babel/React** : la configuration est fragile (version `7.25.9` figée pour éviter
   les pages blanches).
3. **Chemins Jekyll** : `docs/talent-tree.html` est déployé, `talent-tree.html` en
   racine ne l'est pas — attention à ne pas confondre.
4. **Calculs interdépendants** : DÉGÂTS utilise `getTotalEndu()`, ce qui crée un risque
   de récursion si mal modifié.
5. **Partage d'URL** : la compression peut échouer silencieusement si le format JSON
   change sans migration.

> Tout changement structurel doit inclure une migration automatique des données
> existantes et tester la rétrocompatibilité.
