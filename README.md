# 🧭 TP Packet Tracer — Comprendre le routage et la distance administrative

> TP progressif pour comprendre **comment un routeur choisit une route**.

---

# 🎯 Objectifs pédagogiques

À la fin de ce TP, vous devrez être capables de :

✅ Lire une table de routage  
✅ Différencier routes connectées / statiques / dynamiques  
✅ Comprendre la distance administrative (AD)  
✅ Comparer RIP et OSPF  
✅ Expliquer pourquoi un chemin est choisi

---

# 🧠 Rappel théorique simple

Un routeur choisit une route selon :

1️⃣ Distance administrative (priorité du protocole)  
2️⃣ Métrique du protocole (coût interne)  
3️⃣ Premier chemin valide trouvé

👉 **Plus l’AD est petite, plus la route est prioritaire.**

---

# 🗺️ Topologie du TP

...
(Chemin A)
PC1 --- R1 -------- R2 -------- R3 --- PC2
        |                       |
        |                       |
        -------- R4 -------------
(Chemin B)


## Lecture du schéma

- PC1 veut joindre PC2  
- Deux chemins possibles :
  - Chemin A : R1 → R2 → R3  
  - Chemin B : R1 → R4 → R3  
- Les protocoles décideront du chemin

---

# 🌐 Plan d’adressage

| Lien | Réseau |
|------|--------|
PC1–R1 | 192.168.1.0/24 |
R1–R2 | 10.0.12.0/24 |
R2–R3 | 10.0.23.0/24 |
R1–R4 | 10.0.14.0/24 |
R4–R3 | 10.0.43.0/24 |
R3–PC2 | 192.168.2.0/24 |

---

# 🧩 Matériel Packet Tracer

- 4 routeurs 2911  
- 2 PC  
- Câbles cuivre droits  

---

# 🔹 PARTIE 1 — Routes directement connectées

## Étape 1 : Configurer les IPs

Attribuez toutes les adresses IP selon le tableau.

Activez les interfaces :
```
no shutdown
```
---

## Vérification
```
show ip route
```

---

## À observer

Vous voyez des routes avec la lettre **C**.

### Questions (Répondez directement dans ce Readme.md)

1. Que signifie C ?  
2. Pourquoi ces routes existent-elles automatiquement ?  
3. Quelle est leur AD ?

---

# 🔹 PARTIE 2 — Routes statiques

## Configuration

### Sur R1
```
ip route 192.168.2.0 255.255.255.0 10.0.12.2
```

### Sur R3
```
ip route 192.168.1.0 255.255.255.0 10.0.23.1
```

---

## Test

Ping PC1 → PC2.

---

## Questions

1. Quelle est l’AD d’une route statique ?  
2. Le chemin utilisé est-il A ou B ?  
3. Que se passe-t-il si on coupe le lien R1–R2 ?

---

# 🔹 PARTIE 3 — RIP

> RIP = protocole simple basé sur le nombre de sauts.

---

## Configuration (sur tous les routeurs)

```
router rip
version 2
network 10.0.0.0
network 192.168.0.0
no auto-summary
```

---

## Vérification
```
show ip route
```

Routes marquées **R**.

---

## Questions

1. Quelle est l’AD de RIP ?  
2. Quelle métrique utilise RIP ?  
3. Quel chemin RIP préfère-t-il ?

---

# 🔹 PARTIE 4 — OSPF

> OSPF choisit le meilleur coût (lié à la bande passante).

---

## Supprimer RIP
```
no router rip
```

---

## Activer OSPF

```
router ospf 1
network 10.0.0.0 0.255.255.255 area 0
network 192.168.0.0 0.0.255.255 area 0
```

---

## Questions

1. Quelle lettre représente OSPF ?  
2. Quelle est son AD ?  
3. Pourquoi OSPF choisit-il un chemin ?

---

# 🔹 PARTIE 5 — Conflit de routes

Activez simultanément :

- Route statique  
- RIP  
- OSPF  

---

## Question clé

Pourquoi une seule route apparaît-elle dans la table ?

---

# 📘 CORRIGÉ ENSEIGNANT

## Routes connectées
- C = Connected  
- AD = 0  

---

## Routes statiques
- AD = 1  
- Prioritaires sur RIP/OSPF  

---

## RIP
- Lettre R  
- AD = 120  
- Métrique = nombre de sauts  

---

## OSPF
- Lettre O  
- AD = 110  
- Métrique = coût  

---

## Règle d’or

| Type | AD |
|------|----|
Connectée | 0 |
Statique | 1 |
EIGRP | 90 |
OSPF | 110 |
RIP | 120 |

👉 Le routeur garde l’AD la plus faible.

---

# 📝 Grille d’évaluation (/20)

| Critère | Points |
|--------|-------|
Adressage correct | 4 |
Routes statiques OK | 4 |
RIP fonctionnel | 4 |
OSPF fonctionnel | 4 |
Explication AD | 4 |

---

# ⭐ Bonus

Configurer une **route flottante** :

```
ip route 192.168.2.0 255.255.255.0 10.0.14.2 130
```

Question :
Pourquoi n’est-elle utilisée qu’en secours ?

---

# 🚀 Pour aller plus loin

- Tester EIGRP  
- Ajouter BGP  
- Simuler des pannes de liens  
- Observer la convergence

---

# ✅ Fin du TP

Si vous savez expliquer :

> "Pourquoi ce routeur choisit ce chemin ?"

Alors vous avez compris le routage 👍












