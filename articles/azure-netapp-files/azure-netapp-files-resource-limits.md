---
title: Resource limieten voor Azure NetApp Files | Microsoft Docs
description: Beschrijft de limieten voor Azure NetApp Files resources en hoe toename van de resource limiet kan worden aangevraagd.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: b-juche
ms.openlocfilehash: 6fcea0aaecb860e07c2066877494c05b51f43ca4
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74976244"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Resourcelimieten voor Azure NetApp Files

Informatie over resource limieten voor Azure NetApp Files helpt u bij het beheren van uw volumes.

## <a name="resource-limits"></a>Bronbeperkingen

In de volgende tabel worden resource limieten voor Azure NetApp Files beschreven:

|  Bron  |  Standaardlimiet  |  Aanpasbaar via ondersteunings aanvraag  |
|----------------|---------------------|--------------------------------------|
|  Aantal NetApp-accounts per Azure-regio   |  10    |  Ja   |
|  Aantal capaciteits Pools per NetApp-account   |    25     |   Ja   |
|  Aantal volumes per capaciteits pool     |    500   |    Ja     |
|  Aantal moment opnamen per volume       |    255     |    Nee        |
|  Aantal subnetten dat wordt gedelegeerd aan Azure NetApp Files (micro soft. NetApp/volumes) per Azure-Virtual Network    |   1   |    Nee    |
|  Aantal IP-adressen in een VNet (inclusief peered VNets) die toegang hebben tot Azure NetApp Files   |    1000   |    Ja   |
|  Minimum grootte van één capaciteits groep   |  4 TiB     |    Nee  |
|  Maximale grootte van één capaciteits groep    |  500 TiB   |   Nee   |
|  Minimum grootte van één volume    |    100 GiB    |    Nee    |
|  Maximale grootte van één volume     |    100 TiB    |    Nee    |
|  Maximum aantal bestanden ([maxfiles](#maxfiles)) per volume     |    100.000.000    |    Ja    |    
|  Maximale grootte van één bestand     |    16 TiB    |    Nee    |    

## Limieten voor maxfiles<a name="maxfiles"></a> 

Azure NetApp Files volumes hebben een limiet van *maxfiles*. De limiet voor maxfiles is het aantal bestanden dat een volume kan bevatten. De limiet voor het aantal maxfiles voor een Azure NetApp Files volume wordt geïndexeerd op basis van de grootte (quotum) van het volume. De maxfiles-limiet voor een volume verhoogt of verlaagt de snelheid van 20.000.000 bestanden per TiB van de ingerichte grootte van het volume. 

De service past de maxfiles-limiet voor een volume dynamisch aan op basis van de ingerichte grootte. Een volume dat aanvankelijk is geconfigureerd met een grootte van 1 TiB zou bijvoorbeeld een maxfiles-limiet hebben van 20.000.000. Volgende wijzigingen in de grootte van het volume zouden leiden tot een automatische aanpassing van de limiet van maxfiles op basis van de volgende regels: 

|    Volume grootte (quotum)     |  Automatische aanpassing van de limiet van maxfiles    |
|----------------------------|-------------------|
|    < 1 TiB                 |    20.000.000     |
|    > = 1 TiB, maar < 2 TiB    |    40.000.000     |
|    > = 2 TiB, maar < 3 TiB    |    60.000.000     |
|    > = 3 TiB, maar < 4 TiB    |    80.000.000     |
|    > = 4 TiB                |    100.000.000    |

Voor elke volume grootte kunt u een [ondersteunings aanvraag](#limit_increase) initiëren om de limiet van maxfiles meer dan 100.000.000 te verhogen.

## Toename van aanvraag limiet<a name="limit_increase"></a> 

U kunt een ondersteunings aanvraag voor Azure maken om de aanpas bare limieten van de bovenstaande tabel te verg Roten. 

Vanuit Azure Portal navigatie vlak: 

1. Klik op **Help en ondersteuning**.
2. Klik op **+ nieuwe ondersteunings aanvraag**.
3. Geef op het tabblad basis beginselen de volgende informatie op: 
    1. Probleem type: Selecteer **service-en abonnements limieten (quota's)** .
    2. Abonnementen: Selecteer het abonnement voor de resource waarvoor u het quotum hebt verhoogd.
    3. Quotum type: Selecteer **opslag: Azure NetApp files limieten**.
    4. Klik op **volgende: oplossingen**.
4. Op het tabblad Details:
    1. Geef in het vak Beschrijving de volgende informatie op voor het bijbehorende resource type:

        |  Bron  |    Bovenliggende resources      |    Aangevraagde nieuwe limieten     |    Reden voor toename van quotum       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Account |  *Subscription ID*   |  *Aangevraagd nieuw Maxi maal **account** nummer*    |  *Welk scenario of use-case vraagt de aanvraag?*  |
        |  Pool    |  *Abonnements-ID, account-URI*  |  *Nieuw maximum **groeps** nummer aangevraagd*   |  *Welk scenario of use-case vraagt de aanvraag?*  |
        |  Volume  |  *Abonnements-ID, account-URI, groeps-URI*   |  *Nieuw maximum **volume** nummer aangevraagd*     |  *Welk scenario of use-case vraagt de aanvraag?*  |
        |  Maxfiles  |  *Abonnements-ID, account-URI, groeps-URI, volume-URI*   |  *Aangevraagd aantal voor nieuwe maximum **maxfiles***     |  *Welk scenario of use-case vraagt de aanvraag?*  |    

    2. Geef de juiste ondersteunings methode op en geef uw contract informatie op.

    3. Klik op **volgende: controleren + maken** om de aanvraag te maken. 


## <a name="next-steps"></a>Volgende stappen  

- [Informatie over de opslaghiërarchie van Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
- [Kosten model voor Azure NetApp Files](azure-netapp-files-cost-model.md)
