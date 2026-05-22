# lab-14
#  Android Root Detection Analysis & Bypass

Ce projet présente une méthodologie d'analyse dynamique et de contournement (*bypass*) des mécanismes de détection de root sur une application Android cible (`owasp.mstg.uncrackable`), en utilisant le framework d'instrumentation dynamique **Frida**.

---

##  Objectifs
* Analyser les mécanismes de sécurité anti-root (Java et Natifs) d'une application Android à l'exécution.
* Comprendre le comportement du processus face à un environnement de test/debug.
* Développer et injecter un script Frida permettant de neutraliser ces vérifications.

---

##  Environnement & Outils
* **Émulateur Android (AVD)** (Image système avec privilèges root/debug)
* **ADB (Android Debug Bridge)** configuré
* **Frida** (Frida-client sur la machine hôte + `frida-server` sur l'émulateur)
* **JADX-GUI** (Pour la rétro-ingénierie et la lecture du code source décompilé)

---

##  Méthodologie & Étapes

### Étape 1 — Vérification de l'environnement
Avant toute manipulation, on s'assure que l'appareil est correctement détecté par ADB et que Frida communique avec le serveur distant :
```bash
adb devices
frida-ps -Uai
```
---
### Étape 2 — Identification du package cible
On repère l'identifiant unique de l'application (Package Name) :
```bash
Uncrackable Level 1     owasp.mstg.uncrackable
```
### Étape 3 — Phase d'Observation (Analyse Statique & Dynamique)
Un script initial observer.js a été déployé pour surveiller les indicateurs clés :

Propriétés système : Build.FINGERPRINT, Build.TAGS

Appels API Java : File.exists(), Runtime.exec()

Appels système natifs : open(), access(), stat()
```bash
frida -U -f owasp.mstg.uncrackable -l observer.js
```
```bash
Build.FINGERPRINT = google/sdk_gphone_x86/...:userdebug/dev-keys
File.exists(/system/xbin/su) => true
```
<img width="142" height="392" alt="1A" src="https://github.com/user-attachments/assets/4785ae1e-96c5-46da-b420-526c717336c2" />
<img width="201" height="287" alt="image" src="https://github.com/user-attachments/assets/47ac24e4-4b03-429c-9b08-aebd4d16bed4" />


```
