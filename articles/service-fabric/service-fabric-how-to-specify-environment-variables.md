---
title: Omgevings variabelen voor services opgeven
description: Laat zien hoe u omgevings variabelen gebruikt voor toepassingen in Service Fabric
author: mikkelhegn
ms.topic: conceptual
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: f4c4f2a1c140e3d0f181c4fd55482056f9f91b62
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614312"
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Omgevings variabelen opgeven voor services in Service Fabric

In dit artikel leest u hoe u omgevings variabelen kunt opgeven voor een service of container in Service Fabric.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Procedure voor het opgeven van omgevings variabelen voor services

In dit voor beeld stelt u een omgevings variabele in voor een container. In dit artikel wordt ervan uitgegaan dat u al een toepassings-en service manifest hebt.

1. Open het bestand ServiceManifest. XML.
2. Voeg in het element `CodePackage` een nieuw `EnvironmentVariables` element en een `EnvironmentVariable`-element toe voor elke omgevings variabele.

    ```xml
    <CodePackage Name="MyCode" Version="CodeVersion1">
            ...
            <EnvironmentVariables>
                  <EnvironmentVariable Name="MyEnvVariable" Value="DefaultValue"/>
                  <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentVariables>
    </CodePackage>
    ```

   Omgevings variabelen kunnen worden overschreven in het toepassings manifest.

3. Als u de omgevings variabelen in het manifest van de toepassing wilt overschrijven, gebruikt u het element `EnvironmentOverrides`.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="specifying-environment-variables-dynamically-using-docker-compose"></a>Omgevings variabelen dynamisch opgeven met docker opstellen

Service Fabric ondersteunt de mogelijkheid om [docker samen te gebruiken voor implementatie](service-fabric-docker-compose.md#supported-compose-directives). Het opstellen van bestanden kan variabelen van de bron omgeving van de shell zijn. Dit gedrag kan worden gebruikt om de gewenste omgevings waarden dynamisch te vervangen:

```yml
environment:
  - "hostname:${hostname}"
```

## <a name="next-steps"></a>Volgende stappen
Zie de [artikelen toepassingen beheren voor meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md)voor meer informatie over een aantal van de belangrijkste concepten die in dit artikel worden besproken.

Zie [uw service Fabric-toepassingen beheren in Visual Studio](service-fabric-manage-application-in-visual-studio.md)voor informatie over andere mogelijkheden voor het beheren van apps die beschikbaar zijn in Visual Studio.
