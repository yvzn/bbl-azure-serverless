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
* qu'est-ce que je gère ?
* pannes matérielles
* firewall / réseau

---

## IaaS
* <del>Hardware</del>
* OS <!-- .element: class="fragment" -->

note:
* qu'est-ce que je gère ?
* pannes logicielles
* mises à jour

---

# PaaS
* <del>Hardware</del>
* <del>OS</del>
* Framework <!-- .element: class="fragment" -->

note:
* qu'est-ce que je gère ?
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
* qu'est-ce que je gère ?
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
* Event-driven
* Serverless <!-- .element: class="fragment" -->

note:
* Implémentation Microsoft
* Ecosystème Azure

---

# Avantages
* Infinite scaling

note:
* promesse
* vs VM pré-provisionnée
* eg black-friday

---

# Avantages
* Pay as you go

note:
* 1 million d'exécutions gratuites
* 0 exécution = 0€

---

# Quels use cases ?
* back-end pour une SPA <!-- .element: class="fragment" -->

note:
* avec static web storage

---

# Quels use cases ?
* Loterie bons de réduction

note:
* un fonctionnalité spécifique avec bcp de trafic
* qu'on ne connaît pas à l'avance

---

# Quels use cases ?
* Réagir aux modifications de resources Azure

---

# Quels use cases ?
* Event-sourcing

note:
* nb d'événements non connus à l'avance

---

# Inconvénients
* Stateless

note:
* lié au framework
* contournable (durable functions, cache partagé)

---

# Inconvénients
* Durée limitée

note:
* 10 min max par exécution

---

# Inconvénients
* Cold start

note:
* instance démarée automatiquement / éteinte si pas d'activité
* c^ orchestrateurs
* adapter sa stratégie

---

# Opiniated framework
* Déclarations simplifiées 

---

# TODO
* Exemple trigger HTTP (C#)
* Exemple trigger BDD (Java)

---

# Triggers
* Http / Webhooks
* Bus de messages <!-- .element: class="fragment" -->
* Bases de données <!-- .element: class="fragment" -->
* Scheduler <!-- .element: class="fragment" -->
* etc.

--- 

# Opiniated framework
* Accès simplifié aux resources

---

# Resources
* Bus de messages
* Bases de données <!-- .element: class="fragment" -->
* Key Vault <!-- .element: class="fragment" -->
* Azure AD <!-- .element: class="fragment" -->

---

# TODO
* Exemple resource Service Bus (C#)
* Exemple resource BDD (NodeJS)

--- 

# Langages
* C#
* JavaScript <!-- .element: class="fragment" -->
* Java <!-- .element: class="fragment" -->
* Python <!-- .element: class="fragment" -->
* PowerShell <!-- .element: class="fragment" -->

note:
* C# et JavaScript ++
* C# script

---

# Attributes (annotations)
* Pour les triggers et les bindings
* C# et Java

---

# Configuration
* function.json 
* Variables d'environnement <!-- .element: class="fragment" -->
* Azure Key Vault <!-- .element: class="fragment" -->

---

# En local
* Azure functions tools
* VS / Code <!-- .element: class="fragment" -->
* Maven <!-- .element: class="fragment" -->
* Emulateurs (Cosmos) <!-- .element: class="fragment" -->

note:
* Resources Azure ? Service bus

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

# Durable functions

---

# Logic Apps 

---

# Merci

