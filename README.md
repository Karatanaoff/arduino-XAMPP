# 🌍 Station Météo Arduino & Dashboard Web (IoT)

Ce projet est un système IoT (Internet of Things) complet de bout en bout. Il permet à un Arduino Nano de lire la température d'une pièce via un capteur DS18B20, de l'envoyer sur le réseau local via un shield Ethernet, et d'afficher les données en temps réel sur un tableau de bord web (avec graphique et historique).

## 🛠️ Matériel Requis

* **Microcontrôleur :** Arduino Nano
* **Réseau :** Module Ethernet (ex: ENC28J60)
* **Capteur :** DS18B20 (Capteur de température numérique)
* **Composant :** Résistance de 4.7 kΩ (Uniquement si le capteur est "nu", inutile si c'est un module KY-001)
* **Câblage :** Câble RJ45, Câble USB (branché à l'arrière du PC pour une alimentation stable)

## 💻 Logiciels & Prérequis

* **Serveur local :** [XAMPP](https://www.apachefriends.org/fr/index.html) (Apache & MySQL)
* **IDE :** Arduino IDE (v2.x recommandée)
* **Bibliothèques Arduino :**
  * `UIPEthernet` (ou `Ethernet` selon le shield)
  * `OneWire` (par Jim Studt, Paul Stoffregen...)
  * `DallasTemperature` (par Miles Burton)

---

## ⚙️ 1. Configuration de la Base de Données (MySQL)

1. Lancer **XAMPP** et démarrer les modules **Apache** et **MySQL**.
2. Ouvrir le navigateur et aller sur `http://localhost/phpmyadmin/`.
3. Créer une base de données nommée `arduino_projet`.
4. Créer une table nommée `mesures` avec les colonnes suivantes :
   * `id` : INT, Auto-Incrément (A_I), Clé Primaire.
   * `valeur` : INT.
   * `date_heure` : TIMESTAMP (Valeur par défaut : CURRENT_TIMESTAMP).

---

## 🌐 2. Configuration du Serveur Web (PHP)

Dans le dossier `C:\xampp\htdocs\`, placer les deux fichiers suivants :

### A. `sauvegarder.php` (L'API de réception)
Ce script est appelé par l'Arduino pour insérer les données dans la base.
*(Penser à y configurer les identifiants de la base de données : `$utilisateur = "root"`, `$mot_de_passe = "ton_mot_de_passe"`).*

### B. `index.php` (Le Tableau de Bord)
Interface publique affichant :
* La température en direct.
* Un graphique dynamique généré avec **Chart.js**.
* Un tableau d'historique des 20 dernières mesures.
* *Fonctionnalité : Auto-refresh toutes les 5 secondes.*

---

## 🔌 3. Câblage de l'Arduino

| Capteur DS18B20 | Arduino Nano | Remarque |
| :--- | :--- | :--- |
| **GND** (-) | GND | |
| **VCC** (+) | 5V | |
| **DATA** (Signal) | D5 | Placer une résistance 4.7kΩ entre VCC et DATA |

---

## 🚀 4. Configuration et Déploiement

1. Dans le code source Arduino (`.ino`), modifier l'adresse IP du serveur XAMPP pour qu'elle corresponde à celle de la machine hôte :
   ```cpp
   IPAddress server(192, 168, X, X); // Remplacer par l'IP locale du PC
