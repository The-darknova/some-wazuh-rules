# 📘 Règles Événements Windows

## 🧩 Règle Racine

### Règle `100100` – Niveau 0
- **Champ** : `data.win.eventdata.elevatedToken = yes`
- **Description** : Règle parente pour les règles d'événements privilégiés.

---

## 🔐 Événements de Connexion Utilisateur Windows

### ✅ Connexions Réussies

#### Règle `100101` – Niveau 4
- **Basée sur** : `if_sid = 18107`
- **Critère** : `Logon Type: 2`
- **Description** : Connexion interactive physique réussie.
- **MITRE ATT&CK** : `T1078`
- **Groupe** : `windows`, `authentication_success`

#### Règle `100102` – Niveau 12
- **Basée sur** : `if_sid = 18107`
- **Critère** : `Logon Type: 4`
- **Description** : Connexion par lot réussie au nom d'un utilisateur.
- **MITRE ATT&CK** : `T1569`, `T1053`
- **Groupe** : `windows`, `authentication_success`

#### Règle `100103` – Niveau 3
- **Basée sur** : `if_sid = 18107`
- **Critère** : `Logon Type: 5`
- **Description** : Un service a été démarré par le gestionnaire de services.
- **MITRE ATT&CK** : `T1569`, `T1053`
- **Groupe** : `windows`, `authentication_success`

#### Règle `100104` – Niveau 3
- **Critère** : `Logon Type: 7`
- **Description** : Ce poste de travail a été déverrouillé.
- **MITRE ATT&CK** : `T1078`
- **Groupe** : `windows`, `authentication_success`

#### Règle `100105` – Niveau 13
- **Critère** : `Logon Type: 8`
- **Description** : Le mot de passe de l'utilisateur a été transmis en clair au package d'authentification.
- **MITRE ATT&CK** : `T1569`, `T1078`
- **Groupe** : `windows`, `authentication_success`

#### Règle `100106` – Niveau 12
- **Critère** : `Logon Type: 9`
- **Description** : Connexion Windows avec de nouvelles informations d'identification (RunAs /netonly).
- **MITRE ATT&CK** : `T1569`, `T1078`
- **Groupe** : `windows`, `authentication_success`

#### Règle `100107` – Niveau 10
- **Critère** : `Logon Type: 10`
- **Description** : Connexion RDP interactive réussie.
- **MITRE ATT&CK** : `T1078`
- **Groupe** : `windows`, `authentication_success`

#### Règles `100108` et `100109` – Niveau 10
- **Basée sur** : `if_sid = 18119`
- **Description** : Première connexion d'un utilisateur privilégié à ce système.
- **MITRE ATT&CK** : `T1078`
- **Groupe** : `windows`, `authentication_success`, `privileged`

---

### 🔐 Connexions Réussies d’Utilisateurs Privilégiés

> Ces règles s’appuient sur les connexions réussies précédentes combinées à la règle parente `100100`.

#### Règle `100110` – Connexion Interactive Physique Privilégiée
- **MITRE** : `T1078`, `T1668`
- **Niveau** : 14

#### Règle `100111` – Connexion par lot d’un utilisateur privilégié
- **MITRE** : `T1068`, `T1569`, `T1053`
- **Niveau** : 14

#### Règle `100112` – Démarrage de service avec privilèges
- **MITRE** : `T1068`, `T1569`, `T1053`
- **Niveau** : 4

#### Règle `100113` – Déverrouillage de session d’un administrateur
- **MITRE** : `T1078`
- **Niveau** : 4

#### Règle `100114` – Transmission du mot de passe admin en clair
- **MITRE** : `T1078`, `T1569`
- **Niveau** : 14

#### Règle `100115` – Connexion avec nouvelles identifiants privilégiés (RunAs)
- **MITRE** : `T1078`, `T1134`
- **Niveau** : 9

#### Règle `100116` – Connexion RDP privilégiée
- **MITRE** : `T1078`
- **Tactique MITRE** : `Persistence`
- **Niveau** : 12

#### Règle `100117` – Première connexion système d’un utilisateur privilégié
- **MITRE** : `T1078`
- **Niveau** : 10

---

### ❌ Échecs de Connexion

#### Règle `100118` – Tentative de connexion échouée
- **Basée sur** : `Event ID 4625`
- **MITRE** : `T1110`
- **Niveau** : 10

#### Tentatives de Connexions Répétées (Brute Force)

- `100119` – 3 tentatives en 60s → Niveau 6
- `100120` – 5 tentatives en 60s → Niveau 7
- `100121` – 10 tentatives en 60s → Niveau 8
- `100122` – 12 tentatives en 60s → Niveau 12
- **MITRE commun** : `T1110`
- **Groupe** : `authentication_failed`, `brute_force`

---

## 👤 Gestion des Comptes

| ID | Description | Événements | Niveau | MITRE |
|----|-------------|------------|--------|-------|
| 100123 | Création d’un compte utilisateur | 624, 4720 | 13 | T1136 |
| 100124 | Activation d’un compte utilisateur | 626, 4722 | 13 | T1098 |
| 100125 | Tentative de réinitialisation de mot de passe | 628 | 13 | T1098 |
| 100126 | Modification de compte utilisateur | 685, 4738, 4781 | 13 | T1098 |
| 100127 | Suppression d’un compte utilisateur | 630, 4726 | 13 | T1098 |
| 100128 | Désactivation d’un compte utilisateur | 629, 4725 | 13 | T1098 |
| 100129 | Ajout à `Domain Admins` | Log message match | 14 | T1098 |
| 100130 | Modification de compte machine | 646, 4742 | 14 | T1098 |
| 100131 | Ajout de compte machine | 645, 4741 | 14 | T1098 |
| 100132 | Suppression de compte machine | 647, 4743 | 14 | T1098 |

---

## 🧾 Intégrité des Journaux & Services

#### Règle `100133` – Nettoyage des journaux d’audit
- **MITRE** : `T1070.001`
- **Niveau** : 15

#### Règle `100134` – Installation de service
- **MITRE** : `T1071`
- **Niveau** : 13

---

> 📌 **Note** : Les événements liés à la gestion opérationnelle Windows sont mentionnés mais non détaillés dans cette version.

