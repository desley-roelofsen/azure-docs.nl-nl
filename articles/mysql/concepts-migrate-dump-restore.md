---
title: Migreren met dump en Restore-Azure Database for MySQL
description: In dit artikel worden twee algemene manieren uitgelegd voor het maken van back-ups en het herstellen van data bases in uw Azure Database for MySQL, met behulp van hulpprogram ma's zoals mysqldump, MySQL Workbench en PHPMyAdmin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 65cd5e637434c717ab9ba1b5598c467eea9b4a74
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770931"
---
# <a name="migrate-your-mysql-database-to-azure-database-for-mysql-using-dump-and-restore"></a>Uw MySQL-data base migreren naar Azure Database for MySQL met dump en herstel
In dit artikel worden twee algemene manieren uitgelegd voor het maken van back-ups en het herstellen van data bases in uw Azure Database for MySQL
- Dump en herstel vanaf de opdracht regel (met behulp van mysqldump) 
- Dump en herstel met behulp van PHPMyAdmin 

## <a name="before-you-begin"></a>Voordat u begint
Als u deze hand leiding wilt door lopen, hebt u het volgende nodig:
- [Azure Database for MySQL-server maken-Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) -opdracht regel programma dat op een computer is geïnstalleerd.
- MySQL Workbench [MySQL Workbench down load](https://dev.mysql.com/downloads/workbench/), Toad, Navicat of een ander mysql-hulp programma van derden om dump-en herstel opdrachten uit te voeren.

## <a name="use-common-tools"></a>Algemene hulpprogram ma's gebruiken
Gebruik algemene hulpprogram ma's, zoals MySQL Workbench, mysqldump, Toad of Navicat, om gegevens op afstand te verbinden en te herstellen in Azure Database for MySQL. Gebruik deze hulpprogram ma's op de client computer met een Internet verbinding om verbinding te maken met de Azure Database for MySQL. Gebruik een met SSL versleutelde verbinding voor best practices voor beveiliging, Zie [SSL-connectiviteit ook configureren in azure database for MySQL](concepts-ssl-connection-security.md). U hoeft de dump bestanden niet te verplaatsen naar een speciale Cloud locatie wanneer u migreert naar Azure Database for MySQL. 

## <a name="common-uses-for-dump-and-restore"></a>Veelvoorkomende toepassingen voor dump en herstel
U kunt MySQL-hulpprogram ma's, zoals mysqldump en mysqlpump, gebruiken om data bases te dumpen en te laden in een Azure MySQL-data base in verschillende algemene scenario's. In andere scenario's kunt u in plaats daarvan de benadering voor [importeren en exporteren](concepts-migrate-import-export.md) gebruiken.

- Gebruik database dumps wanneer u de gehele data base migreert. Deze aanbeveling houdt in dat u een grote hoeveelheid MySQL-gegevens verplaatst of wanneer u de service onderbreking voor Live sites of toepassingen wilt minimaliseren. 
-  Zorg ervoor dat alle tabellen in de database gebruikmaken van de InnoDB-opslag-engine bij het laden van gegevens in Azure Database for MySQL. Azure Database for MySQL ondersteunt alleen de InnoDB-opslag engine en biedt daarom geen ondersteuning voor alternatieve opslag engines. Als uw tabellen zijn geconfigureerd met andere opslag engines, converteert u deze naar de InnoDB-engine-indeling voordat u de migratie naar Azure Database for MySQL.
   Als u bijvoorbeeld een WordPress of WebApp met behulp van de MyISAM-tabellen hebt, moet u deze tabellen eerst converteren door de migratie naar de InnoDB-indeling voordat u naar Azure Database for MySQL herstelt. Gebruik de-component `ENGINE=InnoDB` om de engine in te stellen die wordt gebruikt bij het maken van een nieuwe tabel en vervolgens de gegevens over te dragen naar de compatibele tabel vóór de herstel bewerking. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- U kunt compatibiliteitsproblemen voorkomen door ervoor te zorgen dat dezelfde versie van MySQL wordt gebruikt in het bron- en doelsysteem bij het dumpen van databases. Als uw bestaande MySQL-server bijvoorbeeld versie 5,7 is, moet u migreren naar Azure Database for MySQL geconfigureerd voor het uitvoeren van versie 5,7. De `mysql_upgrade` opdracht werkt niet in een Azure Database for MySQL-server en wordt niet ondersteund. Als u een upgrade wilt uitvoeren naar MySQL-versies, moet u eerst de data base van uw lagere versie dumpen of exporteren naar een hogere versie van MySQL in uw eigen omgeving. Voer `mysql_upgrade`uit voordat u een migratie naar een Azure Database for MySQL uitvoert.

## <a name="performance-considerations"></a>Prestatieoverwegingen
Bekijk de volgende overwegingen bij het dumpen van grote data bases om de prestaties te optimaliseren:
-   Gebruik de optie `exclude-triggers` in mysqldump bij het dumpen van data bases. Sluit triggers uit van dump bestanden om te voor komen dat trigger opdrachten worden geactiveerd tijdens het herstellen van gegevens. 
-   Gebruik de optie `single-transaction` om de isolatie modus voor trans acties in te stellen op HERHAALbaar lezen en verzendt een SQL-instructie voor het starten van een trans actie naar de server voordat gegevens worden gedumpt. Het dumpen van veel tabellen binnen één trans actie leidt ertoe dat er tijdens het herstellen enkele extra opslag ruimte wordt gebruikt. De optie `single-transaction` en de `lock-tables` optie sluiten elkaar wederzijds uit omdat VERGRENDELings tabellen alle trans acties die in behandeling zijn, impliciet moeten worden doorgevoerd. Als u grote tabellen wilt dumpen, moet u de optie `single-transaction` combi neren met de optie `quick`. 
-   Gebruik de `extended-insert` syntaxis met meerdere rijen die verschillende waardelijsten bevat. Dit leidt tot een kleiner dump bestand en versnelt het invoegen wanneer het bestand opnieuw wordt geladen.
-  Gebruik de optie `order-by-primary` in mysqldump bij het dumpen van data bases, zodat de gegevens in de volg orde van de primaire sleutel worden gescripteerd.
-   Gebruik de optie `disable-keys` in mysqldump bij het dumpen van gegevens om refererende-sleutel beperkingen voor het laden uit te scha kelen. Het uitschakelen van externe-sleutel controles levert prestatie verbeteringen. Schakel de beperkingen in en controleer de gegevens na de belasting om de referentiële integriteit te waarborgen.
-   Gebruik gepartitioneerde tabellen wanneer dat nodig is.
-   Gegevens parallel laden. Vermijd te veel parallellisme waardoor u een resource limiet kunt bereiken en resources kunt bewaken met behulp van de metrische gegevens die beschikbaar zijn in de Azure Portal. 
-   Gebruik de optie `defer-table-indexes` in mysqlpump bij het dumpen van data bases, zodat het maken van de index plaatsvindt nadat tabellen gegevens zijn geladen.
-   Gebruik de optie `skip-definer` in mysqlpump om de instructie define en SQL SECURITY te weglaten uit de Create-instructies voor weer gaven en opgeslagen procedures.  Wanneer u het dump bestand opnieuw laadt, worden er objecten gemaakt die gebruikmaken van de standaard definiëren en SQL-BEVEILIGINGS waarden.
-   Kopieer de back-upbestanden naar een Azure-Blob/-Store en voer de herstel bewerking uit. Dit is veel sneller dan het terugzetten via internet.

## <a name="create-a-backup-file-from-the-command-line-using-mysqldump"></a>Maak een back-upbestand via de opdracht regel met behulp van mysqldump
Als u een back-up wilt maken van een bestaande MySQL-data base op de lokale on-premises server of op een virtuele machine, voert u de volgende opdracht uit: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

De para meters die u moet opgeven, zijn:
- [uname] Uw data base-gebruikers naam 
- door Het wacht woord voor uw data base (er is geen spatie tussen-p en het wacht woord) 
- dbname De naam van uw data base 
- [upbestand. SQL] de bestands naam van de back-up van de data base 
- [--opt] De optie mysqldump 

Als u bijvoorbeeld een back-up wilt maken van een Data Base met de naam ' testdb ' op de MySQL-server met de gebruikers naam ' test ' en zonder wacht woord aan een bestand testdb_backup. SQL, gebruikt u de volgende opdracht. De opdracht maakt een back-up van de `testdb`-data base in een bestand met de naam `testdb_backup.sql`, dat alle SQL-instructies bevat die nodig zijn om de data base opnieuw te maken. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Als u specifieke tabellen in de Data Base wilt selecteren om een back-up te maken, vermeldt u de tabel namen gescheiden door spaties. Als u bijvoorbeeld alleen een back-up wilt maken van de tabellen Tabel1 en tabel2 van de ' testdb ', volgt u dit voor beeld: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```
Als u een back-up van meer dan één data base tegelijk wilt maken, gebruikt u de-data base-switch en geeft u de database namen op, gescheiden door spaties. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```

## <a name="create-a-database-on-the-target-azure-database-for-mysql-server"></a>Een Data Base maken op de doel-Azure Database for MySQL server
Maak een lege data base op de doel Azure Database for MySQL server waarnaar u de gegevens wilt migreren. Gebruik een hulp programma zoals MySQL Workbench, Toad of Navicat om de data base te maken. De data base kan dezelfde naam hebben als de Data Base waarin de dumping gegevens zijn opgenomen of u kunt een Data Base met een andere naam maken.

Als u verbinding wilt krijgen, zoekt u de verbindings gegevens in het **overzicht** van uw Azure database for MySQL.

![De verbindings gegevens in de Azure Portal zoeken](./media/concepts-migrate-dump-restore/1_server-overview-name-login.png)

Voeg de verbindings gegevens toe aan uw MySQL Workbench.

![MySQL Workbench-verbindings reeks](./media/concepts-migrate-dump-restore/2_setup-new-connection.png)


## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Uw MySQL-data base herstellen met behulp van opdracht regel of MySQL Workbench
Wanneer u de doel database hebt gemaakt, kunt u de MySQL-opdracht of MySQL Workbench gebruiken om de gegevens in de specifieke zojuist gemaakte data base te herstellen vanuit het dump bestand.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
In dit voor beeld herstelt u de gegevens in de zojuist gemaakte Data Base op de doel Azure Database for MySQL server.
```bash
$ mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>Exporteren met behulp van PHPMyAdmin
Als u wilt exporteren, kunt u gebruikmaken van de algemene hulp middelen phpMyAdmin, die u mogelijk al lokaal hebt geïnstalleerd in uw omgeving. Uw MySQL-data base exporteren met behulp van PHPMyAdmin:
1. Open phpMyAdmin.
2. Selecteer uw data base. Klik op de naam van de data base in de lijst aan de linkerkant. 
3. Klik op de koppeling **exporteren** . Er wordt een nieuwe pagina weer gegeven om de dump van de Data Base weer te geven.
4. Klik in het gedeelte exporteren op de koppeling **Alles selecteren** om de tabellen in de data base te kiezen. 
5. Klik in het gebied SQL-opties op de gewenste opties. 
6. Klik op de optie **Opslaan als bestand** en de bijbehorende compressie optie en klik vervolgens op de knop **Go** . Er wordt een dialoog venster weer gegeven waarin u wordt gevraagd het bestand lokaal op te slaan.

## <a name="import-using-phpmyadmin"></a>Importeren met behulp van PHPMyAdmin
Het importeren van uw data base lijkt op het exporteren. Voer de volgende acties uit:
1. Open phpMyAdmin. 
2. Klik op de pagina phpMyAdmin Setup op **toevoegen** om uw Azure database for mysql-server toe te voegen. Geef de verbindings gegevens en aanmeldings gegevens op.
3. Maak een Data Base met de juiste naam en selecteer deze aan de linkerkant van het scherm. Als u de bestaande data base opnieuw wilt schrijven, klikt u op de naam van de data base, selecteert u alle selectie vakjes naast de tabel namen en selecteert u **neerzetten** om de bestaande tabellen te verwijderen. 
4. Klik op de **SQL** -koppeling om de pagina weer te geven waarop u SQL-opdrachten kunt typen of uw SQL-bestand moet uploaden. 
5. Gebruik de knop **Bladeren** om het database bestand te zoeken. 
6. Klik op de knop **Go** om de back-up te exporteren, de SQL-opdrachten uit te voeren en de data base opnieuw te maken.

## <a name="next-steps"></a>Volgende stappen
- [Toepassingen verbinden met Azure database for MySQL](./howto-connection-string.md).
- Zie de [hand leiding voor database migratie](https://aka.ms/datamigration)voor meer informatie over het migreren van data bases naar Azure database for MySQL.
