---
title: Controle van beheerde exemplaren
description: Meer informatie over hoe u aan de slag gaat met Azure SQL Database Managed instance auditing met behulp van T-SQL
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
f1_keywords:
- mi.azure.sqlaudit.general.f1
author: barmichal
ms.author: mibar
ms.reviewer: vanto
ms.date: 04/08/2019
ms.openlocfilehash: 41d632b145a7187dd0aeaab740cd8546a3955e7f
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818996"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Aan de slag met Azure SQL Database Managed instance auditing

Met controle van [beheerde exemplaren](sql-database-managed-instance.md) worden database gebeurtenissen bijgehouden en naar een audit logboek in uw Azure Storage-account geschreven. Controleren is ook:

- Helpt u bij het onderhouden van naleving van regelgeving, het begrijpen van database activiteiten en inzicht te krijgen in verschillen en afwijkingen die kunnen wijzen op problemen met het bedrijf of vermoedelijke beveiligings schendingen.
- Maakt en vergemakkelijkt het naleven van nalevings standaarden, hoewel dit geen garantie biedt voor naleving. Voor meer informatie over Azure-Program ma's die naleving van standaarden ondersteunen, raadpleegt u de [Vertrouwenscentrum van Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) waarin u de meest recente lijst met SQL database nalevings certificeringen kunt vinden.

## <a name="set-up-auditing-for-your-server-to-azure-storage"></a>Controle instellen voor uw server naar Azure Storage

In de volgende sectie wordt de configuratie van de controle op uw beheerde exemplaar beschreven.

1. Ga naar de [Azure Portal](https://portal.azure.com).
1. Een Azure Storage- **container** maken waar audit logboeken worden opgeslagen.

   1. Ga naar het Azure Storage waar u de audit logboeken wilt opslaan.

      > [!IMPORTANT]
      > Gebruik een opslag account in dezelfde regio als het beheerde exemplaar om Lees-en schrijf bewerkingen in meerdere regio's te voor komen.

   1. Ga in het opslag account naar **overzicht** en klik op **blobs**.

      ![Azure Blob-widget](./media/sql-managed-instance-auditing/1_blobs_widget.png)

   1. Klik in het bovenste menu op **+ container** om een nieuwe container te maken.

      ![Pictogram van een BLOB-container maken](./media/sql-managed-instance-auditing/2_create_container_button.png)

   1. Geef een container **naam**op, stel openbaar toegangs niveau in op **privé**en klik vervolgens op **OK**.

      ![Configuratie van BLOB-container maken](./media/sql-managed-instance-auditing/3_create_container_config.png)

1. Nadat de container voor de audit Logboeken is gemaakt, zijn er twee manieren om deze te configureren als het doel voor de audit logboeken: het [gebruik van T-SQL](#blobtsql) of [de gebruikers interface van de SQL Server Management Studio (SSMS)](#blobssms):

   - <a id="blobtsql"></a>Blog opslag configureren voor audit logboeken met T-SQL:

     1. Klik in de lijst containers op de zojuist gemaakte container en klik vervolgens op **container eigenschappen**.

        ![Knop Eigenschappen van BLOB-container](./media/sql-managed-instance-auditing/4_container_properties_button.png)

     1. Kopieer de URL van de container door te klikken op het Kopieer pictogram en de URL op te slaan (bijvoorbeeld in Klad blok) voor toekomstig gebruik. De indeling van de container-URL moet worden `https://<StorageName>.blob.core.windows.net/<ContainerName>`

        ![Kopie-URL van BLOB-container](./media/sql-managed-instance-auditing/5_container_copy_name.png)

     1. Genereer een Azure Storage **SAS-token** om toegangs rechten voor het beheerde exemplaar te verlenen aan het opslag account:

        - Navigeer naar het Azure Storage-account waarin u de container hebt gemaakt in de vorige stap.

        - Klik op de **hand tekening voor gedeelde toegang** in het menu opslag instellingen.

          ![Pictogram voor de hand tekening voor gedeelde toegang in het menu opslag instellingen](./media/sql-managed-instance-auditing/6_storage_settings_menu.png)

        - Configureer de SAS als volgt:

          - **Toegestane Services**: BLOB

          - **Begin datum**: om te voor komen dat er problemen met de tijd zone te maken, kunt u het beste de datum van gisteren gebruiken

          - **Eind datum**: Kies de datum waarop deze SAS-token verloopt

            > [!NOTE]
            > Vernieuw het token na verloop om mislukte audits te voor komen.

          - Klik op **SAS genereren**.
            
            ![SAS-configuratie](./media/sql-managed-instance-auditing/7_sas_configure.png)

        - Nadat u op SAS genereren hebt geklikt, wordt de SAS-token onderaan weer gegeven. Kopieer het token door te klikken op het Kopieer pictogram en sla het op (bijvoorbeeld in Klad blok) voor toekomstig gebruik.

          ![SAS-token kopiëren](./media/sql-managed-instance-auditing/8_sas_copy.png)

          > [!IMPORTANT]
          > Verwijder het vraag teken ('? ') vanaf het begin van het token.

     1. Maak verbinding met uw beheerde exemplaar via SQL Server Management Studio (SSMS) of een ander ondersteund hulp programma.

     1. Voer de volgende T-SQL-instructie uit om **een nieuwe referentie te maken** met behulp van de container-URL en het SAS-token dat u in de vorige stappen hebt gemaakt:

        ```SQL
        CREATE CREDENTIAL [<container_url>]
        WITH IDENTITY='SHARED ACCESS SIGNATURE',
        SECRET = '<SAS KEY>'
        GO
        ```

     1. Voer de volgende T-SQL-instructie uit om een nieuwe server controle te maken (Kies uw eigen audit naam, gebruik de container-URL die u in de vorige stappen hebt gemaakt). Als deze niet wordt opgegeven, is `RETENTION_DAYS` standaard 0 (onbeperkte retentie):

        ```SQL
        CREATE SERVER AUDIT [<your_audit_name>]
        TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
        GO
        ```

        1. Ga door met het [maken van een server audit specificatie of specificatie van de database audit](#createspec)

   - <a id="blobssms"></a>Blob-opslag configureren voor audit logboeken met behulp van de SQL Server Management Studio (SSMS) 18 (preview):

     1. Maak verbinding met het beheerde exemplaar met behulp van de gebruikers interface van SQL Server Management Studio (SSMS).

     1. Vouw de hoofd notitie van de Objectverkenner uit.

     1. Vouw het **beveiligings** knooppunt uit, klik met de rechter muisknop op het knoop punt **controles** en klik op nieuwe controle:

        ![Beveiligings-en controle knooppunt uitvouwen](./media/sql-managed-instance-auditing/10_mi_SSMS_new_audit.png)

     1. Zorg ervoor dat "URL" is geselecteerd in **controle doel** en klik op **Bladeren**:

        ![Azure Storage bladeren](./media/sql-managed-instance-auditing/11_mi_SSMS_audit_browse.png)

     1. Beschrijving Meld u aan bij uw Azure-account:

        ![Aanmelden bij Azure](./media/sql-managed-instance-auditing/12_mi_SSMS_sign_in_to_azure.png)

     1. Selecteer een abonnement, opslag account en BLOB-container in de vervolg keuzelijsten of maak uw eigen container door te klikken op **maken**. Klik op **OK**als u klaar bent:

        ![Azure-abonnement, opslag account en BLOB-container selecteren](./media/sql-managed-instance-auditing/13_mi_SSMS_select_subscription_account_container.png)

     1. Klik op **OK** in het dialoog venster controle maken.

1. <a id="createspec"></a>Nadat u de BLOB-container als doel voor de audit Logboeken hebt geconfigureerd, maakt u een specificatie voor de server audit of een database audit, zoals u zou doen voor SQL Server:

   - [T-SQL-hand leiding voor Server audit Specification maken](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [T-SQL-hand leiding voor database audit specificatie maken](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

1. Schakel de server controle in die u in stap 6 hebt gemaakt:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

Meer informatie:

- [Controle verschillen tussen afzonderlijke data bases, elastische Pools en beheerde exemplaren in Azure SQL Database en data bases in SQL Server](#auditing-differences-between-databases-in-azure-sql-database-and-databases-in-sql-server)
- [SERVER CONTROLE MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
- [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

## <a name="set-up-auditing-for-your-server-to-event-hub-or-azure-monitor-logs"></a>Auditing voor uw server instellen op Event hub-of Azure Monitor-logboeken

Audit logboeken van een beheerd exemplaar kunnen worden verzonden naar zelfs hubs of Azure Monitor Logboeken. In deze sectie wordt beschreven hoe u dit kunt configureren:

1. Navigeer in [Azure Portal](https://portal.azure.com/) naar het beheerde exemplaar.

2. Klik op **Diagnostische instellingen**.

3. Klik op **Diagnostische gegevens inschakelen**. Als de diagnostische gegevens al is ingeschakeld, wordt in plaats daarvan de *instelling diagnostische gegevens toevoegen* weer gegeven.

4. Selecteer **SQLSecurityAuditEvents** in de lijst met Logboeken.

5. Selecteer een doel voor de controle gebeurtenissen-Event hub, Azure Monitor Logboeken of beide. Configureer voor elk doel de vereiste para meters (bijvoorbeeld Log Analytics-werk ruimte).

6. Klik op **Opslaan**.

    ![Diagnostische instellingen configureren](./media/sql-managed-instance-auditing/9_mi_configure_diagnostics.png)

7. Maak verbinding met het beheerde exemplaar met behulp van **SQL Server Management Studio (SSMS)** of een andere ondersteunde client.

8. Voer de volgende T-SQL-instructie uit om een server controle te maken:

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>] TO EXTERNAL_MONITOR;
    GO
    ```

9. Maak een server audit specificatie of specificatie van de database audit zoals u zou doen voor SQL Server:

   - [T-SQL-hand leiding voor Server audit Specification maken](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [T-SQL-hand leiding voor database audit specificatie maken](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

10. Schakel de server controle in die is gemaakt in stap 8:
 
    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>] WITH (STATE=ON);
    GO
    ```

## <a name="consume-audit-logs"></a>Controle Logboeken gebruiken

### <a name="consume-logs-stored-in-azure-storage"></a>Logboeken gebruiken die zijn opgeslagen in Azure Storage

Er zijn verschillende methoden die u kunt gebruiken om de controle logboeken van blobs weer te geven.

- Gebruik de systeem functie `sys.fn_get_audit_file` (T-SQL) om de controle logboek gegevens in tabel vorm te retour neren. Zie de [documentatie voor sys. fn_get_audit_file](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql)voor meer informatie over het gebruik van deze functie.

- U kunt audit logboeken verkennen met behulp van een hulp programma zoals [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/). In azure Storage worden controle Logboeken opgeslagen als een verzameling BLOB-bestanden binnen een container die is gedefinieerd voor het opslaan van de audit Logboeken. Zie voor meer informatie over de hiërarchie van de opslagmap, de naam conventies en de logboek indeling de [verwijzing naar de indeling van het BLOB-controle logboek](https://go.microsoft.com/fwlink/?linkid=829599).

- Raadpleeg de controle aan de [slag met SQL database](sql-database-auditing.md)voor een volledige lijst met verbruiks methoden voor controle Logboeken.

### <a name="consume-logs-stored-in-event-hub"></a>Logboeken gebruiken die zijn opgeslagen in Event hub

Als u gegevens van de audit logboeken van Event hub wilt gebruiken, moet u een stroom instellen om gebeurtenissen te gebruiken en deze naar een doel te schrijven. Zie de documentatie van Azure Event Hubs voor meer informatie.

### <a name="consume-and-analyze-logs-stored-in-azure-monitor-logs"></a>Logboeken die zijn opgeslagen in Azure Monitor Logboeken gebruiken en analyseren

Als audit logboeken naar Azure Monitor logboeken worden geschreven, zijn ze beschikbaar in de Log Analytics-werk ruimte, waar u geavanceerde zoek opdrachten kunt uitvoeren op de controle gegevens. Ga als uitgangs punt naar de Log Analytics-werk ruimte en klik in de sectie *Algemeen* op *Logboeken* en voer een eenvoudige query in, zoals: `search "SQLSecurityAuditEvents"` om de audit logboeken weer te geven.  

Met Azure Monitor-Logboeken kunt u in realtime operationeel inzicht krijgen met behulp van geïntegreerde Zoek-en aangepaste Dash boards waarmee u miljoenen records in al uw workloads en servers eenvoudig kunt analyseren. Zie voor aanvullende nuttige informatie over Azure Monitor Zoek taal en-opdrachten in Logboeken [Azure monitor logboeken zoeken](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="auditing-differences-between-databases-in-azure-sql-database-and-databases-in-sql-server"></a>Controleren van verschillen tussen data bases in Azure SQL Database en data bases in SQL Server

De belangrijkste verschillen tussen controles in data bases in Azure SQL Database en data bases in SQL Server zijn:

- Met de implementatie optie Managed instance in Azure SQL Database, werkt auditing op server niveau en worden `.xel` logboek bestanden opgeslagen in Azure Blob-opslag.
- Bij SQL Server on-premises/virtuele machines werkt audit op het niveau van de server, maar worden gebeurtenissen opgeslagen in gebeurtenis logboeken van het bestand systeem/Windows.

XEvent-controle in een beheerd exemplaar ondersteunt Azure Blob-opslag doelen. Bestands-en Windows-logboeken worden **niet ondersteund**.

De belangrijkste verschillen in de `CREATE AUDIT` syntaxis voor de controle van Azure Blob-opslag zijn:

- Er wordt een nieuwe syntaxis `TO URL` opgegeven, waarmee u de URL kunt opgeven van de Azure Blob Storage-container waar de `.xel` bestanden worden geplaatst.
- Er wordt een nieuwe syntaxis `TO EXTERNAL MONITOR` gegeven om zelfs hub-en Azure Monitor-logboeken te kunnen doelen.
- De syntaxis `TO FILE` wordt **niet ondersteund** omdat SQL database geen toegang krijgt tot Windows-bestands shares.
- De optie shutdown wordt **niet ondersteund**.
- `queue_delay` van 0 wordt **niet ondersteund**.

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg de controle aan de [slag met SQL database](sql-database-auditing.md)voor een volledige lijst met verbruiks methoden voor controle Logboeken.
- Voor meer informatie over Azure-Program ma's die naleving van standaarden ondersteunen, raadpleegt u de [Vertrouwenscentrum van Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) waarin u de meest recente lijst met SQL database nalevings certificeringen kunt vinden.

<!--Image references-->









