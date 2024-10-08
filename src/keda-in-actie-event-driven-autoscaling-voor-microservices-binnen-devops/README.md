# KEDA in Actie: Event-Driven Autoscaling voor Microservices binnen DevOps

<img src="plaatjes/keda.png" width="250" align="right" alt="mdbook logo om weg te halen" title="maar vergeet de alt tekst niet">

*[Osama Halabi, oktober 2024.](https://github.com/hanaim-devops/devops-blog-oshalabi)*
<hr/>

In de wereld van DevOps en microservices is schaalbaarheid een belangrijk thema. Met de opkomst van Kubernetes als platform voor het beheren van containergebaseerde applicaties, zijn er verschillende manieren ontstaan om applicaties automatisch te schalen. In dit blog wil ik ingaan op KEDA (Kubernetes Event-Driven Autoscaling), een tool die het mogelijk maakt om applicaties te schalen op basis van externe gebeurtenissen. Ik schrijf dit blog om inzicht te krijgen in hoe KEDA werkt, wat de voordelen en nadelen zijn, en hoe ik het kan toepassen in een microservice-omgeving. Het doel is om mijn bevindingen te delen met iedereen die geïnteresseerd is in het optimaliseren van schaalbaarheid binnen een DevOps-omgeving.

In dit blog zal ik de volgende vragen beantwoorden:

1. Wat is KEDA?
2. Hoe past KEDA in het DevOps-landschap?
3. Wat zijn de voor- en nadelen van KEDA?
4. Wat zijn alternatieven voor KEDA?
5. Hoe pas ik KEDA toe in het klein?

## Wat is KEDA?

**KEDA,** oftewel Kubernetes Event-Driven Autoscaling, is een open-source component dat dynamische autoscaling mogelijk maakt voor Kubernetes-applicaties. In tegenstelling tot de standaard autoscaling van Kubernetes, die vaak is gebaseerd op CPU- of geheugenbelasting, stelt KEDA applicaties in staat om te schalen op basis van externe gebeurtenissen. Dit betekent dat je bijvoorbeeld je microservice kunt laten schalen op basis van het aantal berichten in een RabbitMQ-wachtrij of de lengte van een Azure Queue.

De kracht van KEDA ligt in zijn flexibiliteit. Het ondersteunt verschillende soorten `triggers` ([Sahoo, 2023](https://devtron.ai/blog/introduction-to-kubernetes-event-driven-autoscaling-keda/#what-is-keda)), zoals message queues, databases, en HTTP-requests. Hiermee kan KEDA zorgen voor een efficiëntere inzet van resources, omdat de applicatie alleen schaalt wanneer er daadwerkelijk vraag is. Dit past goed bij de dynamische architectuur van microservices, waar de belasting sterk kan variëren.

KEDA maakt gebruik van een `ScaledObject` ([KEDA | Autoscaling Azure Pipelines Agents With KEDA, 2021](https://keda.sh/blog/2021-05-27-azure-pipelines-scaler/)), een Kubernetes-resource waarmee je kunt definiëren hoe en wanneer een applicatie moet schalen. Dit object specificeert bijvoorbeeld de naam van de deployment die moet worden geschaald, de minimale en maximale aantallen replica's, en de gebeurtenisbron waarop geschaald moet worden.

Door deze eigenschappen is KEDA geschikt voor organisaties die hun Kubernetes-omgevingen willen optimaliseren op basis van specifieke werklasten en gebeurtenissen, waardoor zij kosten kunnen besparen en efficiënter kunnen omgaan met schaarse resources.

In de volgende sectie zal ik bespreken hoe KEDA past binnen het bredere DevOps-landschap en hoe het kan bijdragen aan een betere CI/CD-pijplijn.

## Hoe past KEDA in het DevOps-landschap?

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



## Bronnen

- Sahoo, J. (2023, 23 augustus). Introduction to Kubernetes Event-Driven Autoscaling (KEDA). https://devtron.ai/blog/introduction-to-kubernetes-event-driven-autoscaling-keda/#what-is-keda
- KEDA | Autoscaling Azure Pipelines agents with KEDA. (2021, 27 mei). KEDA. https://keda.sh/blog/2021-05-27-azure-pipelines-scaler/
- Gupta, A. (2023, 29 december). Scaling Kubernetes: Intro to Kubernetes-based event-driven autoscaling (KEDA). Microsoft Open Source Blog. https://opensource.microsoft.com/blog/2020/05/12/scaling-kubernetes-keda-intro-kubernetes-based-event-driven-autoscaling/
- KEDA. (z.d.). KEDA. https://keda.sh/
- Tomkerkhove. (2024, 6 augustus). Kubernetes Event-driven Autoscaling (KEDA) - Azure Kubernetes Service. Microsoft Learn. https://learn.microsoft.com/en-us/azure/aks/keda-about#capabilities-and-features
- Singh, G. (2024, 8 oktober). KEDA (Kubernetes Event-driven Autoscaling). XenonStack. https://www.xenonstack.com/blog/kubernetes-event-driven-autoscaling