---
title: Apparaat inrichten met behulp van symmetrische sleutel Attestation-Azure IoT Edge
description: Symmetrische-sleutel attest gebruiken om het automatisch inrichten van apparaten te testen voor Azure IoT Edge met Device Provisioning Service
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: mrohera
ms.date: 10/04/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c42d13f4d2e00b67a2ef471a07c80e1ef61e9c07
ms.sourcegitcommit: 57eb9acf6507d746289efa317a1a5210bd32ca2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2019
ms.locfileid: "74666321"
---
# <a name="create-and-provision-an-iot-edge-device-using-symmetric-key-attestation"></a>Een IoT Edge apparaat maken en inrichten met behulp van symmetrische sleutel attest

Azure IoT Edge apparaten kunnen automatisch worden ingericht met behulp van de [Device Provisioning Service](../iot-dps/index.yml) , net zoals apparaten die niet Edge-ingeschakeld zijn. Als u niet bekend bent met het proces van automatische inrichting, raadpleegt u de [concepten voor automatische inrichting](../iot-dps/concepts-auto-provisioning.md) voordat u doorgaat.

In dit artikel wordt beschreven hoe u een individuele inschrijving voor een Device Provisioning Service kunt maken met behulp van symmetrische sleutel attest op een IoT Edge apparaat, met de volgende stappen:

* Maak een instantie van IoT Hub Device Provisioning Service (DPS).
* Een afzonderlijke inschrijving voor het apparaat maken.
* Installeer de IoT Edge runtime en maak verbinding met de IoT Hub.

Symmetrische-sleutel attest is een eenvoudige benadering voor het verifiëren van een apparaat met een Device Provisioning service-exemplaar. Deze Attestation-methode vertegenwoordigt een ' Hello World '-ervaring voor ontwikkel aars die nieuw zijn voor het inrichten van apparaten of waarvoor geen strikte beveiligings vereisten gelden. Attestation van apparaten met behulp van een [TPM](../iot-dps/concepts-tpm-attestation.md) of [X. 509-certificaten](../iot-dps/concepts-security.md#x509-certificates) is veiliger en moet worden gebruikt voor strengere beveiligings vereisten.

## <a name="prerequisites"></a>Vereisten

* Een actieve IoT Hub
* Een fysiek of virtueel apparaat

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>De IoT Hub Device Provisioning Service instellen

Maak een nieuw exemplaar van de IoT Hub Device Provisioning Service in Azure en koppel deze aan uw IoT-hub. U kunt de instructies voor [het instellen van de IOT hub DPS](../iot-dps/quick-setup-auto-provision.md)volgen.

Nadat u de Device Provisioning Service hebt uitgevoerd, kopieert u de waarde van **id-bereik** van de pagina overzicht. U gebruikt deze waarde bij het configureren van de IoT Edge runtime.

## <a name="choose-a-unique-registration-id-for-the-device"></a>Een unieke registratie-ID voor het apparaat kiezen

Er moet een unieke registratie-ID worden gedefinieerd om elk apparaat te identificeren. U kunt het MAC-adres, serie nummer of unieke informatie van het apparaat gebruiken.

In dit voor beeld gebruiken we een combi natie van een MAC-adres en serie nummer dat de volgende teken reeks voor een registratie-ID vormt.

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Maak een unieke registratie-ID voor uw apparaat. Geldige tekens zijn kleine letters en streepjes ('-').

## <a name="create-a-dps-enrollment"></a>Een DPS-inschrijving maken

Gebruik de registratie-ID van uw apparaat om een afzonderlijke inschrijving in DPS te maken.

Wanneer u een inschrijving in DPS maakt, hebt u de mogelijkheid om een **eerste dubbele toestand**van het apparaat te declareren. In het dubbele apparaat kunt u Tags instellen om apparaten te groeperen op elke gewenste metrische waarde in uw oplossing, zoals regio, omgeving, locatie of apparaattype. Deze tags worden gebruikt voor het maken van [automatische implementaties](how-to-deploy-monitor.md).

> [!TIP]
> Groeps registraties zijn ook mogelijk bij het gebruik van symmetrische sleutel attest en dezelfde beslissingen als afzonderlijke inschrijvingen.

1. Ga in het [Azure Portal](https://portal.azure.com)naar uw exemplaar van IOT hub Device Provisioning Service.

1. Selecteer onder **instellingen**de optie **inschrijvingen beheren**.

1. Selecteer **Individuele inschrijving toevoegen** en voer de volgende stappen uit om de registratie te configureren:  

   1. Selecteer voor **mechanisme** **symmetrische sleutel**.

   1. Schakel het selectie vakje **sleutels automatisch genereren** in.

   1. Geef de **registratie-id** op die u hebt gemaakt voor uw apparaat.

   1. Geef een **IOT hub apparaat-id** voor uw apparaat op als u wilt. U kunt apparaat-Id's gebruiken om een afzonderlijk apparaat te richten op het implementeren van een module. Als u geen apparaat-ID opgeeft, wordt de registratie-ID gebruikt.

   1. Selecteer **waar** om te declareren dat de inschrijving voor een IOT edge apparaat is. Voor de registratie van een groep moeten alle apparaten worden IoT Edge apparaten of geen van beide.

   1. Accepteer de standaard waarde van het toewijzings beleid van de Device Provisioning Service voor de **manier waarop u apparaten aan hubs wilt toewijzen** of kies een andere waarde die specifiek is voor deze inschrijving.

   1. Kies de gekoppelde **IOT hub** waarmee u uw apparaat wilt verbinden. U kunt meerdere hubs kiezen en het apparaat wordt toegewezen aan een van deze op basis van het geselecteerde toewijzings beleid.

   1. Kies **hoe u wilt dat apparaatgegevens worden verwerkt bij het opnieuw inrichten** wanneer apparaten na de eerste keer worden ingericht.

   1. Voeg indien gewenst een tag-waarde toe aan de **eerste dubbele toestand** van het apparaat. U kunt tags gebruiken om groepen apparaten te richten op het implementeren van modules. Bijvoorbeeld:

      ```json
      {
         "tags": {
            "environment": "test"
         },
         "properties": {
            "desired": {}
         }
      }
      ```

   1. Zorg ervoor dat **vermelding inschakelen** is ingesteld op **inschakelen**.

   1. Selecteer **Opslaan**.

Nu een inschrijving voor dit apparaat bestaat, kan de IoT Edge runtime automatisch het apparaat inrichten tijdens de installatie. Zorg ervoor dat u de waarde van de **primaire sleutel** van uw registratie kopieert om te gebruiken tijdens de installatie van de IOT Edge runtime, of dat u een apparaatcode wilt maken voor gebruik met een groeps registratie.

## <a name="derive-a-device-key"></a>Een apparaatcode afleiden

> [!NOTE]
> Deze sectie is alleen vereist als u een groeps registratie gebruikt.

Elk apparaat gebruikt de afgeleide apparaatwachtwoord met uw unieke registratie-ID voor het uitvoeren van de symmetrische sleutel attest met de inschrijving tijdens het inrichten. Als u de apparaatcode wilt genereren, gebruikt u de sleutel die u hebt gekopieerd uit uw DPS-inschrijving om een [HMAC-sha256](https://wikipedia.org/wiki/HMAC) van de unieke registratie-id voor het apparaat te berekenen en zet u het resultaat om in Base64-indeling.

Neem de primaire of secundaire sleutel van uw inschrijving niet op in uw apparaatcode.

### <a name="linux-workstations"></a>Linux-werk stations

Als u een Linux-werk station gebruikt, kunt u openssl gebruiken om uw afgeleide apparaatwachtwoord te genereren, zoals wordt weer gegeven in het volgende voor beeld.

Vervang de waarde van **Key** door de **primaire sleutel** die u eerder hebt genoteerd.

Vervang de waarde van **REG_ID** door de registratie-id van uw apparaat.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```

### <a name="windows-based-workstations"></a>Windows-werk stations

Als u een Windows-werk station gebruikt, kunt u Power shell gebruiken om uw afgeleide apparaatwachtwoord te genereren, zoals wordt weer gegeven in het volgende voor beeld.

Vervang de waarde van **Key** door de **primaire sleutel** die u eerder hebt genoteerd.

Vervang de waarde van **REG_ID** door de registratie-id van uw apparaat.

```powershell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```powershell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```

## <a name="install-the-iot-edge-runtime"></a>De IoT Edge runtime installeren

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. De onderdelen worden in containers uitgevoerd en bieden u de mogelijkheid om extra containers op het apparaat te implementeren, zodat u code aan de rand kunt uitvoeren.

U hebt de volgende informatie nodig bij het inrichten van uw apparaat:

* De waarde voor het bereik van de DPS **-id**
* De **registratie-id** van het apparaat dat u hebt gemaakt
* De **primaire sleutel** die u hebt gekopieerd uit de DPS-inschrijving

> [!TIP]
> Voor groeps registraties hebt u de [afgeleide sleutel](#derive-a-device-key) van elk apparaat nodig in plaats van de registratie sleutel voor DPS.

### <a name="linux-device"></a>Linux-apparaat

Volg de instructies voor de architectuur van uw apparaat. Zorg ervoor dat u de IoT Edge-runtime configureert voor automatisch, niet hand matig, inrichten.

[Installeer de Azure IoT Edge runtime op Linux](how-to-install-iot-edge-linux.md)

De sectie in het configuratie bestand voor het inrichten van symmetrische sleutels ziet er als volgt uit:

```yaml
# DPS symmetric key provisioning configuration
provisioning:
   source: "dps"
   global_endpoint: "https://global.azure-devices-provisioning.net"
   scope_id: "{scope_id}"
   attestation:
      method: "symmetric_key"
      registration_id: "{registration_id}"
      symmetric_key: "{symmetric_key}"
```

Vervang de waarden van de tijdelijke aanduidingen voor `{scope_id}`, `{registration_id}`en `{symmetric_key}` met de gegevens die u eerder hebt verzameld.

### <a name="windows-device"></a>Windows-apparaat

Installeer de IoT Edge runtime op het apparaat waarvoor u een afgeleide-apparaatwachtwoord hebt gegenereerd. U configureert de IoT Edge runtime voor automatisch, niet hand matig, provisioning.

Zie [install the Azure IOT Edge runtime on Windows](how-to-install-iot-edge-windows.md)(Engelstalig) voor meer informatie over het installeren van IOT Edge op Windows, inclusief vereisten en instructies voor taken zoals het beheren van containers en het bijwerken van IOT Edge.

1. Open een Power shell-venster in de beheerders modus. Zorg ervoor dat u een AMD64-sessie van Power shell gebruikt bij het installeren van IoT Edge, niet in Power shell (x86).

1. Met de opdracht **Deploy-IoTEdge** wordt gecontroleerd of de Windows-computer een ondersteunde versie heeft, wordt de functie containers ingeschakeld, waarna de Moby-runtime en de IOT Edge-runtime worden gedownload. De opdracht wordt standaard ingesteld op het gebruik van Windows-containers.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

1. Op dit moment kunnen IoT-kern apparaten automatisch opnieuw worden opgestart. Op andere Windows 10-of Windows Server-apparaten wordt u mogelijk gevraagd om opnieuw op te starten. Als dit het geval is, start u het apparaat nu opnieuw op. Zodra het apparaat klaar is, voert u Power shell als beheerder opnieuw uit.

1. De **initialisatie-IoTEdge-** opdracht configureert de IOT Edge runtime op de computer. De opdracht wordt standaard ingesteld op hand matig inrichten met Windows-containers, tenzij u de vlag `-Dps` gebruikt om automatische inrichting te gebruiken.

   Vervang de waarden van de tijdelijke aanduidingen voor `{scope_id}`, `{registration_id}`en `{symmetric_key}` met de gegevens die u eerder hebt verzameld.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -Dps -ScopeId {scope ID} -RegistrationId {registration ID} -SymmetricKey {symmetric key}
   ```

## <a name="verify-successful-installation"></a>Geslaagde installatie controleren

Als de runtime is gestart, kunt u naar uw IoT Hub gaan en IoT Edge modules op het apparaat implementeren. Gebruik de volgende opdrachten op het apparaat om te controleren of de runtime is geïnstalleerd en is gestart.

### <a name="linux-device"></a>Linux-apparaat

Controleer de status van de IoT Edge-service.

```cmd/sh
systemctl status iotedge
```

Bekijk service Logboeken.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Een lijst met actieve modules weer geven.

```cmd/sh
iotedge list
```

### <a name="windows-device"></a>Windows-apparaat

Controleer de status van de IoT Edge-service.

```powershell
Get-Service iotedge
```

Bekijk service Logboeken.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Een lijst met actieve modules weer geven.

```powershell
iotedge list
```

U kunt controleren of de afzonderlijke registratie die u hebt gemaakt in Device Provisioning Service is gebruikt. Navigeer naar het Device Provisioning service-exemplaar in het Azure Portal. Open de inschrijvings gegevens voor de afzonderlijke inschrijving die u hebt gemaakt. U ziet dat de status van de registratie is **toegewezen** en dat de apparaat-id wordt vermeld.

## <a name="next-steps"></a>Volgende stappen

Met het inschrijvings proces voor Device Provisioning Service kunt u de apparaat-ID en de dubbele Tags van het apparaat instellen op hetzelfde moment als u het nieuwe apparaat inricht. U kunt deze waarden gebruiken om afzonderlijke apparaten of groepen apparaten te richten met behulp van automatische Apparaatbeheer. Meer informatie over [het implementeren en bewaken van IOT Edge modules op schaal met behulp van de Azure Portal](how-to-deploy-monitor.md) of [met behulp van Azure cli](how-to-deploy-monitor-cli.md).
