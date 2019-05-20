# Azure Serverless

---

## Serverless ?

---

* <del>Serverless</del>
* Less servers <!-- .element: class="fragment" -->

note:
* moins de config/gestion
* managé par un mécanisme externe
* concentrer sur le code

---

## On premise
* Hardware <!-- .element: class="fragment" -->

note:
* rappel offres hébergement =>
    * qu'est-ce que je gère ?
* pannes matérielles
* firewall / réseau

---

## IaaS
* <del>Hardware</del>
* OS <!-- .element: class="fragment" -->

note:
* je gère pannes logicielles
* mises à jour

---

# PaaS
* <del>Hardware</del>
* <del>OS</del>
* Framework <!-- .element: class="fragment" -->

note:
* montées de version
* archi/solutions logicielles (bases, messaging, etc.)
* scalabilité

---

# FaaS
* <del>Hardware</del> 
* <del>OS</del>
* <del>Framework</del>
* Function <!-- .element: class="fragment" -->

note:
* code

---

# Implémentations

* Azure Functions
* AWS Lambda <!-- .element: class="fragment" -->
* Google Functions <!-- .element: class="fragment" -->

note:
* Open FaaS

---

# Azure Functions

note:
* Implémentation Microsoft
* Ecosystème Azure

---

# Avantages
* Infinite scaling

note:
* promesse de scalabilité
* vs VM pré-provisionnée (black-friday)

---

# Avantages
* Pay as you go

note:
* 1 million d'exécutions gratuites
* 0 exécution = 0€

---

# Propriétés
* Event-driven

---

# Use cases

* Loterie bons de réduction

note:
* un fonctionnalité spécifique avec bcp de trafic
* qu'on ne connaît pas à l'avance

---

# Use cases

* Event-sourcing

note:
* nb d'événements non connus à l'avance

---

# Use cases

* Détecter la modification de resources Azure

---

# Contreparties
* Stateless

note:
* lié au framework
* contournable
* durable functions

---

# Contreparties
* Durée limitée

note:
* 10 min max par exécution

---

# Contreparties
* Cold start

note:
* instance démarée automatiquement / éteinte si pas d'activité
* c^ orchestrateurs
* adapter sa stratégie

---

# Opiniated framework
* Dirigiste
* Accès simplifié aux resources

---

# TODO
* Exemple trigger HTTP (C#)
* Exemple trigger schedule (Java)
* Exemple trigger base (NodeJS)

---

# Triggers
* Http / Webhooks
* Bus de messages
* Bases de données
* Scheduler
* etc.

--- 

# Resources
* Bus de messages
* Bases de données
* Key Vault
* Azure AD

--- 

# Langages
* C#
* JavaScript
* Java
* Python

note:
* C# et JavaScript ++
* C# script

---

# Configuration
* function.json
* env vars
* Key Vault

---

# Attributes (annotations)
* Pour les triggers et les bindings
* C# et Java

---

# Dev en local

* Azure functions tools
* VS / VS Code
* Maven
* Emulateurs (Cosmos)

note:
* Resources Azure ? Service bus

---

# Function apps
* groupement logique de fonctions
* durable functions

---

# Déploiement

* Templates ARM
* Azure DevOps

---

# Retour d'expérience
* Code simple et direct
* Fonctions simples (micro-services)

---

# Manques
* Healthchecks

---

# TODO Durable functions

---

# TODO Logic Apps 

