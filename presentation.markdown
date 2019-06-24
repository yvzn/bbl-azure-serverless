<!-- .slide: data-background="var(--microsoft-blue)" -->

# Azure, serverless

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

## faas ?
* Azure functions ⚡ <!-- .element: class="fragment" -->
* Google functions <!-- .element: class="fragment" -->
* AWS lambda <!-- .element: class="fragment" -->

note:
* sous-partie de serverless
* open faas

---

## ⚡ functions 
* event-driven <!-- .element: class="fragment" -->

note:
* microsoft
* déclencheur = événement
* ecosystème Azure

---

## ⚡ functions
* event-driven
* infinite scaling

note:
* vs vm pré-provisionnée
* load balancer
* eg black-friday

---

## todo
schéma montée charge vm

---


## todo
schéma montée charge serverless

---

## ⚡ functions
* event-driven
* infinite scaling 
* pay as you go <!-- .element: class="fragment" -->

note:
* 1 million d'exécutions gratuites
* 0 exécution = 0€
* exemple uat, testing

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# use cases

---

## api http
* microservices <!-- .element: class="fragment" -->
* web hooks <!-- .element: class="fragment" -->

note:
* événement = requête http

---

```typescript <!-- .element: class="fragment" -->
@FunctionName("hello")
public HttpResponseMessage run(
    @HttpTrigger(name = "req", methods = HttpMethod.GET }, authLevel = AuthorizationLevel.ANONYMOUS)
    HttpRequestMessage<Optional<String>> request,
    final ExecutionContext context) {

    context.getLogger().info("Java HTTP trigger processed a request.");

    String name = request.getQueryParameters().get("name");

    return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
}
```

---

```typescript
@FunctionName("hello")
public HttpResponseMessage run(
    @HttpTrigger(name = "req", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.ANONYMOUS,
        route = "trigger/{id}/{name=EMPTY}")
    HttpRequestMessage<Optional<String>> request,
    @BindingName("id") String id,
    @BindingName("name") String name,
    final ExecutionContext context) {

    // ...

}
```

---

```csharp
[FunctionName("hello")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)] 
    HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    return new OkObjectResult($"Hello, {name}");;
}
```

---

## todo
screenshot mvn run

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

## tip
* utilisez les bindings 💡 <!-- .element: class="fragment" -->
* utilisez les triggers 💡 <!-- .element: class="fragment" -->

---

## api http
back-end pour une spa ? <!-- .element: class="fragment" -->

note:
* API créé rapidement

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

## tip
utilisez function proxy 💡 <!-- .element: class="fragment" -->

---

## todo
screenshot function proxy 💡

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

## tip
utilisez static web storage 💡 <!-- .element: class="fragment" -->

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

<!-- .slide: data-background="var(--microsoft-blue)" -->

# use cases

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
réagir aux update de resources Azure <!-- .element: class="fragment" -->

---

## todo
* exemple resource bdd (nodejs)
* entrée et sortie

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->


## tip
* utilisez les bindings 💡
* utilisez les triggers 💡

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

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
* Azure AD <!-- .element: class="fragment" -->

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

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

## tip
ddd 💡 <!-- .element: class="fragment" -->

note:
* séparer logique métier

--- 

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

# avantages

---

## configuration simplifiée
* function.json <!-- .element: class="fragment" -->
* variables d'environnement <!-- .element: class="fragment" -->
* 🌟 Azure key vault <!-- .element: class="fragment" -->

---

## en local
* Azure functions tools (cli) 💡 <!-- .element: class="fragment" -->
* VS / VS code <!-- .element: class="fragment" -->
* maven <!-- .element: class="fragment" -->
* emulateurs <!-- .element: class="fragment" -->

note:
* != autres faas
* Cosmos DB
* resources Azure ? service bus ?

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

# inconvénients 

---

## stateless

note:
* lié au framework
* contournable 🤔 (durable functions, cache partagé) 
* exemple id vs pays

---

## durée limitée

note:
* 10 min max par exécution

---

## cold start

note:
* conteneurs, c^ orchestrateur
* à la demande / éteint si inactif
* billing
* qq secondes, en théorie 💡

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

## tip 
adapter sa stratégie 💡 <!-- .element: class="fragment" -->

note:
* polling 
* Azure front door
* 🌟 functions premium


---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

## tip 
utiliser l'effet cache 💡 <!-- .element: class="fragment" -->

---

```csharp
public class MyFunction
{
    private static ServiceWithCostlyStartup service = ...

} 
```

---

## manquent...
* healthcheck 💡 <!-- .element: class="fragment" -->
* swagger 💡 <!-- .element: class="fragment" -->

note:
* swagger first / swagger manuel

---

## networking
* 🌟 VNET / ASE 💡 <!-- .element: class="fragment" -->
* DNS <!-- .element: class="fragment" -->

---

## pricing

note:
* avoir une idée de la volumétrie

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# ecosystème

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

## tip
utilisez application insights 💡 <!-- .element: class="fragment" -->

---

<!-- .slide:  data-background="var(--microsoft-green)" class="tip" -->

## tip
* templates arm 💡 <!-- .element: class="fragment" -->
* Azure devops <!-- .element: class="fragment" -->
* 🌟 job functions <!-- .element: class="fragment" -->

note:
* deploy depuis poste local 

---

## durable functions
* orchestration <!-- .element: class="fragment" -->
* 🌟 durable entities <!-- .element: class="fragment" -->
---

## logic apps 

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# merci

---

<!-- .slide: data-background="var(--microsoft-blue)" -->

# 💬
