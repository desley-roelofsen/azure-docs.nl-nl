---
title: Dynamisch een permanent volume maken en gebruiken met Azure-schijven in azure Kubernetes service (AKS)
description: Meer informatie over het dynamisch maken van een permanent volume met Azure-schijven in azure Kubernetes service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 03/01/2019
ms.author: mlearned
ms.openlocfilehash: 1c7a406f0b06b94aaa6d8b4af63b1416b11c7c56
ms.sourcegitcommit: 16c5374d7bcb086e417802b72d9383f8e65b24a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2019
ms.locfileid: "73847334"
---
# <a name="dynamically-create-and-use-a-persistent-volume-with-azure-disks-in-azure-kubernetes-service-aks"></a>Dynamisch een permanent volume maken en gebruiken met Azure-schijven in azure Kubernetes service (AKS)

Een permanent volume vertegenwoordigt een opslag ruimte die is ingericht voor gebruik met Kubernetes peul. Een permanent volume kan worden gebruikt door een of meer peulen en kan dynamisch of statisch worden ingericht. In dit artikel wordt beschreven hoe u dynamisch permanente volumes maakt met Azure-schijven voor gebruik door één pod in een Azure Kubernetes service-cluster (AKS).

> [!NOTE]
> Een Azure-schijf kan alleen worden gekoppeld met de *toegangs modus* type *ReadWriteOnce*, waardoor deze alleen beschikbaar is voor één pod in AKS. Als u een permanent volume op meerdere peulen wilt delen, gebruikt u [Azure files][azure-files-pvc].

Zie [opslag opties voor toepassingen in AKS][concepts-storage]voor meer informatie over Kubernetes-volumes.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u beschikt over een bestaand AKS-cluster. Als u een AKS-cluster nodig hebt, raadpleegt u de AKS Quick Start [met behulp van de Azure cli][aks-quickstart-cli] of [met behulp van de Azure Portal][aks-quickstart-portal].

Ook moet de Azure CLI-versie 2.0.59 of hoger zijn geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, raadpleegt u [Azure cli installeren][install-azure-cli].

## <a name="built-in-storage-classes"></a>Ingebouwde opslag klassen

Een opslag klasse wordt gebruikt om te definiëren hoe een opslag eenheid dynamisch wordt gemaakt met een permanent volume. Zie [Kubernetes Storage klassen][kubernetes-storage-classes](Engelstalig) voor meer informatie over Kubernetes-opslag klassen.

Elk AKS-cluster bevat twee vooraf gemaakte opslag klassen, die zijn geconfigureerd om te werken met Azure-schijven:

* De *standaard* opslag klasse voorziet in een standaard Azure-schijf.
    * Standaard opslag wordt ondersteund door Hdd's en levert rendabele opslag capaciteit terwijl deze nog steeds wordt uitgevoerd. Standard-schijven zijn ideaal voor een kostenefficiënte werkbelasting voor ontwikkelen en testen.
* De *beheerde-Premium-* opslag klasse richt zich op een Premium Azure-schijf.
    * Premium-schijven worden ondersteund door hoogwaardige schijven met een lage latentie op basis van SSD. Ideaal voor virtuele machines met een productiewerkbelasting. Als de AKS-knoop punten in uw cluster Premium Storage gebruiken, selecteert u de *beheerde-Premium-* klasse.
    
Bij deze standaard opslag klassen mag u de volume grootte niet bijwerken nadat deze is gemaakt. Als u deze mogelijkheid wilt inschakelen, voegt u de regel *allowVolumeExpansion: True* toe aan een van de standaard-opslag klassen of maakt u een eigen aangepaste opslag klasse. U kunt een bestaande opslag klasse bewerken met behulp van de `kubectl edit sc` opdracht. Zie [opslag opties voor toepassingen in AKS][storage-class-concepts]voor meer informatie over opslag klassen en het maken van uw eigen gegevens.

Gebruik de opdracht [kubectl Get SC][kubectl-get] om de vooraf gemaakte opslag klassen te bekijken. In het volgende voor beeld ziet u de opslag klassen vooraf maken die beschikbaar zijn in een AKS-cluster:

```console
$ kubectl get sc

NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

> [!NOTE]
> Permanente volume claims worden opgegeven in GiB, maar Azure Managed disks worden gefactureerd op basis van de SKU voor een specifieke grootte. Deze Sku's variëren van 32GiB voor S4-of P4-schijven naar 32TiB voor S80-of P80-schijven (in preview-versie). De prestaties van de door Voer en IOPS van een Premium Managed disk zijn afhankelijk van de SKU en de instantie grootte van de knoop punten in het AKS-cluster. Zie [prijzen en prestaties van Managed disks][managed-disk-pricing-performance]voor meer informatie.

## <a name="create-a-persistent-volume-claim"></a>Een permanente volume claim maken

Een permanente volume claim (PVC) wordt gebruikt voor het automatisch inrichten van opslag op basis van een opslag klasse. In dit geval kan een PVC een van de vooraf gemaakte opslag klassen gebruiken om een standaard of Premium Azure Managed disk te maken.

Maak een bestand met de naam `azure-premium.yaml`en kopieer het in het volgende manifest. De claim vraagt een schijf met de naam `azure-managed-disk` die *5 GB* is met *ReadWriteOnce* -toegang. De *beheerde-Premium-* opslag klasse wordt opgegeven als de opslag klasse.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

> [!TIP]
> Als u een schijf wilt maken die gebruikmaakt van standaard opslag, gebruikt u `storageClassName: default` in plaats van *Managed-Premium*.

Maak de permanente volume claim met de opdracht [kubectl apply][kubectl-apply] en geef het bestand *Azure-Premium. yaml* op:

```console
$ kubectl apply -f azure-premium.yaml

persistentvolumeclaim/azure-managed-disk created
```

## <a name="use-the-persistent-volume"></a>Het permanente volume gebruiken

Als de permanente volume claim is gemaakt en de schijf is ingericht, kan een pod worden gemaakt met toegang tot de schijf. In het volgende manifest wordt een eenvoudige NGINX-pod gemaakt die gebruikmaakt van de permanente volume claim met de naam *Azure-Managed-Disk* om de Azure-schijf te koppelen aan het pad `/mnt/azure`. Voor Windows Server-containers (momenteel in de preview-versie van AKS) geeft u een *mountPath* op met behulp van de Windows Path-Conventie, zoals *: '* .

Maak een bestand met de naam `azure-pvc-disk.yaml`en kopieer het in het volgende manifest.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
    - mountPath: "/mnt/azure"
      name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

Maak de Pod met de opdracht [kubectl apply][kubectl-apply] , zoals wordt weer gegeven in het volgende voor beeld:

```console
$ kubectl apply -f azure-pvc-disk.yaml

pod/mypod created
```

U hebt nu een actieve pod met uw Azure-schijf die is gekoppeld in de `/mnt/azure` Directory. Deze configuratie kan worden weer gegeven bij het controleren van uw Pod via `kubectl describe pod mypod`, zoals wordt weer gegeven in het volgende versmald-voor beeld:

```console
$ kubectl describe pod mypod

[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azure-managed-disk
    ReadOnly:   false
  default-token-smm2n:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-smm2n
    Optional:    false
[...]
Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              2m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  2m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-smm2n"
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "pvc-faf0f176-8b8d-11e8-923b-deb28c58d242"
[...]
```

## <a name="back-up-a-persistent-volume"></a>Back-up maken van een permanent volume

Als u een back-up wilt maken van de gegevens in uw permanente volume, maakt u een moment opname van de beheerde schijf voor het volume. U kunt deze moment opname vervolgens gebruiken om een herstelde schijf te maken en te koppelen aan een manier om de gegevens te herstellen.

Haal eerst de volume naam op met de opdracht `kubectl get pvc`, bijvoorbeeld voor het PVC met de naam *Azure-Managed-Disk*:

```console
$ kubectl get pvc azure-managed-disk

NAME                 STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS      AGE
azure-managed-disk   Bound     pvc-faf0f176-8b8d-11e8-923b-deb28c58d242   5Gi        RWO            managed-premium   3m
```

Deze volume naam vormt de onderliggende naam van de Azure-schijf. Zoek naar de schijf-ID met [AZ Disk List][az-disk-list] en geef de volume naam van uw PVC op, zoals wordt weer gegeven in het volgende voor beeld:

```azurecli-interactive
$ az disk list --query '[].id | [?contains(@,`pvc-faf0f176-8b8d-11e8-923b-deb28c58d242`)]' -o tsv

/subscriptions/<guid>/resourceGroups/MC_MYRESOURCEGROUP_MYAKSCLUSTER_EASTUS/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

Gebruik de schijf-ID om een momentopname schijf te maken met [AZ snap shot Create][az-snapshot-create]. In het volgende voor beeld wordt een moment opname gemaakt met de naam *pvcSnapshot* in dezelfde resource groep als de AKS-cluster (*MC_myResourceGroup_myAKSCluster_eastus*). U kunt problemen met de machtigingen ondervinden als u moment opnamen maakt en schijven herstelt in resource groepen waartoe het AKS-cluster geen toegang heeft.

```azurecli-interactive
$ az snapshot create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name pvcSnapshot \
    --source /subscriptions/<guid>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

Afhankelijk van de hoeveelheid gegevens op uw schijf kan het enkele minuten duren voordat de moment opname is gemaakt.

## <a name="restore-and-use-a-snapshot"></a>Een moment opname herstellen en gebruiken

Als u de schijf wilt herstellen en wilt gebruiken met een Kubernetes-Pod, gebruikt u de moment opname als bron bij het maken van een schijf met [AZ Disk Create][az-disk-create]. Met deze bewerking wordt de oorspronkelijke resource behouden als u vervolgens toegang moet hebben tot de oorspronkelijke gegevens momentopname. In het volgende voor beeld wordt een schijf met de naam *pvcRestored* gemaakt in de moment opname met de naam *pvcSnapshot*:

```azurecli-interactive
az disk create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --source pvcSnapshot
```

Als u de herstelde schijf wilt gebruiken met een Pod, geeft u de ID van de schijf op in het manifest. Haal de schijf-ID op met de opdracht [AZ Disk show][az-disk-show] . In het volgende voor beeld wordt de schijf-ID opgehaald voor *pvcRestored* die zijn gemaakt in de vorige stap:

```azurecli-interactive
az disk show --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --query id -o tsv
```

Maak een pod-manifest met de naam `azure-restored.yaml` en geef de schijf-URI op die in de vorige stap is verkregen. In het volgende voor beeld wordt een eenvoudige NGINX-webserver gemaakt, waarbij de herstelde schijf is gekoppeld als een volume op */mnt/Azure*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypodrestored
spec:
  containers:
  - name: mypodrestored
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
    - mountPath: "/mnt/azure"
      name: volume
  volumes:
    - name: volume
      azureDisk:
        kind: Managed
        diskName: pvcRestored
        diskURI: /subscriptions/<guid>/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
```

Maak de Pod met de opdracht [kubectl apply][kubectl-apply] , zoals wordt weer gegeven in het volgende voor beeld:

```console
$ kubectl apply -f azure-restored.yaml

pod/mypodrestored created
```

U kunt `kubectl describe pod mypodrestored` gebruiken om de details van de pod weer te geven, zoals het volgende verkorte voor beeld waarin de volume gegevens worden weer gegeven:

```console
$ kubectl describe pod mypodrestored

[...]
Volumes:
  volume:
    Type:         AzureDisk (an Azure Data Disk mount on the host and bind mount to the pod)
    DiskName:     pvcRestored
    DiskURI:      /subscriptions/19da35d3-9a1a-4f3b-9b9c-3c56ef409565/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
    Kind:         Managed
    FSType:       ext4
    CachingMode:  ReadWrite
    ReadOnly:     false
[...]
```

## <a name="next-steps"></a>Volgende stappen

Zie [Aanbevolen procedures voor opslag en back-ups in AKS][operator-best-practices-storage]voor gekoppelde aanbevolen procedures.

Meer informatie over Kubernetes permanente volumes met behulp van Azure-schijven.

> [!div class="nextstepaction"]
> [Kubernetes-invoeg toepassing voor Azure-schijven][azure-disk-volume]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/disks-types.md
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-snapshot-create]: /cli/azure/snapshot#az-snapshot-create
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-disk-show]: /cli/azure/disk#az-disk-show
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
[storage-class-concepts]: concepts-storage.md#storage-classes
