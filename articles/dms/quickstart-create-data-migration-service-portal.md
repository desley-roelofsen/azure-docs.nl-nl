---
title: 'Snelstartgids: een exemplaar maken met behulp van de Azure Portal'
titleSuffix: Azure Database Migration Service
description: Gebruik de Azure Portal om een exemplaar van Azure Database Migration Service te maken.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: quickstart
ms.date: 11/05/2019
ms.openlocfilehash: 71308b7f17d6009e8650cafbb02afd577fd73cf4
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75437736"
---
# <a name="quickstart-create-an-instance-of-the-azure-database-migration-service-by-using-the-azure-portal"></a>Snelstart: Een Azure Database Migration Service-exemplaar maken met behulp van de Azure-portal

In deze Quick Start gebruikt u de Azure Portal om een exemplaar van Azure Database Migration Service te maken.  Nadat u de service hebt gemaakt, kunt u deze gebruiken om gegevens van SQL Server on-premises te migreren naar Azure SQL Database.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Open de webbrowser, navigeer naar [Microsoft Azure Portal](https://portal.azure.com/), en voer vervolgens uw referenties in om u aan te melden bij de portal.

De standaardweergave is uw service-dashboard.

## <a name="register-the-resource-provider"></a>De resourceprovider registreren

Registreer de Microsoft.DataMigration-resourceprovider voordat u uw eerste Database Migration Service-exemplaar maakt.

1. Selecteer in Azure Portal de optie **Alle services** en vervolgens **Abonnementen**.

2. Selecteer het abonnement waarin u het exemplaar van Azure Database Migration Service wilt maken en selecteer vervolgens **resource providers**.

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media/quickstart-create-data-migration-service-portal/dms-register-provider.png)

## <a name="create-an-instance-of-the-service"></a>Een exemplaar van de service maken

1. Selecteer +**een resource maken** om een exemplaar van Azure database Migration service te maken.

2. Zoek in de marketplace naar 'migration', selecteer **Azure Database Migration Service** en selecteer vervolgens in het scherm **Azure Database Migration Service**  de optie **Maken**.

3. Ga in het scherm **Migratieservice maken** als volgt te werk:

    - Kies een **service naam** die u wilt onthouden en die uniek is om uw exemplaar van Azure database Migration service te identificeren.
    - Selecteer het Azure-**abonnement** waarin u het exemplaar wilt maken.
    - Selecteer een bestaande **Resourcegroep** of maak een nieuwe.
    - Kies de **locatie** die zich het dichtst bij uw bron- of doelserver bevindt.
    - Selecteer een bestaand **virtueel netwerk** (VNet) of maak een.

        Het VNet biedt Azure Database Migration Service toegang tot de bron database en de doel omgeving.

        Zie het artikel [een virtueel netwerk maken met behulp van de Azure Portal](https://aka.ms/vnet)voor meer informatie over het maken van een VNet in de Azure Portal.

    - Selecteer Basis: 1 vCore als **prijscategorie**.

        ![Migratieservice maken](media/quickstart-create-data-migration-service-portal/dms-create-service1.png)

4. Selecteer **Maken**.

    Na enkele ogen blikken is uw exemplaar van Azure data base Migration service gemaakt en klaar voor gebruik. Azure Database Migration Service wordt weer gegeven zoals in de volgende afbeelding:

    ![Migratieservice gemaakt](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Resources opschonen

Alle resources die u in deze QuickStart hebt gemaakt, kunt u verwijderen door de [Azure-resourcegroep](../azure-resource-manager/management/overview.md) te verwijderen. U verwijdert de resourcegroep door te navigeren naar het Azure Database Migration Service-exemplaar dat u hebt gemaakt. Selecteer bij **Resourcegroep** de naam van de resourcegroep en selecteer vervolgens **Resourcegroep verwijderen**. Door deze actie worden alle items in de resourcegroep verwijderd, evenals de groep zelf.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een on-premises SQL-server migreren naar Azure SQL Database](tutorial-sql-server-to-azure-sql.md)