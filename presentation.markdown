# azure serverless

---

## serverless

---

## <del>serverless</del>
less servers

note:
* ↘ de config, de gestion
* managé
* focus sur le code

---

## serveurs  ?

note:
* c^ déployer une application
* je gère, je m'occupe

---

## on premise
* hardware <!-- .element: class="fragment" -->

note:
* avantages vs. inconvénients
* pannes matérielles
* réseau (firewall, fai)

---

## iaas
* <del>hardware</del>
* os <!-- .element: class="fragment" -->

note:
* pannes logicielles
* mises à jour

---

## paas
* <del>hardware</del>
* <del>os</del>
* framework <!-- .element: class="fragment" -->

note:
* montées de version
* archi logicielles (bases, messaging, etc.)
* scalabilité

---

## faas
* <del>hardware</del> 
* <del>os</del>
* <del>framework</del>
* function <!-- .element: class="fragment" -->

note:
* code

---

## offres faas
* azure functions ⚡ <!-- .element: class="fragment" -->
* google functions <!-- .element: class="fragment" -->
* aws lambda <!-- .element: class="fragment" -->

note:
* sous-partie de serverless
* open faas

---

## functions 
⚡ event-driven <!-- .element: class="fragment" -->

note:
* microsoft
* déclencheur = événement
* ecosystème azure

---

## functions
⚡ infinite scaling <!-- .element: class="fragment" -->

note:
* vs vm pré-provisionnée
* + load balancer
* eg black-friday

---

## todo
schéma montée charge vm

---


## todo
schéma montée charge serverless

---

## functions
⚡ pay as you go <!-- .element: class="fragment" -->

note:
* 1 million d'exécutions gratuites
* 0 exécution = 0€
* exemple uat, testing

---

# use cases ?

---

## api http
microservices <!-- .element: class="fragment" -->

note:
* événement = requête http

---

## todo
exemple trigger http (java)

---

## todo
exemple trigger http (c#)

---

## api http
back-end pour une spa ? <!-- .element: class="fragment" -->

---

## todo
screenshot function proxy 💡

---

## todo
screenshot static web storage 💡

---

## api http
loterie bons de réduction <!-- .element: class="fragment" -->

note:
* != full serverless/microservices 💡
* un fonctionnalité avec bcp de trafic ?

---

# use cases ?

---

## event sourcing pt. 1

note:
* volumétrie non connues à l'avance

---

## todo
schéma event sourcing

---

## todo
exemple trigger service bus (c#)

---

## event sourcing pt. 2
réagir aux update de resources azure <!-- .element: class="fragment" -->

---

## todo
* exemple resource bdd (nodejs)
* entrée et sortie

---

# opiniated framework

note:
* déclarations simplifiées

---

## événements = triggers
* http / webhooks <!-- .element: class="fragment" -->
* bus de messages <!-- .element: class="fragment" -->
* bases de données <!-- .element: class="fragment" -->
* scheduler <!-- .element: class="fragment" -->
* etc. <!-- .element: class="fragment" -->

---

## resources = bindings
* bus de messages <!-- .element: class="fragment" -->
* bases de données <!-- .element: class="fragment" -->
* key vault <!-- .element: class="fragment" -->
* azure ad <!-- .element: class="fragment" -->

---

## langages
c# , java, javascript, python, powershell <!-- .element: class="fragment" -->

note:
* c# et javascript ++
* vs autres faas

---

## attributes ou annotations
* triggers et bindings  <!-- .element: class="fragment" -->
* c# et java <!-- .element: class="fragment" -->

note:
* sinon function.json

---

## 🌟 ioc
todo

--- 

# avantages

---

## configuration simplifiée
* function.json <!-- .element: class="fragment" -->
* variables d'environnement <!-- .element: class="fragment" -->
* 🌟 azure key vault <!-- .element: class="fragment" -->

---

## en local
* azure functions tools (cli) 💡 <!-- .element: class="fragment" -->
* vs ou vs code <!-- .element: class="fragment" -->
* maven <!-- .element: class="fragment" -->
* emulateurs (cosmos db) <!-- .element: class="fragment" -->

note:
* != autres faas
* resources azure ? service bus

---

# inconvénients 

---

## stateless

note:
* lié au framework
* contournable 🤔 (durable functions, cache partagé) 

---

## durée limitée

note:
* 10 min max par exécution

---

## cold start
* conteneurs <!-- .element: class="fragment" -->
* à la demande / éteint si inactif <!-- .element: class="fragment" -->

note:
* billing
* qq secondes, en théorie 💡
* c^ orchestrateurs
* adapter sa stratégie 🤔 (polling, 🌟 azure front door) 💡
* 🌟 functions premium

---

## manquent...
* healthcheck 💡 <!-- .element: class="fragment" -->
* swagger 💡 <!-- .element: class="fragment" -->

note:
* swagger first / swagger manuel

---

## networking
* 🌟 vnet / ase 💡 <!-- .element: class="fragment" -->
* dns <!-- .element: class="fragment" -->

---

## pricing

note:
* avoir une idée de la volumétrie

---

# ecosystème

---

## application insights 💡

---

## déploiement
* templates arm 💡 <!-- .element: class="fragment" -->
* azure devops <!-- .element: class="fragment" -->
* 🌟 job functions <!-- .element: class="fragment" -->

note:
* push depuis poste local 

---

## durable functions
* orchestration <!-- .element: class="fragment" -->
* 🌟 durable entities <!-- .element: class="fragment" -->
---

## logic apps 

---

# merci

---

# 💬
