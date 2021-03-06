---
title: Authentication
description: Meer informatie over het verifiëren van Azure SQL Data Warehouse met behulp van Azure Active Directory (AAD) of SQL Server verificatie.
services: sql-data-warehouse
author: julieMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/02/2019
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: fda29e432fbd952261893f3c32a4df7b9990ae66
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73692933"
---
# <a name="authenticate-to-azure-sql-data-warehouse"></a>Verifiëren bij Azure SQL Data Warehouse
Meer informatie over het verifiëren van Azure SQL Data Warehouse met behulp van Azure Active Directory (AAD) of SQL Server verificatie.

Als u verbinding wilt maken met SQL Data Warehouse, moet u beveiligings referenties door geven voor verificatie doeleinden. Bij het tot stand brengen van een verbinding worden bepaalde Verbindings instellingen geconfigureerd als onderdeel van het maken van de query sessie.  

Zie [een Data Base beveiligen in SQL Data Warehouse][Secure a database in SQL Data Warehouse]voor meer informatie over beveiliging en het inschakelen van verbindingen met uw data warehouse.

## <a name="sql-authentication"></a>SQL-verificatie
Als u verbinding wilt maken met SQL Data Warehouse, moet u de volgende informatie opgeven:

* Volledig gekwalificeerde servername
* SQL-verificatie opgeven
* Gebruikersnaam
* Wachtwoord
* Standaard database (optioneel)

Uw verbinding maakt standaard verbinding met de *hoofd* database en niet op uw gebruikers database. Als u verbinding wilt maken met uw gebruikers database, kunt u kiezen uit een van de volgende twee dingen:

* Geef de standaard database op wanneer u uw server registreert bij de SQL Server-objectverkenner in SSDT, SSMS of in uw toepassings connection string. Neem bijvoorbeeld de para meter InitialCatalog op voor een ODBC-verbinding.
* Markeer de gebruikers database voordat u een sessie maakt in SSDT.

> [!NOTE]
> De Transact-SQL-instructie **use MyDatabase;** wordt niet ondersteund voor het wijzigen van de Data Base voor een verbinding. Raadpleeg het artikel [query with Visual Studio][Query with Visual Studio] voor hulp bij het maken van verbinding met SQL data WAREHOUSE met SSDT.
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory-verificatie (AAD)
[Azure Active Directory][What is Azure Active Directory] -verificatie is een mechanisme om verbinding te maken met Microsoft Azure SQL data warehouse met behulp van identiteiten in azure Active Directory (Azure AD). Met Azure Active Directory-verificatie kunt u de identiteiten van database gebruikers en andere micro soft-services centraal beheren op één centrale locatie. Centraal-ID-beheer biedt één locatie voor het beheren van SQL Data Warehouse gebruikers en het vereenvoudigt het beheer van machtigingen. 

### <a name="benefits"></a>Voordelen
Azure Active Directory voor delen zijn onder andere:

* Biedt een alternatief voor SQL Server-verificatie.
* Helpt de verspreiding van gebruikers identiteiten over database servers te stoppen.
* Maakt rotatie van wachtwoorden op één plek mogelijk
* Beheer database machtigingen met externe groepen (AAD).
* Elimineert het opslaan van wacht woorden door geïntegreerde Windows-authenticatie en andere vormen van verificatie die door Azure Active Directory worden ondersteund, in te scha kelen.
* Gebruikt Inge sloten database gebruikers voor het verifiëren van identiteiten op database niveau.
* Biedt ondersteuning voor verificatie op basis van tokens voor toepassingen die verbinding maken met SQL Data Warehouse.
* Ondersteunt multi-factor Authentication via Active Directory universele verificatie voor verschillende hulpprogram ma's, waaronder [SQL Server Management Studio](../sql-database/sql-database-ssms-mfa-authentication.md) en [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/azure-active-directory?toc=/azure/sql-data-warehouse/toc.json).

> [!NOTE]
> Azure Active Directory nog steeds relatief nieuw is en enkele beperkingen heeft. Zie [Azure AD-functies en-beperkingen][Azure AD features and limitations], met name de aanvullende overwegingen, om ervoor te zorgen dat Azure Active Directory geschikt is voor uw omgeving.
> 
> 

### <a name="configuration-steps"></a>Configuratiestappen
Volg deze stappen om Azure Active Directory-verificatie te configureren.

1. Een Azure Active Directory maken en vullen
2. Optioneel: de Active Directory die momenteel aan uw Azure-abonnement is gekoppeld, koppelen of wijzigen
3. Een Azure Active Directory beheerder maken voor Azure SQL Data Warehouse.
4. Uw client computers configureren
5. Inge sloten database gebruikers in uw data base maken die zijn toegewezen aan Azure AD-identiteiten
6. Verbinding maken met uw data warehouse met behulp van Azure AD-identiteiten

Momenteel worden gebruikers Azure Active Directory niet weer gegeven in SSDT Objectverkenner. Als tijdelijke oplossing kunt u de gebruikers weer geven in [sys. database_principals](https://msdn.microsoft.com/library/ms187328.aspx).

### <a name="find-the-details"></a>De details zoeken
* De stappen voor het configureren en gebruiken van Azure Active Directory verificatie zijn bijna identiek voor Azure SQL Database en Azure SQL Data Warehouse. Volg de gedetailleerde stappen in het onderwerp [verbinding maken met SQL database of SQL data warehouse met behulp van Azure Active Directory-verificatie](../sql-database/sql-database-aad-authentication.md).
* Aangepaste database rollen maken en gebruikers toevoegen aan de rollen. Ken vervolgens gedetailleerde machtigingen toe aan de rollen. Zie aan de slag met de machtigingen voor de [Data base-engine](https://msdn.microsoft.com/library/mt667986.aspx)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
Zie [Query’s uitvoeren bij Visual Studio][Query with Visual Studio] als u wilt beginnen met het uitvoeren van query’s bij uw datawarehouse met Visual Studio en andere toepassingen.

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]:../active-directory/fundamentals/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
