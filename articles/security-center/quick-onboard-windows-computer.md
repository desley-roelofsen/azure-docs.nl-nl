---
title: Windows-computers onboarden naar Azure Security Center
description: In deze snelstart leert u hoe u Microsoft Monitoring Agent op een Windows-computer inricht.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: memildin
ms.openlocfilehash: 417d8379d019a9ef0da41638cba4a1f9cb7b8bc2
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686501"
---
# <a name="quickstart-onboard-windows-computers-to-azure-security-center"></a>Snelstart: Windows-computers onboarden naar Azure Security Center
Nadat u de onboarding van uw Azure-abonnementen hebt uitgevoerd, kunt u Security Center inschakelen voor resources die buiten Azure worden uitgevoerd, bijvoorbeeld on-premises of in andere clouds, door Microsoft Monitoring Agent in te richten.

In deze snelstart ziet u hoe u Microsoft Monitoring Agent op een Windows-computer installeert.

## <a name="prerequisites"></a>Vereisten
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

U moet u zich in de Standard-prijscategorie van Security Center bevinden voordat u aan deze snelstartgids kunt beginnen. Zie [Onboard your Azure subscription to Security Center Standard](security-center-get-started.md) (Uw Azure-abonnement registreren voor Security Center Standard) voor upgrade-instructies. U kunt Security Center Standard kosteloos uitproberen. Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.

## <a name="add-new-windows-computer"></a>Nieuwe Windows-computer toevoegen

1. Meld u aan bij de [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. Ga naar het **Microsoft Azure**-menu en selecteer **Security Center**. **Security Center - Overzicht** wordt geopend.

   ![Security Center-overzicht][2]

3. Selecteer in het hoofdmenu van Security Center de optie **Aan de slag**.
4. Selecteer het tabblad **Aan de slag**.

   ![Aan de slag][3]

5. Klik op **Configureren** onder **Nieuwe niet-Azure-computers toevoegen**. Een lijst met uw Log Analytics-werkruimten wordt weergegeven. De lijst bevat, indien van toepassing, de standaardwerkruimte die is gemaakt door Security Center toen automatisch inrichten werd ingeschakeld. Selecteer deze werkruimte of een andere werkruimte die u wilt gebruiken.

    ![Niet-Azure-computer toevoegen](./media/quick-onboard-windows-computer/non-azure.png)

   De blade **Agent toewijzen** wordt geopend, met een koppeling om een Windows-agent en sleutels voor uw werkruimte-id te downloaden, zodat u de agent kunt configureren.

6. Selecteer de koppeling **Windows-agent downloaden** die van toepassing is op het processortype van uw computer om het installatiebestand te downloaden.

7. Selecteer rechts van **Werkruimte-id** het kopieerpictogram en plak de id in Kladblok.

8. Selecteer rechts van **Primaire sleutel** het kopieerpictogram en plak de sleutel in Kladblok.

## <a name="install-the-agent"></a>De agent installeren
U dient nu het gedownloade bestand op de doelcomputer te installeren.

1. Kopieer het bestand naar de doelcomputer en voer setup uit.
2. Op de pagina **Welkom** selecteert u **Volgende**.
3. Lees de licentie op de pagina **Licentievoorwaarden** en selecteer **Akkoord**.
4. Op de pagina **Doelmap** wijzigt u desgewenst de standaardinstallatiemap en selecteert u **Volgende**.
5. Op de pagina **Installatieopties voor Agent** kiest u ervoor de agent verbinding te laten maken met Azure Log Analytics en selecteert u **Volgende**.
6. Op de pagina **Azure Log Analytics** plakt u de **werkruimte-id** en **werkruimtesleutel (primaire sleutel)** die u in de vorige stap in Kladblok hebt gekopieerd.
7. Als u de computer wilt laten rapporteren bij een Log Analytics-werkruimte in de Azure Government-cloud, selecteert u **Azure US Government** in de vervolgkeuzelijst **Azure Cloud**. Als de computer met de Log Analytics-service moet communiceren via een proxyserver, selecteert u **Geavanceerd** en geeft u de URL en het poortnummer van de proxyserver op.
8. Selecteer **Volgende** als u de vereiste configuratie-instellingen hebt voltooid.

   ![De agent installeren][5]

9. Controleer op de pagina **Gereed om te installeren** uw keuzes en selecteer **Installeren**.
10. Selecteer op de pagina **Configuratie voltooid** de optie **Voltooien**

Als u klaar bent wordt de **MMA** in het **Configuratiescherm** weergegeven. U kunt hier de configuratie controleren en verifiëren of de agent is verbonden.

Zie [Windows-computers verbinden](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard) voor meer informatie over het installeren en configureren van de agent.

U kunt nu uw Azure-VM's en niet-Azure-computers op één plek bewaken. Onder **Compute** vindt u een overzicht van alle VM's en computers, plus aanbevelingen. Elke kolom vertegenwoordigt een reeks aanbevelingen. De kleur vertegenwoordigt de huidige beveiligingsstatus van de VM's of computers voor die aanbeveling. Security Center geeft ook detecties voor deze computers in beveiligingswaarschuwingen weer.

  ![Blade Compute][6]

Er worden twee soorten pictogrammen weergegeven op de blade **Compute**:

![pictogram1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Niet-Azure-computer

![pictogram2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Azure VM

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer de agent niet langer nodig is, kunt u deze verwijderen van de Windows-computer.

De agent verwijderen:

1. Open het **Configuratiescherm**.
2. Open **Programma's en onderdelen**.
3. Selecteer in **Programma's en onderdelen** de optie **MMA** en klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen
In deze snelstart richt u Microsoft Monitoring Agent in op een Windows-computer. Voor meer informatie over het gebruik van Security Center gaat u verder met de zelfstudie voor het configureren van een beveiligingsbeleid en het beoordelen van de beveiliging van uw resources.

> [!div class="nextstepaction"]
> [Zelfstudie: Beveiligingsbeleidsregels opstellen en beoordelen](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/get-started.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png
