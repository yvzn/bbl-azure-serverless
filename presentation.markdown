<!-- .slide: data-background="var(--microsoft-blue)" -->

# Azure serverless ...

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

## ... et dans la vraie vie ?

---

## serverless

note:
* nom mal choisi

---

## less servers
<del>serverless</del>

note:
* ↘ config, gestion
* managé
* focus sur le code

---

## servers  ?
* hardware <!-- .element: class="fragment" -->
* os <!-- .element: class="fragment" -->
* framework <!-- .element: class="fragment" -->
* code <!-- .element: class="fragment" -->

note:
* de quoi on parle ?
* déployer mon app
* je gère, je m'en occupe

---

## on premise
* hardware 👨‍💻 
* os 👩‍💻 <!-- .element: class=" faded" -->
* framework 👩‍💻 <!-- .element: class=" faded" -->
* code 👩‍💻 <!-- .element: class=" faded" -->

note:
* billing one shot
* pannes matérielles
* réseau (firewall, fai)

---

## iaas
* hardware ⛅
* os 👨‍💻
* framework 👨‍💻 <!-- .element: class=" faded" -->
* code 👨‍💻 <!-- .element: class=" faded" -->

note:
* scalabilité
* pannes logicielles

---

## paas
* hardware ⛅
* os ⛅
* framework 👩‍💻 
* code 👩‍💻 <!-- .element: class=" faded" -->

note:
* montées de version
* archi logicielle (bases, messaging, etc.)

---

## faas
* hardware ⛅ 
* os ⛅
* framework ⛅
* code 👩‍💻 

---

## plateformes faas
* Azure functions ⚡ <!-- .element: class="fragment" -->
* Google functions <!-- .element: class="fragment" -->
* AWS lambda <!-- .element: class="fragment" -->

note:
* sous-partie de serverless
* open faas

---

## ⚡ functions 

note:
* Microsoft
* ecosystème Azure
* déclencheur = événement

---

## ⚡ functions 
* event-driven 

note:
* déclencheur = événement

---

## ⚡ functions
* event-driven
* infinite scaling

note:
* vs vm pré-provisionnée
* load balancer auto
* eg black-friday

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


---

![montée en charge](resource/load_vs_capacity009.png)


---

![montée en charge](resource/load_vs_capacity011.png)

note:
* automatique
* managé

---

## ⚡ functions
* event-driven
* infinite scaling 
* pay as you go <!-- .element: class="fragment" -->

note:
* 0 exécution = 0€
* exemple uat, testing
* free tier 1 million
* reste stockage + bande passante

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

https:// .... .azurewebsites.net/api/<mark>hello</mark>

note:
* métaprogrammation

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
* annotations sur return

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

---

```typescript 
...
public HttpResponseMessage run(
    ...
    final ExecutionContext context) {

    context.getLogger().info("Java HTTP trigger processed a request.");

    ...
}
```

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
* généré: c#, java
* à écrire: nodejs, python

---

```typescript
...
@FunctionName("hello")
public HttpResponseMessage run(
    @HttpTrigger(name = "req", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.ANONYMOUS,
        route = "trigger/{id}/{name=EMPTY}")
    HttpRequestMessage<Optional<String>> request,
    @BindingName("id") String id,
    @BindingName("name") String name,
    ...) {

    ...

}
```
https:// .... .azurewebsites.net/api/trigger/<mark>1234</mark>/<mark>test</mark>

note:
* route parameters

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
* API plus simple

---

```bash
mvn archetype:generate 
	"-DarchetypeGroupId=com.microsoft.azure" 
	"-DarchetypeArtifactId=azure-functions-archetype"
```

---

```bash
mvn clean package 
mvn azure-functions:run
```

---

![video mvn azure-functions:run](resource/mvn_azure_functions_run.png)

note:
* spring cloud functions

---

## function app

* https://<mark> .... </mark>.azurewebsites.net/api/hello 

note:
* groupement logique
* mises à jour simultanées
* paramétrage commun

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* bindings
* triggers <!-- .element: class="fragment" -->

note:
* automatisation vs code manuel 

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# use case 2

---

## back-end + spa

note:
* API créé rapidement

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* function proxy

---

![définition function proxy](https://docs.microsoft.com/fr-fr/azure/azure-functions/media/functions-create-vnet/create-proxy.png)

note:
* API Management
* Azure front door 🌟

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* static web on blob storage


---

![static website on azure blob storage](https://docs.microsoft.com/en-us/azure/storage/blobs/media/storage-blob-static-website-host/enable-static-website-hosting.png)

---

![architecture web app serverless](https://docs.microsoft.com/fr-fr/azure/architecture/reference-architectures/serverless/_images/serverless-web-app.png)

note:
* architectures référence azure

---

## full serverless ?

---

![architecture complexe](resource/complex.svg)

note:
* déplacement complexité

---

![architecture Netflix](https://pbs.twimg.com/media/DUyIfFtX0AAoZGz.jpg)

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* migrer partiellement
* migrer progressivement <!-- .element: class="fragment" -->


note:
* micro-fonctionnalité avec peut-être bcp de trafic
* loterie bons de réduction

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# use case 3

---

## transformations de données
* http ↦ queue <!-- .element: class="fragment" -->
* queue ↦ http <!-- .element: class="fragment" -->
* queue ↦ queue <!-- .element: class="fragment" -->

note:
* interfaces entre systèmes
* extensibilité
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
* ack vs. exceptions
* retries
* poison queue
* string

---

```csharp
...
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
    Message myQueueMessage,
    ...
```

note:
* objet et metadonnées
* connection string (vs. hard coded)

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

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# use case 4

---

## event sourcing

---

![schema event sourcing](resource/event_sourcing.svg)

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
* méthodes IQueryable
* CloudTable

---

## event sourcing
* CosmosDB <!-- .element: class="fragment" -->
* change feed processor <!-- .element: class="fragment" -->

note:
* c^ trigger

---

![schema event sourcing](resource/event_sourcing.svg)

note:
* hot/cold

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
* cadre dirigiste
* simplifie le dév

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
* c# et javascript ++
* vs autres faas

---

## attributes ou annotations
* triggers et bindings  <!-- .element: class="fragment" -->
* c# et java <!-- .element: class="fragment" -->

note:
* sinon function.json

---

## IoC + DI 🌟

note:
* csharp
* spring

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
* Startup .Net core

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* DDD 

note:
* séparer logique métier
* functions = framework

---

<!-- .slide:  data-background="var(--microsoft-blue)" -->

# ⚡ = 😎

note:
* avantages

---

## configuration simplifiée
* variables d'environnement <!-- .element: class="fragment" -->
* Azure key vault 🌟 <!-- .element: class="fragment" -->
* local.settings.json <!-- .element: class="fragment" -->

---

![ecran app settings](https://docs.microsoft.com/fr-fr/azure/azure-functions/media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

---


![ecran platform features](https://docs.microsoft.com/fr-fr/azure/azure-functions/media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

---

## en local
* Azure functions tools (cli) <!-- .element: class="fragment" -->
* debugging <!-- .element: class="fragment" -->
* intégration maven / dotnet <!-- .element: class="fragment" -->
* emulateurs <!-- .element: class="fragment" -->

note:
* != autres faas 💡 (docker)
* Cosmos DB

---

<!-- .slide:  data-background="var(--microsoft-red)" -->

# ⚡ = 🤨 

note:
* inconvénients

---

## stateless

note:
* lié au framework
* contournements 🤔 (cache partagé) 
* exemple plages id vs pays

---

## durée limitée

note:
* max par exécution
* configurable

---

## cold start 😭

note:
* conteneurs, c^ orchestrateur
* à la demande / éteint si inactif
* billing
* qq secondes, en théorie 💡

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

---

## pricing

note:
* avoir une idée de la volumétrie
* vs. prix fixe mensuel instance vm

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# ecosystème

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* Application Insights 

---

![Application map in app insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/media/opencensus-python/application-map.png)

note:
* application map

---

![Transaction diagnostics in app insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/media/transaction-diagnostics/searchresults.png)

note:
* transaction diagnostics
* cross micro-services

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

* Azure DevOps 
* job functions 🌟 <!-- .element: class="fragment" -->
* templates ARM <!-- .element: class="fragment" -->

note:
* vs deploy depuis poste local 
* yaml / json

---

![Nouveaux jobs Azure DevOps functions](https://docs.microsoft.com/en-us/azure/azure-functions/media/functions-how-to-azure-devops/build-templates.png)

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
* focus sur le code
* prise en main facilitée
* évolue rapidement
* contraintes

note:
* qualité du code
* aller sur cloud pour pas cher
* pas tous les uses cases


---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# merci

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# 💬
