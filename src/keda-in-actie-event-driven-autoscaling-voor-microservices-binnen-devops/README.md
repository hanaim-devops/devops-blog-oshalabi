# KEDA in Actie: Event-Driven Autoscaling voor Microservices binnen DevOps

<img src="plaatjes/keda.png" width="250" align="right" alt="keda logo om weg te halen" title="KEAD(Kubernetes Event-Driven Autoscaling)">

*[Osama Halabi, oktober 2024.](https://github.com/hanaim-devops/devops-blog-oshalabi)*
<hr/>

In de wereld van DevOps en microservices is schaalbaarheid een belangrijk thema. Met de opkomst van Kubernetes als platform voor het beheren van containergebaseerde applicaties, zijn er verschillende manieren ontstaan om applicaties automatisch te schalen. In dit blog wil ik ingaan op KEDA (Kubernetes Event-Driven Autoscaling), een tool die het mogelijk maakt om applicaties te schalen op basis van externe gebeurtenissen. Ik schrijf dit blog om inzicht te krijgen in wat KEDA is, hoe KEDA werkt, wat de voordelen en nadelen zijn, en hoe ik het kan toepassen in een microservice-omgeving. Daarnaast wil ik met deze blogpost anderen inspireren om KEDA te overwegen als een oplossing voor schaalproblemen in hun eigen DevOps-omgevingen.

## Wat is KEDA

**KEDA,** oftewel Kubernetes Event-Driven Autoscaling, is een open-source component dat dynamische autoscaling mogelijk maakt voor Kubernetes-applicaties. In tegenstelling tot de standaard autoscaling van Kubernetes, die vaak is gebaseerd op CPU- of geheugenbelasting, stelt KEDA applicaties in staat om te schalen op basis van externe gebeurtenissen. Dit betekent dat ik bijvoorbeeld eem microservice kun laten schalen op basis van het aantal berichten in een RabbitMQ-wachtrij of de lengte van een Azure Queue.

<img src="plaatjes/keda-architecture.png" width="450" align="right" alt="keda-architecture" title="KEAD Architecture">

De kracht van KEDA ligt in zijn flexibiliteit. Het ondersteunt verschillende soorten `triggers` ([Sahoo, 2023](https://devtron.ai/blog/introduction-to-kubernetes-event-driven-autoscaling-keda/#what-is-keda)), zoals message queues, databases, en HTTP-requests. Hiermee kan KEDA zorgen voor een efficiëntere inzet van resources, omdat de applicatie alleen schaalt wanneer er daadwerkelijk vraag is. Dit past goed bij de dynamische architectuur van microservices, waar de belasting sterk kan variëren.

KEDA maakt gebruik van een `ScaledObject` ([KEDA | Autoscaling Azure Pipelines Agents With KEDA, 2021](https://keda.sh/blog/2021-05-27-azure-pipelines-scaler/)), een Kubernetes-resource waarmee je kunt definiëren hoe en wanneer een applicatie moet schalen. Dit object specificeert bijvoorbeeld de naam van de deployment die moet worden geschaald, de minimale en maximale aantallen replica's, en de gebeurtenisbron waarop geschaald moet worden.

Door deze eigenschappen is KEDA geschikt voor organisaties die hun Kubernetes-omgevingen willen optimaliseren op basis van specifieke werklasten en gebeurtenissen, waardoor zij kosten kunnen besparen en efficiënter kunnen omgaan met schaarse resources.

## Hoe KEDA werkt

KEDA speelt een belangrijke rol in Kubernetes-omgevingen door het automatisch schalen van applicaties op basis van externe gebeurtenissen. Deze rollen zijn als volgt:

1. **Agent:**
   KEDA fungeert als een agent die Kubernetes Deployments kan activeren en deactiveren, waardoor het mogelijk is om te schalen naar nul wanneer er geen actieve gebeurtenissen zijn. Dit betekent dat, in plaats van voortdurend een bepaald aantal pods in stand te houden, KEDA de applicatie volledig kan uitschakelen wanneer er geen werk is en deze weer kan inschakelen zodra er nieuwe gebeurtenissen binnenkomen. Deze functionaliteit wordt beheerd door de `keda-operator` container, die draait wanneer je KEDA installeert.

2. **Metrics-collectie:**
   KEDA is in staat om een breed scala aan metrics te monitoren, variërend van eenvoudige counters tot complexe meetgegevens. Deze metrics kunnen afkomstig zijn uit diverse bronnen zoals cloud-gebaseerde services (bijv. AWS CloudWatch), monitoring-tools (bijv. Prometheus), of zelfs custom metrics die specifiek zijn voor een applicatie. Door deze metrics te verzamelen, kan KEDA nauwkeurig bepalen wanneer er behoefte is aan meer (of minder) resources.

3. **Triggers:**
   Op basis van de verzamelde metrics definieert KEDA zogenaamde triggers. Een trigger is een conditie die aangeeft wanneer er moet worden opgeschaald of afgeschaald. Bijvoorbeeld, een trigger kan worden geactiveerd wanneer het aantal berichten in een wachtrij een bepaalde drempel overschrijdt, zoals 100 berichten in een RabbitMQ-queue. Zodra de trigger afgaat, bepaalt KEDA dat het tijd is om actie te ondernemen.

4. **Acties:**
    Wanneer een trigger wordt geactiveerd, voert KEDA acties uit om de applicatie schaalbaar te maken. De meest gebruikelijke actie is het aanpassen van het aantal replica's van een Deployment of StatefulSet. KEDA kan automatisch het aantal actieve pods verhogen wanneer er meer werk is (bijv. een toename van berichten in een queue) en het aantal pods verlagen wanneer de werkdruk afneemt. Dit zorgt voor een efficiënte en flexibele schaalbaarheid van applicaties, waarbij resources alleen worden gebruikt wanneer dat nodig is.

In de volgende sectie zal ik bespreken hoe KEDA past binnen het bredere DevOps-landschap en hoe het kan bijdragen aan een betere CI/CD-pijplijn.

## Hoe past KEDA in het DevOps-landschap

KEDA (Kubernetes-based Event Driven Autoscaling) past binnen het DevOps-landschap door het focusen op het automatiseren van schaalbaarheid op een event-driven manier. Dit sluit zeker aan bij de kernprincipes van DevOps, zoals automatisering, continue integratie en continue delivery (CI/CD), en dynamisch infrastructuurbeheer.
Hier zijn de belangrijkste punten van hoe KEDA integreert met de DevOps-aanpak:

1. **Automatisering van schaalbaarheid:**
KEDA maakt gebruik van event-driven triggers om Kubernetes-applicaties automatisch op te schalen. Hierdoor kunnen DevOps-teams de schaalbaarheid afstemmen op specifieke externe gebeurtenissen zoals een toename in het aantal berichten in een wachtrij of verhoogde API-verzoeken. Dit vermindert de noodzaak van handmatige interventie, wat perfect past bij de DevOps-benadering van geautomatiseerde processen.
2. **Integratie met CI/CD-pijplijnen:**
In een CI/CD-omgeving kan KEDA ervoor zorgen dat resources dynamisch schalen tijdens piekmomenten, zoals bij het uitrollen van nieuwe softwareversies of bij het testen van nieuwe features.
3. **Ondersteuning van Infrastructure as Code (IaC):**
KEDA past binnen het principe van Infrastructure as Code, een belangrijk onderdeel van DevOps, door schaalinstellingen in configuraties vast te leggen en te beheren via tools zoals Terraform en Ansible.
Dit biedt DevOps-teams de mogelijkheid om schaalbare omgevingen consistent en reproduceerbaar op te zetten, en stelt hen in staat om infrastructuurbeheer en applicaties dynamisch aan te passen op basis van daadwerkelijke vraag.
4. **Kostenbesparing en Resource-efficiëntie:**
KEDA helpt bij het dynamisch schalen van resources op basis van de werkelijke vraag. Dit voorkomt dat er onnodig veel resources worden gebruikt, wat bijdraagt aan kostenbesparingen in een cloud-native omgeving.
5. **Gebruik in de praktijk:**
Grote bedrijven zoals Microsoft maaky gebruik van [KEDA](https://learn.microsoft.com/en-us/azure/aks/keda-about#capabilities-and-features) om hun cloud-applicaties efficiënter te beheren. Microsoft gebruikt KEDA bijvoorbeeld voor Azure Functions, waar automatisch wordt opgeschaald op basis van inkomende events.

### Wat zijn de voor- en nadelen van KEDA

#### Voordelen van KEDA

1. **Flexibiliteit in scaling**: KEDA biedt meer flexibiliteit dan de standaard Horizontal Pod Autoscaler (HPA) van Kubernetes doordat het kan schalen op basis van externe gebeurtenissen. Dit betekent dat een applicatie kan schalen op basis van de werkelijke belasting, zoals het aantal berichten in een message queue of het aantal inkomende API-verzoeken.

2. **Kostenbesparing door efficiëntere resources**: Omdat KEDA applicaties alleen schaalt wanneer er daadwerkelijk vraag is, kan het helpen om onnodige resourceverspilling te voorkomen. Dit is voordelig in cloudomgevingen waar je betaalt per gebruikte resource, zoals bij Amazon Web Services (AWS) of Microsoft Azure.

3. **Eenvoudige integratie met bestaande Kubernetes-clusters**: KEDA kan eenvoudig worden toegevoegd aan een bestaand Kubernetes-cluster zonder dat er veel aanpassingen nodig zijn. Het is compatibel met bestaande autoscaling-mechanismen en kan als aanvulling werken op HPA.

4. **Ondersteuning voor diverse triggers**: KEDA ondersteunt een breed scala aan triggers, zoals RabbitMQ, Kafka, Prometheus-metrics, en vele andere. Dit maakt het geschikt voor verschillende soorten microservice-architecturen en workflows.

#### Nadelen van KEDA

1. **Complexiteit voor kleinere projecten**: Voor kleinere projecten kan de inzet van KEDA een overkill zijn. De extra configuratie en het beheer kunnen meer tijd kosten dan wat je wint aan schaalbaarheid. Voor eenvoudige microservices kan een standaard HPA op basis van CPU of geheugen vaak al voldoende zijn.

2. **Afhankelijkheid van externe gebeurtenissen**: Omdat KEDA is gebaseerd op externe triggers, is de configuratie sterk afhankelijk van de betrouwbaarheid van deze gebeurtenisbronnen. Als de gebeurtenisbron tijdelijk niet beschikbaar is, kan dit invloed hebben op de werking van de autoscaling.

3. **Leercurve**: KEDA heeft een leercurve, vooral voor teams die nieuw zijn met Kubernetes en event-driven architecturen. Het begrijpen van hoe je `ScaledObject` en `TriggerAuthentication` correct configureert, vergt tijd en oefening, zeker als je werkt met complexe gebeurtenisbronnen.

4. **Minder fijnmazige controle in vergelijking met andere tools**: Hoewel KEDA flexibel is, bieden sommige alternatieve scaling-tools of custom controllers meer controle over specifieke scaling-scenario's. Dit kan een nadeel zijn wanneer je erg specifieke schaalbehoeften hebt die verder gaan dan de standaard mogelijkheden van KEDA.

## Deelvraag 4: Wat zijn alternatieven voor KEDA

### Wat zijn alternatieven voor KEDA

Er zijn verschillende alternatieve oplossingen voor autoscaling in Kubernetes die zich richten op andere aspecten van schaalbaarheid. Hier bespreek ik drie veelgebruikte alternatieven: HPA, Knative en custom controllers.

#### 1. **Horizontal Pod Autoscaler (HPA)**

- **Werking**: HPA is de standaard autoscaling-oplossing binnen Kubernetes. Het schaalt applicaties op basis van resourcegebruik zoals CPU- en geheugenverbruik. HPA monitort deze metrics en schaalt het aantal pod-replicas automatisch op of af, afhankelijk van de belasting.
- **Voordelen**: HPA is eenvoudig te configureren en vereist geen extra tools of configuraties buiten Kubernetes. Dit maakt het geschikt voor de meeste standaard workloads.
- **Nadelen**: HPA mist de flexibiliteit van KEDA als het gaat om schalen op basis van externe gebeurtenissen. HPA kan niet direct schalen op basis van metrics zoals de lengte van een wachtrij of inkomende API-verzoeken zonder extra configuraties.
- **Gebruiksscenario's**: HPA is een goede keuze voor applicaties met voorspelbare belasting en gebruikspatronen waarbij CPU- en geheugenverbruik een goede indicatie zijn voor de belasting van de applicatie.

#### 2. **Knative**

- **Werking**: Knative is een Kubernetes-gebaseerde platformlaag die is ontworpen om serverless toepassingen te draaien. Knative Serving maakt het mogelijk om applicaties automatisch te schalen, zelfs tot nul, wanneer er geen verkeer is. Dit maakt het een goede keuze voor serverless architecturen.
- **Voordelen**: Knative biedt naadloze schaalbaarheid en kan beter omgaan met pieken in korte tijdsbestekken door direct te reageren op HTTP-verkeer. Het is bijzonder geschikt voor serverless workloads.
- **Nadelen**: Knative kan complexer zijn om op te zetten dan KEDA, omdat het afhankelijk is van verschillende componenten zoals Knative Serving en Eventing. Het kan ook overbodig zijn voor applicaties die niet per se serverless hoeven te zijn.
- **Gebruiksscenario's**: Knative is ideaal voor serverless architecturen waarbij applicaties automatisch moeten kunnen opschalen en terugschalen naar nul wanneer er geen vraag is. Het is minder geschikt als je applicaties niet de serverless benadering nodig hebben.

#### 3. **Custom Controllers**

- **Werking**: Met custom controllers kunnen ontwikkelaars zelf logica schrijven voor het schalen van applicaties in Kubernetes. Dit biedt volledige controle over wanneer en hoe een applicatie schaalt, gebaseerd op custom metrics of gebeurtenissen.
- **Voordelen**: Custom controllers bieden maximale flexibiliteit en kunnen volledig worden aangepast aan de behoeften van een specifieke toepassing.
- **Nadelen**: Het ontwikkelen en onderhouden van custom controllers kan tijdrovend zijn en vereist diepgaande kennis van Kubernetes API’s en controllers. Dit maakt het minder geschikt voor teams zonder uitgebreide Kubernetes-ervaring.
- **Gebruiksscenario's**: Custom controllers zijn een goede keuze voor organisaties die zeer specifieke schaalbehoeften hebben die niet door standaardtools zoals KEDA of HPA worden ondersteund.

### Vergelijking van KEDA met Alternatieven

KEDA onderscheidt zich door de mogelijkheid om te schalen op basis van externe gebeurtenissen, wat het een goede keuze maakt voor event-driven microservices. HPA is meer geschikt voor standaard scenarios waarbij CPU- of geheugengebruik een betrouwbare indicator is voor de belasting. Knative biedt meer voordelen voor serverless architecturen, maar brengt ook meer complexiteit met zich mee. Custom controllers bieden de meeste flexibiliteit, maar vereisen een hogere ontwikkelinspanning en expertise.

## Hoe pas ik KEDA toe in het klein

### Stap 1: Voorbereidingen

Voor deze test heb ik een lokale Kubernetes-omgeving opgezet met Minikube. Minikube is handig omdat het een lokaal Kubernetes-cluster simuleert, ideaal voor testdoeleinden. Daarnaast heb ik Helm geïnstalleerd, een package manager voor Kubernetes, om KEDA eenvoudig te kunnen installeren.

### Stap 2: Installeren van KEDA

Met Helm heb ik KEDA in het Kubernetes-cluster geïnstalleerd:

```bash
helm repo add kedacore https://kedacore.github.io/charts
helm repo update
helm install keda kedacore/keda
```

Dit commando installeert KEDA in het cluster. Na de installatie heb ik gecontroleerd of de KEDA-pods draaiden met:

```bash
kubectl get pods -n keda
```

### Stap 3: Configureren van een Microservice en een ScaledObject

Ik heb een eenvoudige Node.js-microservice gemaakt die berichten verwerkt uit een RabbitMQ-wachtrij. Vervolgens heb ik een Kubernetes Deployment gemaakt om de microservice in het cluster te draaien:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: message-processor
spec:
  replicas: 1
  selector:
    matchLabels:
      app: message-processor
  template:
    metadata:
      labels:
        app: message-processor
    spec:
      containers:
      - name: message-processor
        image: <je-microservice-image>
        ports:
        - containerPort: 8080

```

De image verwijst naar een Docker-image van mijn microservice, gehost op Docker Hub.

Vervolgens heb ik een ScaledObject gedefinieerd om de scaling-logica te beheren. Dit object vertelt KEDA hoe en wanneer de microservice automatisch moet schalen op basis van de belasting van de RabbitMQ-wachtrij:

```yaml
apiVersion: keda.sh/v1alpha1
kind: ScaledObject
metadata:
  name: rabbitmq-scaledobject
  namespace: default
spec:
  scaleTargetRef:
    kind: Deployment
    name: message-processor
  pollingInterval: 30  # Interval waarmee KEDA de wachtrij controleert
  cooldownPeriod: 300  # Tijd die KEDA wacht voordat het weer naar beneden schaalt
  minReplicaCount: 1   # Minimale aantal replicas
  maxReplicaCount: 10  # Maximale aantal replicas
  triggers:
  - type: rabbitmq
    metadata:
      queueName: my-queue
      host: amqp://<rabbitmq-host-url>
      queueLength: "5"  # Schaal als er meer dan 5 berichten in de wachtrij staan
```

Dit ScaledObject zorgt ervoor dat de message-processor-service opschaalt als er meer dan 5 berichten in de wachtrij staan, en terugschakelt als de belasting afneemt.

### Stap 4: Testen van de Scaling

Om te testen of KEDA correct werkt, heb ik berichten in de RabbitMQ-wachtrij geplaatst en geobserveerd hoe KEDA de microservice automatisch opschaalde. Met het commando kubectl get deployments kon ik het aantal actieve replicas monitoren.

Tijdens de test schaalde de microservice omhoog naarmate de berichten in de wachtrij toenamen, en terug naar beneden zodra de wachtrij weer leeg was. Dit toonde aan dat KEDA effectief de belasting van de microservice kon beheren op basis van de gebeurtenissen.

## Bronnen

- Sahoo, J. (2023, 23 augustus). Introduction to Kubernetes Event-Driven Autoscaling (KEDA). https://devtron.ai/blog/introduction-to-kubernetes-event-driven-autoscaling-keda/#what-is-keda
- KEDA | Autoscaling Azure Pipelines agents with KEDA. (2021, 27 mei). KEDA. https://keda.sh/blog/2021-05-27-azure-pipelines-scaler/
- Gupta, A. (2023, 29 december). Scaling Kubernetes: Intro to Kubernetes-based event-driven autoscaling (KEDA). Microsoft Open Source Blog. https://opensource.microsoft.com/blog/2020/05/12/scaling-kubernetes-keda-intro-kubernetes-based-event-driven-autoscaling/
- KEDA. (z.d.). KEDA. https://keda.sh/
- Tomkerkhove. (2024, 6 augustus). Kubernetes Event-driven Autoscaling (KEDA) - Azure Kubernetes Service. Microsoft Learn. https://learn.microsoft.com/en-us/azure/aks/keda-about#capabilities-and-features
- Singh, G. (2024, 8 oktober). KEDA (Kubernetes Event-driven Autoscaling). XenonStack. https://www.xenonstack.com/blog/kubernetes-event-driven-autoscaling