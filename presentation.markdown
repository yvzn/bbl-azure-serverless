<!-- .slide: data-background="var(--microsoft-blue)" -->

# Azure serverless ...

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

## ... dans la vraie vie ?

note:
* Retour d'expérience sur le développement et le déploiement d'une application serverless en production

---

## serverless

note:
* le terme est mal choisi

---

## less servers
<del>serverless</del>

note:
* moins de servers = moins de config, moins de gestion
* managé pour moi
* focus sur le code

---

## servers  ?
* hardware <!-- .element: class="fragment" -->
* os <!-- .element: class="fragment" -->
* framework <!-- .element: class="fragment" -->
* code <!-- .element: class="fragment" -->

note:
* qu'entend-on par serveurs ?
* ce dont j'ai besoin pour rendre mon app disponible
* qu'est-ce que je gère, de quoi suis-je responsable

---

## on premise
* hardware 👨‍💻 
* os 👩‍💻 <!-- .element: class="faded" -->
* framework 👩‍💻 <!-- .element: class="faded" -->
* code 👩‍💻 <!-- .element: class="faded" -->

note:
* large choix du matériel
* billing one shot
* je dois gérer les pannes matérielles
* les pannes réseau (firewall...)
* et les pannes de mon FAI !

---

## iaas
* hardware ⛅
* os 👨‍💻
* framework 👨‍💻 <!-- .element: class="faded" -->
* code 👨‍💻 <!-- .element: class="faded" -->

note:
* je dois gérer les pannes logicielles
* et la scalabilité

---

## paas
* hardware ⛅
* os ⛅
* framework 👩‍💻 
* code 👩‍💻 <!-- .element: class="faded" -->

note:
* je dois gérer les montées de version
* et l'archi logicielle (bases, messaging, etc.)

---

## faas
* hardware ⛅ 
* os ⛅
* framework ⛅
* code 👩‍💻 

note:
* l'infra est managée pour moi
* juste "exécutez-moi ce code"

---

## plateformes faas
* Azure functions ⚡ <!-- .element: class="fragment" -->
* Google functions <!-- .element: class="fragment" -->
* AWS lambda <!-- .element: class="fragment" -->

note:
* faas = sous-partie de serverless
* open faas => je dois gérer mon infra (donc rarement serverless)

---

## ⚡ functions 

note:
* Implémentation Microsoft
* ecosystème Azure

---

## ⚡ functions 
* event-driven 

note:
* déclencheur = événement

---

## ⚡ functions
* event-driven <!-- .element: class="faded" -->
* infinite scaling

note:
* vs VM pré-provisionnée
* load balancer automatique
* exemple: black-friday

---

![montée en charge](resource/load_vs_capacity001.png)


---

![montée en charge](resource/load_vs_capacity003.png)


---

![montée en charge](resource/load_vs_capacity005.png)

note:
* "latence"

---

![montée en charge](resource/load_vs_capacity007.png)

note:
* escalade ?

---

![montée en charge](resource/load_vs_capacity009.png)

note:
* je continue à payer "pour rien"

---

![montée en charge](resource/load_vs_capacity011.png)

note:
* scaling automatique, managé pour moi
* pas de limites (ou limites très élevées)

---

## ⚡ functions
* event-driven <!-- .element: class="faded" -->
* infinite scaling <!-- .element: class="faded" -->
* pay as you go <!-- .element: class="fragment" -->

note:
* je ne paie que ce qui tourne
* free tier pour le 1er million d'appels
* 0 exécution = 0€
* intéressant en uat ou en testing
* reste à payer stockage + bande passante

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# use case 1

---

## api http
* microservices <!-- .element: class="fragment" -->
* web hooks <!-- .element: class="fragment" -->

note:
* événement = requête http

---

```typescript 
public class Function {

    public ... run(...)
    {
        ...
    }
}
```

---

```typescript 
...
@FunctionName("hello")
public ... run(...) {
    ...
}
```

* https:// .... .azurewebsites.net/api/<mark>hello</mark>

note:
* route par défaut
* métaprogrammation (annotations ou attributs)

---

```typescript 
...
public HttpResponseMessage run(
    ...
    HttpRequestMessage<Optional<String>> request
    ...) {

    ...
}
```

note:
* typage

---

```typescript 
...
public ... run(
    @HttpTrigger(name = "req", methods = HttpMethod.GET }, authLevel = AuthorizationLevel.ANONYMOUS)
    HttpRequestMessage<Optional<String>> request,
    ...) {

    ...
}
```

note:
* annotations
* méthodes GET / POST
* authLevel pour gérer l'authentification
* annotations sur return également possible

---

```typescript 
...
public HttpResponseMessage run(
    ...
    HttpRequestMessage<Optional<String>> request,
    ...) {

    ...

    String name = request.getQueryParameters().get("name");

    return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
}
```

note:
* manipulation des paramètres

---

```typescript 
...
public ... run(
    ...
    final ExecutionContext context) {

    context.getLogger().info("Java HTTP trigger processed a request.");

    ...
}
```

note:
* accès aux métadonnées via le contexte

---

```typescript 
public class Function {
    @FunctionName("hello")
    public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = HttpMethod.GET }, authLevel = AuthorizationLevel.ANONYMOUS)
        HttpRequestMessage<Optional<String>> request,
        final ExecutionContext context) {

        context.getLogger().info("Java HTTP trigger processed a request.");

        String name = request.getQueryParameters().get("name");

        return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
    }
}
```

---

## function.json

```json
{
  ...
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "get" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
  ...
}
```

note:
* requis par le framework
* généré à la compilation pour c#, java
* à écrire manuellement pour nodejs, python

---

```typescript
...
public ... run(
    @HttpTrigger(...
        route = "trigger/{id}/{name=EMPTY}")
    ... request,
    @BindingName("id") String id,
    @BindingName("name") String name,
    ...) {

    ...

}
```
https:// .... .azurewebsites.net/api/trigger/<mark>1234</mark>/<mark>test</mark> 

note:
* named route parameters

---

```csharp
public class Function {
    [FunctionName("hello")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)] 
        HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        string name = req.Query["name"];

        return new OkObjectResult($"Hello, {name}");;
    }
}
```

note:
* API c# plus simple

---

```bash
mvn archetype:generate 
	"-DarchetypeGroupId=com.microsoft.azure" 
	"-DarchetypeArtifactId=azure-functions-archetype"
```

note:
* archétype et plugins maven

---

```bash
mvn clean package 
mvn azure-functions:run
```

---

![video mvn azure-functions:run](resource/mvn_azure_functions_run.png)

---

## function app

* https://<mark> .... </mark>.azurewebsites.net/api/hello 

note:
* permet un groupement logique de plusieurs fonctions
* mises à jour simultanées
* paramétrage commun

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* bindings
* triggers <!-- .element: class="fragment" -->

note:
* permet automatisation (vs code manuel) 

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# use case 2

---

## back-end + spa

note:
* API créée rapidement

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* function proxy

---

![définition function proxy](https://docs.microsoft.com/fr-fr/azure/azure-functions/media/functions-create-vnet/create-proxy.png)

<small>source: docs.microsoft.com</small>

note:
* gratuit
* autres options (payantes):
* API Management
* Azure front door 🌟

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* static web on blob storage

note:
* hébergement statique

---

![static website on azure blob storage](https://docs.microsoft.com/en-us/azure/storage/blobs/media/storage-blob-static-website-host/enable-static-website-hosting.png)

<small>source: docs.microsoft.com</small>

---

![architecture web app serverless](https://docs.microsoft.com/fr-fr/azure/architecture/reference-architectures/serverless/_images/serverless-web-app.png)

<small>source: docs.microsoft.com</small>

note:
* exemple d'architecture SPA serverless
* architectures référence azure

---

## full serverless ?

---

![architecture complexe](resource/complex.svg)

note:
* déplacement de la complexité (du code vers l'archi des micro-services)

---

![architecture Netflix](https://pbs.twimg.com/media/DUyIfFtX0AAoZGz.jpg)

<small>source: Netflix</small>

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* migrer partiellement
* migrer progressivement <!-- .element: class="fragment" -->


note:
* micro-fonctionnalité avec peut-être bcp de trafic
* exemple loterie bons de réduction

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# use case 3

---

## transformations de données
* http ↦ queue <!-- .element: class="fragment" -->
* queue ↦ http <!-- .element: class="fragment" -->
* queue ↦ queue <!-- .element: class="fragment" -->

note:
* interfaces entre systèmes (extensibilité)
* enrichissement
* volumétrie non connues à l'avance

---


## lire depuis une queue

---

```csharp
...
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
    string myQueueItem,
    ...
```

note:
* gestion de l'acquittement automatique
* si exception = retry
* et poison queue
* queue item = string

---

```csharp
...
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
    Message myQueueMessage,
    ...
```

note:
* queue item = objet (string) + metadonnées
* connection string pour se connecter au bus (variable env, non hard coded)

---

```csharp
...
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
    string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string correlationId,
    ...
```

---


## écrire dans une queue

---

```csharp
...
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run(...)
{
    ...
    return "Hello";
}
```

note:
* annotation sur le return !

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# use case 4

---

## event sourcing

---

![schema event sourcing](resource/event_sourcing.svg)

note:
* différentes bases de données

---

## écrire dans une base

---

```csharp
...
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyObject Run(...) {
    ...
    return new MyObject { Name = "Test", Id = Guid.NewGuid(), };
}
```

note:
* Azure table storage

---

## lire depuis une base

---

```csharp
...
public static void Run(
    [Table("MyTable", "MyPartition")] IQueryable<MyObject> pocos,
    ...) {

}
```

note:
* utiliser les méthodes de IQueryable
* objet CloudTable

---

## event sourcing
* CosmosDB <!-- .element: class="fragment" -->
* change feed processor <!-- .element: class="fragment" -->

note:
* CFP = équivalent d'un trigger SQL

---

![schema event sourcing](resource/event_sourcing.svg)

note:
* hot/cold (rapide et cher vs lent et peu cher)
* datalake

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* bindings
* triggers

note:
* vs clients CosmosDB, PostgreSQL, etc.

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# opiniated framework

note:
* framework dirigiste
* simplifie le développement

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
* stockage <!-- .element: class="fragment" -->
* SignalR <!-- .element: class="fragment" -->
* etc. <!-- .element: class="fragment" -->

note:
* input binding / output

---

## langages
* c# <!-- .element: class="fragment" --> 
* java <!-- .element: class="fragment" -->
* nodejs <!-- .element: class="fragment" -->
* python <!-- .element: class="fragment" -->
* powershell <!-- .element: class="fragment" -->

note:
* c# et javascript sont les plus utilisés
* les autres faas proposent d'autres options

---

## attributes ou annotations
* triggers et bindings  <!-- .element: class="fragment" -->
* c# et java <!-- .element: class="fragment" -->

note:
* sinon function.json

---

## IoC + DI 🌟

note:
* possible depuis peu en csharp
* sinon via spring cloud functions

---

```csharp
[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]
...
public class Startup : FunctionsStartup
{
    public override void Configure(IFunctionsHostBuilder builder)
    {
        builder.Services.AddHttpClient();
        builder.Services.AddSingleton(...);
        ...
```

note:
* ressemble au Startup .Net core

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* DDD 

note:
* utiliser IoC pour séparer la logique métier
* functions = framework
* pouvoir changer de solution

---

<!-- .slide:  data-background="var(--microsoft-blue)" -->

# ⚡ = 😎

note:
* cool features

---

## configuration simplifiée
* variables d'environnement <!-- .element: class="fragment" -->
* Azure key vault 🌟 <!-- .element: class="fragment" -->
* local.settings.json <!-- .element: class="fragment" -->

note:
* vs credentials hard coded
* technologie de conteneurs

---

![ecran app settings](https://docs.microsoft.com/fr-fr/azure/azure-functions/media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

<small>source: docs.microsoft.com</small>

---


![ecran platform features](https://docs.microsoft.com/fr-fr/azure/azure-functions/media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

<small>source: docs.microsoft.com</small>

note:
* cors
* authentification / authorization

---

## dev en local
* Azure functions tools (cli) <!-- .element: class="fragment" -->
* debugging <!-- .element: class="fragment" -->
* intégration maven / dotnet <!-- .element: class="fragment" -->
* emulateurs <!-- .element: class="fragment" -->

note:
* expérience++ vs autres faas 💡 (docker...)
* émulateur Cosmos DB

---

<!-- .slide:  data-background="var(--microsoft-red)" -->

# ⚡ = 🤨 

note:
* inconvénients

---

## stateless

note:
* lié au framework
* contournements possibles 🤔 (cache partagé, système de fichiers) 
* notification multi-instances impossible

---

## durée limitée

note:
* max par exécution
* configurable

---

## cold start 😤

note:
* conteneurs / orchestrateur
* infinite scaling implique éteint si inactif
* (pratique pour le billing, embêtant pour le cold start)
* qq secondes, en théorie 💡 (+JVM +Spring)

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* adapter sa stratégie 

note:
* polling 
* Azure front door 🌟
* functions premium 🌟

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* effet mémoire 

note:
* durée limitée ☹
* mais en mémoire pendant cette durée
* opportunité pour garder des choses en mémoire

---

```csharp
public class MyFunction
{
    private static ClassWithCostlyInit service = ...

} 
```

---

<!-- .slide:  data-background="var(--microsoft-orange)" class="tip" -->

## manquent...
* healthcheck <!-- .element: class="fragment" -->
* swagger <!-- .element: class="fragment" -->

note:
* vs. actuator ou .Net core
* swagger first / swagger manuel

---

## networking
* vnet / ase 🌟 <!-- .element: class="fragment" -->
* dns <!-- .element: class="fragment" -->

note:
* public sur internet par défaut (peut ne pas convenir à tout le monde)

---

## pricing

note:
* avoir une idée de la volumétrie pour prévoir le prix
* pas de prix fixe mensuel != instance VM par ex.
* stockage / réseau

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# ecosystème

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* Application Insights 

---

![Application map in app insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/media/opencensus-python/application-map.png)

<small>source: docs.microsoft.com</small>

note:
* application map

---

![Transaction diagnostics in app insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/media/transaction-diagnostics/searchresults.png)

<small>source: docs.microsoft.com</small>

note:
* transaction diagnostics
* cross micro-services

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* Azure DevOps 
* job functions 🌟 <!-- .element: class="fragment" -->
* templates ARM <!-- .element: class="fragment" -->

note:
* ne pas deploy depuis son poste local 
* yaml / json

---

![Nouveaux jobs Azure DevOps functions](https://docs.microsoft.com/en-us/azure/azure-functions/media/functions-how-to-azure-devops/build-templates.png)

<small>source: docs.microsoft.com</small>

---

## durable functions
* orchestration <!-- .element: class="fragment" -->
* durable entities 🌟 <!-- .element: class="fragment" -->

note:
* stateful ?

---

## logic apps 

---

## Azure 

note:
* Active Directory
* CosmosDB
* APIM / Azure Front Door

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

## Conclusion
* focus sur le code <!-- .element: class="fragment" -->
* prise en main facilitée <!-- .element: class="fragment" -->
* contraintes <!-- .element: class="fragment" -->

note:
* qualité du code
* managé / cloud accessible
* pas tous les uses cases
* évolue rapidement
* open source 

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# merci

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# 💬
