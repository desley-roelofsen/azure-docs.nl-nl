---
title: Kruis platform meldingen verzenden naar gebruikers met Azure Notification Hubs (ASP.NET)
description: Meer informatie over het gebruik van Notification Hubs sjablonen voor het verzenden van een neutraal-melding die gericht is op alle platformen.
services: notification-hubs
documentationcenter: ''
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 09/30/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: 8f4de88ed79ee802866579448681cfe6cee3e654
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2019
ms.locfileid: "72293426"
---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Kruis platform meldingen verzenden naar gebruikers met Notification Hubs

Deze zelf studie is gebaseerd op de vorige zelf studie, [Meldingen verzenden naar specifieke gebruikers met behulp van Azure Notification Hubs]. In deze zelf studie wordt beschreven hoe u meldingen pusht naar alle apparaten die zijn geregistreerd voor een specifieke geverifieerde gebruiker. Deze aanpak vereist meerdere aanvragen om een melding te verzenden naar elk ondersteund client platform. Azure Notification Hubs ondersteunt sjablonen, waarmee u kunt opgeven hoe een specifiek apparaat meldingen wil ontvangen. Deze methode vereenvoudigt het verzenden van cross-platform meldingen.

In dit artikel wordt beschreven hoe u sjablonen kunt gebruiken om een melding te verzenden die gericht is op alle platformen. In dit artikel wordt één aanvraag gebruikt voor het verzenden van een platform neutrale melding. Zie [Notification hubs Overview][Templates]voor meer gedetailleerde informatie over sjablonen.

> [!IMPORTANT]
> Windows Phone projecten 8,1 en lager worden niet ondersteund in Visual Studio 2019. Zie [Visual Studio 2019-platform doelen en-compatibiliteit](/visualstudio/releases/2019/compatibility)voor meer informatie.

> [!NOTE]
> Met Notification Hubs kan een apparaat meerdere sjablonen registreren met behulp van dezelfde tag. In dit geval wordt een inkomend bericht dat de tag bedoelt, in meerdere meldingen bezorgd bij het apparaat, één voor elke sjabloon. Met dit proces kunt u hetzelfde bericht weer geven in meerdere visuele meldingen, zoals een badge en als een pop-upmelding in een Windows Store-app.

## <a name="send-cross-platform-notifications-using-templates"></a>Cross-platform meldingen verzenden met behulp van sjablonen

In deze sectie wordt gebruikgemaakt van de voorbeeld code die u hebt gemaakt in de zelf studie [meldingen verzenden naar specifieke gebruikers met behulp van Azure notification hubs] . U kunt het voorbeeld downloaden uit [GitHub](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/NotifyUsers).

Voer de volgende stappen uit om cross-platform meldingen te verzenden met behulp van sjablonen:

1. Vouw in Visual Studio in **Solution Explorer**de map **controllers** uit en open vervolgens het bestand *RegisterController.cs* .

1. Zoek het code blok in de methode `Put` waarmee een nieuwe registratie wordt gemaakt en vervang vervolgens de `switch`-inhoud door de volgende code:

    ```csharp
    switch (deviceUpdate.Platform)
    {
        case "mpns":
            var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                    "<wp:Toast>" +
                        "<wp:Text1>$(message)</wp:Text1>" +
                    "</wp:Toast> " +
                "</wp:Notification>";
            registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
            break;
        case "wns":
            toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
            registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
            break;
        case "apns":
            var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
            registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
            break;
        case "fcm":
            var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
            registration = new FcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
            break;
        default:
            throw new HttpResponseException(HttpStatusCode.BadRequest);
    }
    ```

    Met deze code wordt de platformspecifieke methode aangeroepen voor het maken van een sjabloon registratie in plaats van een systeem eigen registratie. Omdat sjabloon registraties zijn afgeleid van systeem eigen registraties, hoeft u geen bestaande registraties te wijzigen.

1. Open in **Solution Explorer**in de map **Controllers** het bestand *NotificationsController.cs* . Vervang de `Post`-methode door de volgende code:

    ```csharp
    public async Task<HttpResponseMessage> Post()
    {
        var user = HttpContext.Current.User.Identity.Name;
        var userTag = "username:" + user;

        var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
        await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

        return Request.CreateResponse(HttpStatusCode.OK);
    }
    ```

    Met deze code wordt op hetzelfde moment een melding verzonden naar alle platformen. U geeft geen systeem eigen Payload op. Notification Hubs bouwt en levert de juiste Payload aan elk apparaat met de opgegeven label waarde, zoals opgegeven in de geregistreerde sjablonen.

1. Publiceer uw web API-project opnieuw.

1. Voer de client-app opnieuw uit om te controleren of de registratie is geslaagd.

1. U kunt de client-app eventueel implementeren op een tweede apparaat en vervolgens de app uitvoeren. Op elk apparaat wordt een melding weer gegeven.

## <a name="next-steps"></a>Volgende stappen

Nu u deze zelf studie hebt voltooid, kunt u meer informatie vinden over Notification Hubs en sjablonen in deze artikelen:

* Zie voor een ander scenario voor het gebruik van sjablonen de [Push meldingen naar specifieke Windows-apparaten met universeel Windows-platform][Use Notification Hubs to send breaking news] -zelf studie voor toepassingen.
* Zie [Notification hubs Overview][Templates]voor meer gedetailleerde informatie over sjablonen.

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Push to users ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Push to users Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Visual Studio 2012 Express for Windows 8]: https://visualstudio.microsoft.com/downloads/

[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: https://go.microsoft.com/fwlink/p/?LinkId=314257
[Meldingen verzenden naar specifieke gebruikers met behulp van Azure Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: https://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: https://msdn.microsoft.com/library/windowsazure/jj927172.aspx
