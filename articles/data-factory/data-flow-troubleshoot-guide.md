---
title: Problemen met gegevens stromen oplossen
description: Meer informatie over het oplossen van problemen met gegevens stromen in Azure Data Factory.
services: data-factory
ms.author: makromer
author: kromerm
manager: anandsub
ms.service: data-factory
ms.topic: troubleshooting
ms.custom: seo-lt-2019
ms.date: 12/06/2019
ms.openlocfilehash: b972bbeac419d88afdd257a7fd19587dbaedf0d9
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930179"
---
# <a name="troubleshoot-azure-data-factory-data-flows"></a>Problemen met Azure Data Factory gegevens stromen oplossen

In dit artikel worden algemene probleemoplossings methoden voor gegevens stromen in Azure Data Factory besproken.

## <a name="common-errors-and-messages"></a>Veelvoorkomende fouten en berichten

### <a name="error-message-df-sys-01-shadeddatabricksorgapachehadoopfsazureazureexception-commicrosoftazurestoragestorageexception-the-specified-container-does-not-exist"></a>Fout bericht: DF-SYS-01: gearceerd. databricks. org. apache. Hadoop. FS. Azure. AzureException: com. micro soft. Azure. storage. StorageException: de opgegeven container bestaat niet.

- **Symptomen**: de uitvoering van voor beeld, fout opsporing en pipeline-gegevens stroom is mislukt omdat de container niet bestaat

- **Oorzaak**: wanneer dataset een container bevat die niet voor komt in de opslag

- **Oplossing**: Zorg ervoor dat de container waarnaar u verwijst in uw gegevensset bestaat

### <a name="error-message-df-sys-01-javalangassertionerror-assertion-failed-conflicting-directory-structures-detected-suspicious-paths"></a>Fout bericht: DF-SYS-01: Java. lang. AssertionError: Assertion is mislukt: er is een conflicterende Directory-structuur gedetecteerd. Verdachte paden

- **Symptomen**: Joker tekens gebruiken in bron transformatie met Parquet-bestanden

- **Oorzaak**: onjuiste of ongeldige Joker teken syntaxis

- **Oplossing**: Controleer de syntaxis van de joker tekens die u gebruikt in de opties voor de bron transformatie

### <a name="error-message-df-src-002-container-container-name-is-required"></a>Fout bericht: DF-SRC-002: container (container name) is vereist

- **Symptomen**: de uitvoering van voor beeld, fout opsporing en pipeline-gegevens stroom is mislukt omdat de container niet bestaat

- **Oorzaak**: wanneer dataset een container bevat die niet voor komt in de opslag

- **Oplossing**: Zorg ervoor dat de container waarnaar u verwijst in uw gegevensset bestaat

### <a name="error-message-df-uni-001-primarykeyvalue-has-incompatible-types-integertype-and-stringtype"></a>Fout bericht: DF-UNI-001: PrimaryKeyValue heeft incompatibele typen IntegerType en StringType

- **Symptomen**: de uitvoering van voor beeld, fout opsporing en pipeline-gegevens stroom is mislukt omdat de container niet bestaat

- **Oorzaak**: er is een fout opgetreden bij het invoegen van een onjuist primair sleutel type in data base-sinks

- **Oplossing**: gebruik een afgeleide kolom om de kolom te casten die u gebruikt voor de primaire sleutel in uw gegevens stroom, zodat deze overeenkomt met het gegevens type van uw doel database

### <a name="error-message-df-sys-01-commicrosoftsqlserverjdbcsqlserverexception-the-tcpip-connection-to-the-host-xxxxxdatabasewindowsnet-port-1433-has-failed-error-xxxxdatabasewindowsnet-verify-the-connection-properties-make-sure-that-an-instance-of-sql-server-is-running-on-the-host-and-accepting-tcpip-connections-at-the-port-make-sure-that-tcp-connections-to-the-port-are-not-blocked-by-a-firewall"></a>Fout bericht: DF-SYS-01: com. micro soft. sqlserver. JDBC. SQLServerException: de TCP/IP-verbinding met de host xxxxx.database.windows.net poort 1433 is mislukt. Fout: xxxx.database.windows.net. Controleer de verbindings eigenschappen. Zorg ervoor dat een exemplaar van SQL Server wordt uitgevoerd op de host en TCP/IP-verbindingen accepteert op de poort. Zorg ervoor dat TCP-verbindingen met de poort niet worden geblokkeerd door een firewall. "

- **Symptomen**: kan geen voor beeld van gegevens weer geven of een pijp lijn uitvoeren met de database bron of sink

- **Oorzaak**: de data base wordt beveiligd door een firewall

- **Oplossing**: Open de firewall toegang tot de data base

### <a name="error-message-df-sys-01-commicrosoftsqlserverjdbcsqlserverexception-there-is-already-an-object-named-xxxxxx-in-the-database"></a>Fout bericht: DF-SYS-01: com. micro soft. sqlserver. JDBC. SQLServerException: er bevindt zich al een object met de naam ' xxxxxx ' in de data base.

- **Symptomen**: de Sink kan geen tabel maken

- **Oorzaak**: er bestaat al een bestaande tabel naam in de doel database met dezelfde naam die is gedefinieerd in uw bron of in de gegevensset

- **Oplossing**: Wijzig de naam van de tabel die u wilt maken

### <a name="error-message-df-sys-01-commicrosoftsqlserverjdbcsqlserverexception-string-or-binary-data-would-be-truncated"></a>Fout bericht: DF-SYS-01: com. micro soft. sqlserver. JDBC. SQLServerException: teken reeks of binaire gegevens worden afgekapt. 

- **Symptomen**: wanneer u gegevens naar een SQL-Sink schrijft, mislukt uw gegevens stroom bij het uitvoeren van de pijp lijn met mogelijke Afbrekings fout.

- **Oorzaak**: een veld van uw gegevens stroom wordt toegewezen aan een kolom in uw SQL database niet breed genoeg is om de waarde op te slaan, waardoor het SQL-stuur programma deze fout kan genereren

- **Oplossing**: u kunt de lengte van de gegevens voor teken reeks kolommen met ```left()``` in een afgeleide kolom beperken of het [patroon "fout rijen" implementeren.](how-to-data-flow-error-rows.md)

### <a name="error-message-since-spark-23-the-queries-from-raw-jsoncsv-files-are-disallowed-when-the-referenced-columns-only-include-the-internal-corrupt-record-column"></a>Fout bericht: sinds Spark 2,3 worden de query's van onbewerkte JSON/CSV-bestanden niet toegestaan wanneer de kolommen waarnaar wordt verwezen alleen de kolom interne beschadigde record bevatten. 

- **Symptomen**: het lezen van een JSON-bron mislukt

- **Oorzaak**: wanneer het lezen van een JSON-bron met één document op veel geneste regels, ADF, via Spark, niet kan bepalen waar een nieuw document begint en het vorige document eindigt.

- **Oplossing**: voor de bron transformatie die gebruikmaakt van een JSON-gegevensset, vouwt u JSON-instellingen uit en schakelt u ' Eén document ' in.

### <a name="error-message-duplicate-columns-found-in-join"></a>Fout bericht: dubbele kolommen gevonden in samen voeging

- **Symptomen**: deelname aan trans formatie resulteerde in kolommen aan de linkerkant en aan de rechter kant die dubbele kolom namen bevatten

- **Oorzaak**: de stromen die worden toegevoegd, hebben algemene kolom namen

- **Oplossing**: Voeg een SELECT-transforamtion toe na de koppeling en selecteer ' Remove Duplicate columns ' voor de invoer en uitvoer.


## <a name="general-troubleshooting-guidance"></a>Algemene richt lijnen voor probleem oplossing

1. Controleer de status van uw gegevensset-verbindingen. Ga in elke bron-en Sink-trans formatie naar de gekoppelde service voor elke gegevensset die u gebruikt en test verbindingen.
2. Controleer de status van uw bestands-en tabel verbindingen van de ontwerp functie voor gegevens stromen. Schakel over op fout opsporing en klik op voor beeld van gegevens op de bron transformaties om ervoor te zorgen dat u toegang hebt tot uw gegevens.
3. Als alles er goed uitziet in de preview van gegevens, gaat u naar de ontwerp functie voor pijp lijnen en plaatst u uw gegevens stroom in een pijplijn activiteit. Fout opsporing voor de pijp lijn voor een end-to-end-test.

## <a name="next-steps"></a>Volgende stappen

Probeer deze bronnen voor meer informatie over probleem oplossing:

*  [Data Factory Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory functie aanvragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Video's](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Stack Overflow forum voor Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-informatie over Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Prestatie gids voor gegevens stromen van ADF-toewijzing](concepts-data-flow-performance.md)
