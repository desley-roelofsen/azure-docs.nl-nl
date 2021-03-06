---
title: Taken voor het project leiden in Team Data Science Process
description: Een gedetailleerd overzicht van de taken voor een project leider op een team data Science process team
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 09/24/2019
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 8a94a2ae5298bbee8bb1c9c0fa044eb3189147be
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244389"
---
# <a name="project-lead-tasks-in-the-team-data-science-process"></a>Project Lead-taken in het team data Science process

In dit artikel worden taken beschreven die een *Project Lead* heeft voltooid voor het instellen van een opslag plaats voor hun project team in het [team data Science process](overview.md) (TDSP). De TDSP is een framework dat is ontwikkeld door micro soft en een gestructureerde reeks activiteiten biedt om op een efficiënte manier cloud-gebaseerde predictive analytics oplossingen uit te voeren. De TDSP is ontworpen om samen werking en team educatie te verbeteren. Voor een overzicht van de personeels rollen en de bijbehorende taken voor een Data Science-team dat wordt gestandardization op de TDSP, raadpleegt u [rollen en taken voor team data Science process](roles-tasks.md).

Een project lead beheert de dagelijkse activiteiten van afzonderlijke gegevens wetenschappers over een specifiek data Science-project in de TDSP. In het volgende diagram ziet u de werk stroom voor Project Lead-taken:

![Taak werk stroom project leider](./media/project-lead-tasks/project-leads-1-tdsp-creating-projects.png)

In deze zelf studie wordt stap 1 beschreven: project opslagplaats maken en stap 2: een Seed-project opslagplaats uit de ProjectTemplate-opslag plaats van uw team. 

Voor stap 3: functie werk item voor project maken en stap 4: verhalen toevoegen voor project fasen, Zie [flexibele ontwikkeling van data Science-projecten](agile-development.md).

Zie voor stap 5: opslag/analyse-assets maken en aanpassen en delen, indien nodig, [team gegevens en analyse resources maken](team-lead-tasks.md#create-team-data-and-analytics-resources).

Zie [team leden toevoegen en machtigingen configureren](team-lead-tasks.md#add-team-members-and-configure-permissions)voor stap 6: beveiligings beheer van de project opslagplaats instellen.

> [!NOTE] 
> In dit artikel wordt gebruikgemaakt van Azure opslag plaatsen voor het instellen van een TDSP-project, omdat dat gaat om TDSP te implementeren bij micro soft. Als uw team gebruikmaakt van een ander platform voor het hosten van een code, zijn de taken van de Project Lead hetzelfde, maar de manier om ze te volt ooien, kan afwijken.

## <a name="prerequisites"></a>Vereisten

In deze zelf studie wordt ervan uitgegaan dat uw [groeps Manager](group-manager-tasks.md) en [team leider](team-lead-tasks.md) de volgende resources en machtigingen hebben ingesteld:

- De Azure DevOps **-organisatie** voor uw gegevens eenheid
- Een team **project** voor uw data Science-Team
- **Opslag** plaatsen voor team sjablonen en-hulpprogram ma's
- **Machtigingen** voor uw organisatie account voor het maken en bewerken van opslag plaatsen voor uw project

Als u opslag plaatsen wilt klonen en inhoud wilt wijzigen op uw lokale machine of Data Science Virtual Machine (DSVM), of Azure File Storage wilt instellen en koppelen aan uw DSVM, hebt u ook het volgende nodig:

- Een Azure-abonnement.
- Git geïnstalleerd op de computer. Als u een DSVM gebruikt, is Git vooraf geïnstalleerd. Raadpleeg anders de [platformen en hulpprogramma's voor bijlage](platforms-and-tools.md#appendix).
- Als u een DSVM wilt gebruiken, wordt de Windows-of Linux-DSVM gemaakt en geconfigureerd in Azure. Zie de [Data Science virtual machine-documentatie](/azure/machine-learning/data-science-virtual-machine/)voor meer informatie en instructies.
- Voor een Windows DSVM is [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) op uw computer geïnstalleerd. Schuif in het bestand *README.MD* omlaag naar de sectie **downloaden en installeren** en selecteer het **nieuwste installatie programma**. Down load het installatie programma *. exe* op de pagina installatie programma en voer het uit. 
- Voor een Linux-DSVM is een open bare SSH-sleutel ingesteld op uw DSVM en toegevoegd aan Azure DevOps. Zie de sectie **open bare SSH-sleutel maken** in de [bijlage platformen en hulpprogram ma's](platforms-and-tools.md#appendix)voor meer informatie en instructies. 

## <a name="create-a-project-repository-in-your-team-project"></a>Een project opslagplaats maken in uw team project

Een project opslagplaats maken in het **MyTeam** -project van uw team:

1. Ga naar de project **samenvattings** pagina van uw team op *https:\//\<server naam >/\<organisatie naam >/\<team naam >* , bijvoorbeeld **https:\//dev.Azure.com/DataScienceUnit/MyTeam**, en selecteer **opslag plaatsen** in de linkernavigatiebalk. 
   
1. Selecteer de naam van de opslag plaats boven aan de pagina en selecteer vervolgens **nieuwe opslag plaats** in de vervolg keuzelijst.
   
   ![Nieuwe opslag plaats selecteren](./media/project-lead-tasks/project-leads-9-select-repos.png)
   
1. Zorg ervoor dat in het dialoog venster **een nieuwe opslag plaats maken** de tekst **Git** is geselecteerd onder **type**. Voer *DSProject1* in bij **naam van opslag plaats**en selecteer vervolgens **maken**.
   
   ![Opslag plaats maken](./media/project-lead-tasks/project-leads-3-create-project-repo-2.png)
   
1. Controleer of u de nieuwe **DSProject1** -opslag plaats kunt zien op de pagina project instellingen. 
   
   ![Project opslagplaats in Project instellingen](./media/project-lead-tasks/project-leads-4-create-project-repo-3.png)

## <a name="import-the-team-template-into-your-project-repository"></a>De team sjabloon importeren in uw project opslagplaats

Uw project opslagplaats vullen met de inhoud van de opslag plaats van uw team sjabloon:

1. Selecteer op de pagina project **overzicht** van uw team **opslag plaatsen** in het navigatie venster aan de linkerkant. 
   
1. Selecteer de naam van de opslag plaats boven aan de pagina en selecteer **DSProject1** in de vervolg keuzelijst.
   
1. Selecteer **importeren**op de pagina **DSProject1 is leeg** . 
   
   ![Import selecteren](./media/project-lead-tasks/project-leads-5-create-project-repo-4.png)
   
1. Selecteer **Git** als **bron type**in het dialoog venster **een Git-opslag plaats importeren** en voer de URL voor uw **TeamTemplate** -opslag plaats in onder **kloon-URL**. De URL is *https:\//\<server naam >/\<organisatie naam >/\<team naam >/_git/\<naam van de opslag plaats van de team sjabloon >* . Bijvoorbeeld: **https:\//dev.Azure.com/DataScienceUnit/MyTeam/_git/TeamTemplate**. 
   
1. Selecteer **importeren**. De inhoud van de opslag plaats van uw team sjabloon wordt in uw project opslagplaats geïmporteerd. 
   
   ![Opslag plaats voor team sjabloon importeren](./media/project-lead-tasks/project-leads-6-create-project-repo-5.png)

Als u de inhoud van de project opslagplaats moet aanpassen om te voldoen aan de specifieke behoeften van uw project, kunt u opslag plaats bestanden en mappen toevoegen, verwijderen of wijzigen. U kunt rechtstreeks in azure opslag plaatsen werken of de opslag plaats klonen op uw lokale computer of DSVM, wijzigingen aanbrengen en uw updates door voeren en pushen naar de gedeelde project opslagplaats. Volg de instructies bij het [aanpassen van de inhoud van de team opslagplaatsen](team-lead-tasks.md#customize-the-contents-of-the-team-repositories).

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar gedetailleerde beschrijvingen van de andere rollen en taken die worden gedefinieerd door het team data Science process:

- [Groepsbeheerder taken voor een team van gegevenswetenschappers](group-manager-tasks.md)
- [Team Lead taken voor een team van gegevenswetenschappers](team-lead-tasks.md)
- [Individuele Inzender taken voor een Data Science-Team](project-ic-tasks.md)
