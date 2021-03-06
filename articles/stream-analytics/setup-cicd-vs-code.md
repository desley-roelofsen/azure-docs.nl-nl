---
title: Het NPM-pakket voor de Azure Stream Analytics CI/CD gebruiken
description: In dit artikel wordt beschreven hoe u Azure Stream Analytics CI/CD NPM-pakket gebruikt om een doorlopend integratie-en implementatie proces in te stellen.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: df9afaaeeb7e41c111fe6bd053047095a9cb9349
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2019
ms.locfileid: "70173351"
---
# <a name="use-the-stream-analytics-cicd-npm-package"></a>Het NPM-pakket voor de Stream Analytics CI/CD gebruiken
In dit artikel wordt beschreven hoe u het Azure Stream Analytics CI/CD NPM-pakket gebruikt om een doorlopend integratie-en implementatie proces in te stellen.

## <a name="build-the-vs-code-project"></a>Het VS code-project bouwen

U kunt continue integratie en implementatie voor Azure Stream Analytics-taken inschakelen met het NPM **-pakket ASA-streamanalytics-cicd** . Het NPM-pakket bevat de hulpprogram ma's voor het genereren van Azure Resource Manager sjablonen van [Stream Analytics Visual Studio-code projecten](quick-create-vs-code.md). Het kan worden gebruikt in Windows, macOS en Linux zonder Visual Studio code te installeren.

Nadat u [het pakket hebt gedownload](https://www.npmjs.com/package/azure-streamanalytics-cicd), gebruikt u de volgende opdracht om de Azure Resource Manager-sjablonen uit te voeren. Het argument **ScriptPath** is het absolute pad naar het **asaql** -bestand in uw project. Zorg ervoor dat de bestanden asaproj. json en JobConfig. json zich in dezelfde map bevinden als het script bestand. Als de **outputPath** niet is opgegeven, worden de sjablonen geplaatst in de map **Deploy** onder de **bin** -map van het project.

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
Voor beeld (op macOS)
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

Wanneer een Stream Analytics Visual Studio code-project is gebouwd, worden de volgende twee Azure Resource Manager sjabloon bestanden gegenereerd in de map **bin/[debug/Retail]/Deploy** : 

*  Resource Manager-sjabloon bestand

       [ProjectName].JobTemplate.json 

*  Resource Manager-parameter bestand

       [ProjectName].JobTemplate.parameters.json   

De standaard parameters in het bestand para meters. json zijn afkomstig uit de instellingen in uw Visual Studio code-project. Als u wilt implementeren in een andere omgeving, vervangt u de para meters dienovereenkomstig.

> [!NOTE]
> Voor alle referenties worden de standaard waarden ingesteld op null. U **moet** de waarden instellen voordat u naar de Cloud implementeert.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Meer informatie over hoe u kunt [implementeren met een resource manager-sjabloon bestand en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Meer informatie over het [gebruik van een object als een para meter in een resource manager-sjabloon](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

Als u beheerde identiteit voor Azure Data Lake Store gen1 als uitvoer Sink wilt gebruiken, moet u toegang geven tot de service-principal met behulp van Power shell voordat u implementeert in Azure. Meer informatie over het [implementeren van ADLS gen1 met beheerde identiteit met een resource manager-sjabloon](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).
## <a name="next-steps"></a>Volgende stappen

* [Snelstart: Een Azure Stream Analytics Cloud taak maken in Visual Studio code (preview)](quick-create-vs-code.md)
* [Stream Analytics query's lokaal testen met Visual Studio code (preview)](vscode-local-run.md)
* [Azure Stream Analytics verkennen met Visual Studio code (preview)](vscode-explore-jobs.md)
