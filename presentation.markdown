# Azure Serverless

---

# Serverless ?

---

* <del>Serverless</del>
* Less servers <!-- .element: class="fragment" -->

note:
* ↘ de config, de gestion
* managé
* focus sur le code

---

## On premise
* Hardware <!-- .element: class="fragment" -->

note:
* je gère : je m'occupe
* pannes matérielles
* réseau
    * firewall
    * FAI

---

## IaaS
* <del>Hardware</del>
* OS <!-- .element: class="fragment" -->

note:
* pannes logicielles
* mises à jour

---

## PaaS
* <del>Hardware</del>
* <del>OS</del>
* Framework <!-- .element: class="fragment" -->

note:
* montées de version
* archi logicielles (bases, messaging, etc.)
* scalabilité

---

## FaaS
* <del>Hardware</del> 
* <del>OS</del>
* <del>Framework</del>
* Function <!-- .element: class="fragment" -->

note:
* code

---

## Functions
* Azure Functions
* AWS Lambda <!-- .element: class="fragment" -->
* Google Functions <!-- .element: class="fragment" -->

note:
* sous-partie de serverless
* Open FaaS

---

## Azure Functions
* Event-driven
* Serverless <!-- .element: class="fragment" -->

note:
* Microsoft
* Ecosystème Azure

---

## Avantages
* Infinite scaling

note:
* vs VM pré-provisionnée
* eg black-friday

---

## Avantages
* Pay as you go

note:
* 1 million d'exécutions gratuites
* 0 exécution = 0€
* exemple UAT, testing

---

# Use cases ?

---

## API HTTP
* microservices <!-- .element: class="fragment" -->

note:
* HTTP = événement

---

## TODO
* Exemple trigger HTTP (Java)

note:
* troll

---

## TODO
* Exemple trigger HTTP (C#)

---

## API HTTP
* Loterie bons de réduction

note:
* != full serverless/microservices 💡
* un fonctionnalité avec potentiellement bcp de trafic ?

---

## API HTTP
* back-end pour une SPA <!-- .element: class="fragment" -->

note:
* exemple avec static web storage
* proxy 💡

---

# Use cases ?

---

## Event sourcing

TODO

note:
* volumétrie non connues à l'avance

---

## TODO
* Exemple trigger Service Bus (C#)

---

## Event sourcing

* Réagir aux modifications de resources Azure

---

## TODO

* Exemple resource BDD (NodeJS)
* Entrée et sortie

---

# Opiniated Framework

note:
* Déclarations simplifiées

---

## Triggers
* Http / Webhooks
* Bus de messages <!-- .element: class="fragment" -->
* Bases de données <!-- .element: class="fragment" -->
* Scheduler <!-- .element: class="fragment" -->
* etc.

---

## Resources
* Bus de messages
* Bases de données <!-- .element: class="fragment" -->
* Key Vault <!-- .element: class="fragment" -->
* Azure AD <!-- .element: class="fragment" -->

---

## Langages
* C#
* JavaScript <!-- .element: class="fragment" -->
* Java <!-- .element: class="fragment" -->
* Python <!-- .element: class="fragment" -->
* PowerShell <!-- .element: class="fragment" -->

note:
* C# et JavaScript ++
* C# script

---

## Attributes (annotations)
* Pour les triggers et les bindings
* C# et Java

---

## 🌟 IoC

TODO

--- 

# Avantages

---

## Configuration simplifiée
* function.json 
* Variables d'environnement <!-- .element: class="fragment" -->
* 🌟 Azure Key Vault <!-- .element: class="fragment" -->

---

## En local
* Azure functions tools (CLI) 💡
* VS, VS Code <!-- .element: class="fragment" -->
* Maven <!-- .element: class="fragment" -->
* Emulateurs (Cosmos) <!-- .element: class="fragment" -->

note:
* != des autres clouds
* Resources Azure ? Service bus

---

# Inconvénients 

---

## Stateless

note:
* lié au framework
* contournable 🤔 (durable functions, cache partagé) 

---

## Durée limitée

note:
* 10 min max par exécution

---

## Cold start

note:
* instance démarée automatiquement / éteinte si pas d'activité
* qq secondes, en théorie 💡
* c^ orchestrateurs
* adapter sa stratégie 🤔 (polling, 🌟 Azure Front Door) 💡
* 🌟 Functions premium

---

## Manquent...
* Healthcheck
* Swagger

note:
* swagger manuel

---

## Networking
* 🌟 VNET / ASE 💡
* DNS

---

## Pricing

note:
* Avoir une idée de la volumétrie

---

# Ecosystème

---

## Application Insights 💡

---

## Déploiement
* Templates ARM 💡
* Azure DevOps
    * 🌟 job Functions <!-- .element: class="fragment" -->

note:
* push depuis poste local 

---

## Durable functions

* 🌟 Durable Entities <!-- .element: class="fragment" -->
---

## Logic Apps 

---

# Merci

💬
