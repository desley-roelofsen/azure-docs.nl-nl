---
title: Concepten-Kubernetes-basis beginselen voor Azure Kubernetes Services (AKS)
description: Meer informatie over de basis onderdelen van het cluster en de workload van Kubernetes en hoe deze zijn gerelateerd aan functies in azure Kubernetes service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: mlearned
ms.openlocfilehash: 78fb06c7ecd20d8ed2af40bcc294f2fb1b166d96
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2019
ms.locfileid: "74120611"
---
# <a name="kubernetes-core-concepts-for-azure-kubernetes-service-aks"></a>Kubernetes core-concepten voor Azure Kubernetes service (AKS)

Wanneer toepassings ontwikkeling naar een op een container gebaseerde aanpak gaat, is het belang rijk om resources te organiseren en te beheren. Kubernetes is het toonaangevende platform dat de mogelijkheid biedt om een betrouw bare planning te bieden voor werk belastingen voor fout tolerante toepassingen. Azure Kubernetes service (AKS) is een beheerde Kubernetes-aanbieding waarmee de implementatie en het beheer van toepassingen op basis van containers worden vereenvoudigd.

In dit artikel worden de belangrijkste onderdelen van de Kubernetes-infra structuur geïntroduceerd, zoals het *besturings vlak*, de *knoop punten*en de *knooppunt groepen*. Werkbelasting resources zoals *peulen*, *implementaties*en *sets* worden ook geïntroduceerd, samen met het groeperen van resources in *naam ruimten*.

## <a name="what-is-kubernetes"></a>Wat is Kubernetes?

Kubernetes is een snel evoluerend platform dat toepassingen op basis van containers en de bijbehorende netwerk-en opslag onderdelen beheert. De focus geldt voor de werk belasting van de toepassing, niet voor de onderliggende onderdelen van de infra structuur. Kubernetes biedt een declaratieve aanpak van implementaties, ondersteund door een robuuste set Api's voor beheer bewerkingen.

U kunt moderne, draag bare, op micro Services gebaseerde toepassingen bouwen en uitvoeren die profiteren van Kubernetes en de beschik baarheid van die toepassings onderdelen beheren. Kubernetes ondersteunt zowel stateless als stateful toepassingen als teams voortgang door de aanneming van op micro Services gebaseerde toepassingen.

Als een open platform kunt u met Kubernetes uw toepassingen bouwen met uw favoriete programmeer taal, besturings systeem, bibliotheken of Messa ging-bus. Bestaande hulpprogram ma's voor continue integratie en continue levering (CI/CD) kunnen worden geïntegreerd met Kubernetes voor het plannen en implementeren van releases.

Azure Kubernetes service (AKS) biedt een beheerde Kubernetes-service die de complexiteit van implementatie-en kern beheer taken reduceert, inclusief het coördineren van upgrades. Het AKS-besturings vlak wordt beheerd door het Azure-platform en u betaalt alleen voor de AKS-knoop punten waarop uw toepassingen worden uitgevoerd. AKS is gebaseerd op de open-source Azure Kubernetes service Engine ([AKS engine][aks-engine]).

## <a name="kubernetes-cluster-architecture"></a>Kubernetes-cluster architectuur

Een Kubernetes-cluster is onderverdeeld in twee onderdelen:

- *Beheer vlak* knooppunten bieden de kern Kubernetes Services en de indeling van werk belastingen van toepassingen.
- *Knoop punten* voeren werk belastingen van uw toepassing uit.

![Kubernetes en knooppunt onderdelen](media/concepts-clusters-workloads/control-plane-and-nodes.png)

## <a name="control-plane"></a>Besturings vlak

Wanneer u een AKS-cluster maakt, wordt automatisch een besturings vlak gemaakt en geconfigureerd. Dit besturings element wordt gegeven als een beheerde Azure-resource die is afgeleid van de gebruiker. Er zijn geen kosten verbonden aan het besturings vlak, alleen de knoop punten die deel uitmaken van het AKS-cluster.

Het besturings vlak bevat de volgende kern Kubernetes-onderdelen:

- *uitvoeren-apiserver* -de API-server is hoe de onderliggende Kubernetes api's worden weer gegeven. Dit onderdeel biedt de interactie voor beheer hulpprogramma's, zoals `kubectl` of het Kubernetes-dash board.
- *etcd* : om de status van uw Kubernetes-cluster en-configuratie te behouden, is de Maxi maal beschik bare *etcd* een sleutel waarde Store in Kubernetes.
- *uitvoeren-scheduler* : wanneer u toepassingen maakt of schaalt, bepaalt de planner op welke knoop punten de werk belasting kan worden uitgevoerd en worden deze gestart.
- *uitvoeren-Controller-Manager* : de controller beheerder ziet een aantal kleinere controllers die acties uitvoeren, zoals het repliceren van peulen en het verwerken van knooppunt bewerkingen.

AKS biedt een besturings vlak voor één Tenant, met een speciale API-server, scheduler, enzovoort. U definieert het aantal en de grootte van de knoop punten en het Azure-platform configureert de beveiligde communicatie tussen het besturings vlak en knoop punten. Interactie met het besturings vlak vindt plaats via Kubernetes Api's, zoals `kubectl` of het Kubernetes-dash board.

Dit Managed Control-vlak houdt in dat u geen onderdelen hoeft te configureren, zoals een Maxi maal beschik bare *etcd* -opslag, maar dit betekent ook dat u niet rechtstreeks toegang hebt tot het besturings vlak. Upgrades naar Kubernetes worden beheerd via de Azure CLI of Azure Portal, waarmee het besturings vlak en vervolgens de knoop punten worden bijgewerkt. Om mogelijke problemen op te lossen, kunt u de logboeken van het controle vlak door Azure Monitor logboeken bekijken.

Als u het besturings vlak op een bepaalde manier moet configureren of directe toegang moet hebben, kunt u uw eigen Kubernetes-cluster implementeren met behulp van [AKS-engine][aks-engine].

Zie [Aanbevolen procedures voor cluster beveiliging en upgrades in AKS][operator-best-practices-cluster-security]voor de bijbehorende aanbevolen procedures.

## <a name="nodes-and-node-pools"></a>Knoop punten en knooppunt groepen

Als u uw toepassingen en ondersteunende services wilt uitvoeren, hebt u een Kubernetes- *knoop punt*nodig. Een AKS-cluster heeft een of meer knoop punten, een virtuele Azure-machine (VM) waarop de Kubernetes-knooppunt onderdelen en de container runtime worden uitgevoerd:

- De `kubelet` is de Kubernetes-agent die de Orchestration-aanvragen verwerkt vanuit het besturings vlak en de planning van het uitvoeren van de aangevraagde containers.
- Virtuele netwerken worden verwerkt door de *uitvoeren-proxy* op elk knoop punt. De proxy routeert netwerk verkeer en beheert IP-adres sering voor services en peulen.
- De *container runtime* is het onderdeel dat container toepassingen toestaat om uit te voeren en te communiceren met aanvullende bronnen zoals het virtuele netwerk en de opslag. In AKS wordt Moby gebruikt als container runtime.

![Virtuele Azure-machine en ondersteunende bronnen voor een Kubernetes-knoop punt](media/concepts-clusters-workloads/aks-node-resource-interactions.png)

De grootte van de Azure VM voor uw knoop punten bepaalt hoeveel Cpu's, hoeveel geheugen en de grootte en het type beschik bare opslag (zoals een hoge prestatie SSD of normale HDD). Als u verwacht dat toepassingen die grote hoeveel heden CPU en geheugen of hoge prestaties vereisen, moeten de knooppunt grootte dienovereenkomstig plannen. U kunt ook het aantal knoop punten in uw AKS-cluster omhoog schalen om aan de vraag te voldoen.

In AKS is de VM-installatie kopie voor de knoop punten in uw cluster momenteel gebaseerd op Ubuntu Linux of Windows Server 2019. Wanneer u een AKS-cluster maakt of het aantal knoop punten schaalt, wordt het aangevraagde aantal Vm's door het Azure-platform gemaakt en geconfigureerd. Er is geen hand matige configuratie die u kunt uitvoeren. Agent knooppunten worden gefactureerd als standaard virtuele machines, dus eventuele kortingen op de VM-grootte die u gebruikt (inclusief [Azure-reserve ringen][reservation-discounts]) worden automatisch toegepast.

Als u een ander host-besturings systeem, container runtime of aangepaste pakketten wilt gebruiken, kunt u uw eigen Kubernetes-cluster implementeren met behulp van [AKS-engine][aks-engine]. De upstream-`aks-engine` releases functies en bevat configuratie opties voordat ze officieel worden ondersteund in AKS-clusters. Als u bijvoorbeeld een andere container-runtime dan Moby wilt gebruiken, kunt u `aks-engine` gebruiken om een Kubernetes-cluster te configureren en implementeren dat voldoet aan uw huidige behoeften.

### <a name="resource-reservations"></a>Resource reserveringen

Knooppunt resources worden gebruikt door AKS om de knooppunt functie als onderdeel van het cluster te maken. Dit kan een verschil maken tussen het totale aantal resources van het knoop punt en de resources die kunnen worden verplaatst wanneer ze worden gebruikt in AKS. Dit is belang rijk om te weten wanneer u aanvragen en limieten instelt voor een door de gebruiker geïmplementeerde peul.

Ga als volgt te werk om te zoeken naar de toewijs bare resources van een knoop punt:
```kubectl
kubectl describe node [NODE_NAME]

```

Als u de prestaties en functionaliteit van knoop punten wilt behouden, worden de resources op elk knoop punt gereserveerd door AKS. Naarmate een knoop punt groter wordt in resources, neemt de resource reservering toe als gevolg van een grotere mate van gebruiker die heel veel beheer nodig heeft.

>[!NOTE]
> Het gebruik van AKS-invoeg toepassingen zoals container Insights (OMS) neemt extra knooppunt bronnen in beslag.

- **CPU-** gereserveerde CPU is afhankelijk van het knooppunt type en de cluster configuratie, waardoor er minder TOEWIJS bare CPU kan optreden vanwege het uitvoeren van extra functies

| CPU-kernen op de host | 1 | 2 | 4 | 8 | 16 | 32|64|
|---|---|---|---|---|---|---|---|
|Uitvoeren-gereserveerd (millicores)|60|100|140|180|260|420|740|

- **Geheugen-gereserveerd** geheugen bevat de som van twee waarden

1. De kubelet-daemon wordt geïnstalleerd op alle knoop punten van de Kubernetes-agent om het maken en beëindigen van containers te beheren. Deze daemon bevat standaard de volgende verwijderings regel: memory. available < 750Mi, wat betekent dat een knoop punt altijd ten minste 750 mi te allen tijde kan hebben.  Wanneer een host lager is dan de drempel waarde van het beschik bare geheugen, wordt een van de kubelet beëindigd om geheugen vrij te maken op de hostcomputer en te beveiligen.

2. De tweede waarde is een progressieve hoeveelheid geheugen die is gereserveerd voor de kubelet-daemon goed te laten functioneren (uitvoeren-gereserveerd).
    - 25% van de eerste 4 GB geheugen
    - 20% van de volgende 4 GB geheugen (Maxi maal 8 GB)
    - 10% van de volgende 8 GB geheugen (Maxi maal 16 GB)
    - 6% van de volgende 112 GB geheugen (Maxi maal 128 GB)
    - 2% van de geheugens boven 128 GB

Als gevolg van deze twee gedefinieerde regels die zijn opgelegd om Kubernetes en agent knooppunten in orde te blijven, wordt de hoeveelheid toewijs bare CPU en het geheugen kleiner weer gegeven dan het knoop punt zelf zou kunnen aanbieden. De resource reserveringen die hierboven zijn gedefinieerd, kunnen niet worden gewijzigd.

Als een knoop punt bijvoorbeeld 7 GB biedt, zal het 34% van het geheugen niet worden verplaatst:

`750Mi + (0.25*4) + (0.20*3) = 0.786GB + 1 GB + 0.6GB = 2.386GB / 7GB = 34% reserved`

Naast reserve ringen voor Kubernetes, reserveert het onderliggende knooppunt besturingssysteem ook een hoeveelheid CPU-en geheugen bronnen om besturings systemen te onderhouden.

Zie [Best Practices for Basic scheduler-functies in AKS][operator-best-practices-scheduler]voor gekoppelde aanbevolen procedures.

### <a name="node-pools"></a>Knooppunt groepen

Knoop punten van dezelfde configuratie worden samen in *knooppunt groepen*gegroepeerd. Een Kubernetes-cluster bevat een of meer knooppunt groepen. Het eerste aantal knoop punten en grootte worden gedefinieerd wanneer u een AKS-cluster maakt, waarmee een *standaard knooppunt groep*wordt gemaakt. Deze standaard knooppunt groep in AKS bevat de onderliggende virtuele machines waarop de agent knooppunten worden uitgevoerd.

> [!NOTE]
> Om ervoor te zorgen dat uw cluster betrouwbaar functioneert, moet u ten minste twee knoop punten in de standaard knooppunt groep uitvoeren.

Wanneer u een AKS-cluster schaalt of bijwerkt, wordt de actie uitgevoerd op basis van de standaard knooppunt groep. U kunt er ook voor kiezen om een specifieke knooppunt groep te schalen of bij te werken. Voor upgrade bewerkingen worden actieve containers gepland op andere knoop punten in de knooppunt groep totdat alle knoop punten zijn bijgewerkt.

Zie [meerdere knooppunt groepen maken en beheren voor een cluster in AKS][use-multiple-node-pools]voor meer informatie over het gebruik van meerdere knooppunt groepen in AKS.

### <a name="node-selectors"></a>Knooppunt selecties

In een AKS-cluster dat meerdere knooppunt groepen bevat, moet u mogelijk de Kubernetes-planner laten weten welke knooppunt groep voor een bepaalde resource moet worden gebruikt. Inactieve controllers mogen bijvoorbeeld niet worden uitgevoerd op Windows Server-knoop punten (momenteel in de preview-versie van AKS). Met knooppunt selecties kunt u verschillende para meters definiëren, zoals het besturings systeem van het knoop punt, om te bepalen waar een pod moet worden gepland.

In het volgende eenvoudige voor beeld wordt een NGINX-exemplaar gepland op een Linux-knoop punt met behulp van de knooppunt kiezer *' Beta.kubernetes.io/OS ': Linux*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx
spec:
  containers:
    - name: myfrontend
      image: nginx:1.15.12
  nodeSelector:
    "beta.kubernetes.io/os": linux
```

Zie [Aanbevolen procedures voor geavanceerde functies van scheduler in AKS][operator-best-practices-advanced-scheduler]voor meer informatie over het bepalen van de planning.

## <a name="pods"></a>Pods

Kubernetes maakt gebruik van *peul* om een exemplaar van uw toepassing uit te voeren. Een pod vertegenwoordigt één exemplaar van uw toepassing. In het algemeen is er sprake van een 1:1-toewijzing met een container, hoewel er geavanceerde scenario's zijn waarbij een pod mogelijk meerdere containers bevat. Deze meerdere containers worden op hetzelfde knoop punt gepland en kunnen containers gerelateerde resources delen.

Wanneer u een pod maakt, kunt u *resource limieten* definiëren om een bepaalde hoeveelheid CPU-of geheugen bronnen aan te vragen. De Kubernetes scheduler probeert het peul te plannen dat wordt uitgevoerd op een knoop punt met beschik bare bronnen om te voldoen aan de aanvraag. U kunt ook maximale resource limieten opgeven waarmee wordt voor komen dat een bepaalde pod te veel Compute-resources van het onderliggende knoop punt verbruikt. Een best practice bestaat uit het opneemen van resource limieten voor alle peulen om de Kubernetes te helpen begrijpen welke resources nodig zijn en zijn toegestaan.

Zie [Kubernetes peul][kubernetes-pods] en [Kubernetes pod Lifecycle][kubernetes-pod-lifecycle]voor meer informatie.

Een Pod is een logische resource, maar de container (s) zijn waar de werk belasting van de toepassing wordt uitgevoerd. De meeste zijn doorgaans tijdelijke, wegwerp bronnen en individueel geplande een aantal van de hoge Beschik baarheid en redundantie functies die Kubernetes biedt. In plaats daarvan worden er meestal peulen geïmplementeerd en beheerd door Kubernetes- *controllers*, zoals de implementatie controller.

## <a name="deployments-and-yaml-manifests"></a>Implementaties en YAML-manifesten

Een *implementatie* vertegenwoordigt een of meer identieke peulen, beheerd door de Kubernetes-implementatie controller. In een implementatie wordt het aantal *replica's* (peul) gedefinieerd dat moet worden gemaakt, en de Kubernetes-planner zorgt ervoor dat als er bij de meeste of knoop punten problemen optreden, extra peulen worden gepland op gezonde knoop punten.

U kunt implementaties bijwerken om de configuratie van een Peul, container installatie kopie of gekoppelde opslag te wijzigen. De implementatie controller verwerkt en beëindigt een gegeven aantal replica's, maakt replica's van de nieuwe implementatie definitie en gaat door met het proces totdat alle replica's in de implementatie worden bijgewerkt.

Voor de meeste stateless toepassingen in AKS moet het implementatie model worden gebruikt in plaats van dat er afzonderlijke peulen worden gepland. Kubernetes kan de status en status van implementaties controleren om ervoor te zorgen dat het vereiste aantal replica's binnen het cluster wordt uitgevoerd. Wanneer u alleen afzonderlijke peulen plant, wordt het meren aantal niet opnieuw gestart als er een probleem optreedt en worden ze niet opnieuw gepland op gezonde knoop punten als het huidige knoop punt een probleem aantreft.

Als een toepassing een quorum van instanties vereist om altijd beschikbaar te zijn voor het beheer van beslissingen, wilt u niet dat een update proces die mogelijkheid verstoort. *Pod-onderbrekings budgetten* kunnen worden gebruikt om te definiëren hoeveel replica's in een implementatie kunnen worden uitgevoerd tijdens een update of een upgrade van een knoop punt. Als u bijvoorbeeld *vijf* replica's in uw implementatie hebt, kunt u een pod-onderbreking van *4* definiëren, zodat er slechts één replica tegelijk mag worden verwijderd of opnieuw wordt gepland. Net als bij pod-resource limieten bestaat een best practice uit het definiëren van pod-onderbrekings budgetten voor toepassingen waarvoor een minimum aantal replica's vereist is om altijd aanwezig te zijn.

Implementaties worden doorgaans gemaakt en beheerd met `kubectl create` of `kubectl apply`. Als u een implementatie wilt maken, definieert u een manifest bestand in de indeling YAML (YAML Ain't Markup Language). In het volgende voor beeld wordt een basis implementatie van de NGINX-webserver gemaakt. In de implementatie worden *drie* te maken replica's opgegeven en wordt poort *80* geopend op de container. Resource aanvragen en-limieten worden ook gedefinieerd voor de CPU en het geheugen.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.2
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 250m
            memory: 64Mi
          limits:
            cpu: 500m
            memory: 256Mi
```

Complexere toepassingen kunnen worden gemaakt door ook services zoals load balancers binnen het YAML-manifest op te nemen.

Zie [Kubernetes-implementaties][kubernetes-deployments]voor meer informatie.

### <a name="package-management-with-helm"></a>Pakket beheer met helm

Een veelvoorkomende benadering van het beheren van toepassingen in Kubernetes is met [helm][helm]. U kunt bestaande open bare helm- *grafieken* maken en gebruiken die een verpakte versie van de toepassings code en Kubernetes yaml-manifesten bevatten voor het implementeren van resources. Deze helm-grafieken kunnen lokaal worden opgeslagen of vaak in een externe opslag plaats, zoals een [Azure container Registry helm-grafiek opslag plaats][acr-helm].

Als u helm wilt gebruiken, wordt er een server onderdeel met de naam *Tiller* geïnstalleerd in uw Kubernetes-cluster. De Tiller beheert de installatie van grafieken binnen het cluster. De helm-client zelf is lokaal op uw computer geïnstalleerd of kan worden gebruikt binnen het [Azure Cloud shell][azure-cloud-shell]. U kunt helm-grafieken zoeken of maken met de-client en deze vervolgens installeren op uw Kubernetes-cluster.

![Helm bevat een client onderdeel en een Tiller-onderdeel aan de server zijde dat bronnen in het Kubernetes-cluster maakt](media/concepts-clusters-workloads/use-helm.png)

Zie [Installing Applications with helm in azure Kubernetes service (AKS) (Engelstalig)][aks-helm]voor meer informatie.

## <a name="statefulsets-and-daemonsets"></a>StatefulSets en DaemonSets

De implementatie controller maakt gebruik van de Kubernetes Planner om een bepaald aantal replica's uit te voeren op elk beschikbaar knoop punt met beschik bare resources. Deze methode voor het gebruik van implementaties kan voldoende zijn voor stateless toepassingen, maar niet voor toepassingen waarvoor een permanente naam Conventie of opslag vereist is. Voor toepassingen waarvoor een replica moet bestaan op elk knoop punt, of geselecteerde knoop punten, binnen een cluster, wordt de implementatie controller niet weer geven op hoe replica's over de knoop punten worden gedistribueerd.

Er zijn twee Kubernetes-resources waarmee u deze typen toepassingen kunt beheren:

- *StatefulSets* : Houd de status van toepassingen na een afzonderlijke pod-levens cyclus, zoals opslag.
- *DaemonSets* : Zorg ervoor dat op elk knoop punt een actief exemplaar van de Kubernetes-Boots trap-procedure wordt uitgevoerd.

### <a name="statefulsets"></a>StatefulSets

Ontwikkeling van moderne toepassingen is vaak gericht op stateless toepassingen, maar *StatefulSets* kan worden gebruikt voor stateful toepassingen, zoals toepassingen die database onderdelen bevatten. Een StatefulSet is vergelijkbaar met een implementatie in dat een of meer identieke peulen worden gemaakt en beheerd. Replica's in een StatefulSet volgen een gepaste, sequentiële benadering van implementatie, schaal, upgrades en beëindigingen. Met een StatefulSet zijn de naam Conventie, netwerk namen en opslag persistent, omdat replica's opnieuw worden gepland.

U definieert de toepassing in YAML-indeling met behulp van `kind: StatefulSet`en de StatefulSet-controller verwerkt vervolgens de implementatie en het beheer van de vereiste replica's. Gegevens worden naar permanente opslag geschreven, die wordt verschaft door Azure Managed Disks of Azure Files. Met StatefulSets blijft de onderliggende permanente opslag behouden, zelfs wanneer de StatefulSet is verwijderd.

Zie [Kubernetes StatefulSets][kubernetes-statefulsets]voor meer informatie.

Replica's in een StatefulSet worden gepland en uitgevoerd op alle beschik bare knoop punten in een AKS-cluster. Als u er zeker van wilt zijn dat er ten minste één pod in uw set wordt uitgevoerd op een knoop punt, kunt u in plaats daarvan een Daemonset gebruiken.

### <a name="daemonsets"></a>DaemonSets

Voor specifieke vereisten voor logboek verzameling of-bewaking moet u mogelijk een opgegeven pod uitvoeren op alle, of geselecteerd, knoop punten. Een *daemonset* wordt opnieuw gebruikt om een of meer identieke peulen te implementeren, maar de controller van de daemonset zorgt ervoor dat elk opgegeven knoop punt een exemplaar van de pod uitvoert.

De controller van de Daemonset kan in het proces voor het opstarten van het cluster in een vroeg stadium plannen voordat de standaard Kubernetes scheduler is gestart. Op deze manier zorgt u ervoor dat het Peul in een Daemonset wordt gestart voordat het traditionele peul in een implementatie of StatefulSet wordt gepland.

Net als StatefulSets wordt een Daemonset gedefinieerd als onderdeel van een YAML definitie met behulp van `kind: DaemonSet`.

Zie [Kubernetes DaemonSets][kubernetes-daemonset]voor meer informatie.

> [!NOTE]
> Als de [add-on van de virtuele knoop punten](virtual-nodes-cli.md#enable-virtual-nodes-addon)wordt gebruikt, maakt DaemonSets geen peul op het virtuele knoop punt.

## <a name="namespaces"></a>Naamruimten

Kubernetes-resources, zoals peulen en implementaties, worden logisch gegroepeerd in een *naam ruimte*. Deze groeperingen bieden een manier om een AKS-cluster logisch te verdelen en de toegang te beperken tot het maken, weer geven of beheren van resources. U kunt bijvoorbeeld naam ruimten maken om bedrijfs groepen te scheiden. Gebruikers kunnen alleen communiceren met resources binnen hun toegewezen naam ruimten.

![Kubernetes-naam ruimten om resources en toepassingen logisch te verdelen](media/concepts-clusters-workloads/namespaces.png)

Wanneer u een AKS-cluster maakt, zijn de volgende naam ruimten beschikbaar:

- *standaard* -deze naam ruimte is de standaard instelling voor de meeste en implementaties, wanneer er geen wordt gegeven. In kleinere omgevingen kunt u toepassingen rechtstreeks in de standaard naam ruimte implementeren zonder dat u extra logische schei dingen hoeft te maken. Wanneer u communiceert met de Kubernetes-API, zoals met `kubectl get pods`, wordt de standaard naam ruimte gebruikt wanneer er geen is opgegeven.
- *uitvoeren-systeem* : deze naam ruimte is de belangrijkste bronnen, zoals netwerk functies, zoals DNS en proxy, of het Kubernetes-dash board. Normaal gesp roken implementeert u uw eigen toepassingen niet in deze naam ruimte.
- *uitvoeren-Public* : deze naam ruimte wordt meestal niet gebruikt, maar kan worden gebruikt voor resources die zichtbaar zijn in het hele cluster en kunnen worden weer gegeven door elke wille keurige gebruiker.

Zie [Kubernetes-naam ruimten][kubernetes-namespaces]voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

In dit artikel worden enkele van de belangrijkste Kubernetes-onderdelen beschreven en hoe deze van toepassing zijn op AKS-clusters. Raadpleeg de volgende artikelen voor meer informatie over de belangrijkste Kubernetes-en AKS-concepten:

- [Kubernetes/AKS-toegang en-identiteit][aks-concepts-identity]
- [Kubernetes/AKS-beveiliging][aks-concepts-security]
- [Kubernetes/AKS virtuele netwerken][aks-concepts-network]
- [Kubernetes/AKS-opslag][aks-concepts-storage]
- [Kubernetes/AKS-schaal][aks-concepts-scale]

<!-- EXTERNAL LINKS -->
[aks-engine]: https://github.com/Azure/aks-engine
[kubernetes-pods]: https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/
[kubernetes-pod-lifecycle]: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/
[kubernetes-deployments]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-statefulsets]: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
[kubernetes-namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[helm]: https://helm.sh/
[azure-cloud-shell]: https://shell.azure.com

<!-- INTERNAL LINKS -->
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[acr-helm]: ../container-registry/container-registry-helm-repos.md
[aks-helm]: kubernetes-helm.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[operator-best-practices-scheduler]: operator-best-practices-scheduler.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[reservation-discounts]: ../billing/billing-save-compute-costs-reservations.md