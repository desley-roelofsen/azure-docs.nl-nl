---
title: De gegevens op het hoogste niveau van uw Azure-abonnement exporteren | Microsoft Docs
description: Hierin wordt beschreven hoe u alle Azure-abonnements-id's kunt weergeven die zijn gekoppeld aan uw account.
keywords: ''
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: banders
ms.openlocfilehash: 87c31e663529ea20d119753094133941b02e2756
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74226267"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Uw abonnementsgegevens op het hoogste niveau exporteren en weergeven
Als u de set met abonnements-id's wilt weergeven die zijn gekoppeld aan uw gebruikersreferenties, [downloadt u een .json-bestand met uw abonnementsgegevens in het Azure-accountcentrum](https://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Het gedownloade .json-bestand bevat de volgende informatie:
- E-mail: Het e-mail adres dat is gekoppeld aan uw account.
- PUID: De unieke id die is gekoppeld aan uw factureringsaccount.
- Abonnements-id’s: Een lijst met abonnementen die deel uitmaken van uw account, geïnventariseerd op abonnements-id.

### <a name="subscriptionsjson-sample"></a>voorbeeld abonnementen.json

```json
{
  "Email":"admin@contoso.com",
  "Puid":"00052xxxxxxxxxxx",
  "SubscriptionIds":[
    "38124d4d-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "7c8308f1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "39a25f2b-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "52ec2489-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "e42384b2-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "90757cdc-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  ]
}
```
