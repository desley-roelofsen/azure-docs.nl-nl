---
title: Eenmalige aanmelding voor de toepassing configureren | Microsoft Docs
description: Eenmalige aanmelding configureren voor een aangepaste toepassing die u ontwikkelt en registreert met Azure AD.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: ryanwi
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbc9f20903d65fbde161d26ed3e194e63d341ccb
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320956"
---
# <a name="how-to-configure-single-sign-on-for-an-application"></a>Eenmalige aanmelding voor een toepassing configureren

Het inschakelen van federatieve eenmalige aanmelding (SSO) in uw app wordt automatisch ingeschakeld wanneer federeren via Azure AD voor OpenID Connect Connect, SAML 2,0 of WS-inschakelt. Als uw eind gebruikers zich moeten aanmelden ondanks al een bestaande sessie met Azure AD, is het waarschijnlijk dat uw app onjuist is geconfigureerd.

* Als u ADAL/MSAL gebruikt, zorg er dan voor dat u **PromptBehavior** hebt ingesteld op **auto** in plaats van **altijd**.

* Als u een mobiele app bouwt, hebt u mogelijk aanvullende configuraties nodig om brokered of niet-brokered SSO in te scha kelen.

Zie [SSO van meerdere apps inschakelen in Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)voor Android.<br>

Zie [SSO van meerdere apps inschakelen in Ios](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)voor IOS.

## <a name="next-steps"></a>Volgende stappen

[Azure AD SSO](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[SSO van meerdere apps inschakelen in Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[SSO van meerdere apps inschakelen in iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Apps integreren in AzureAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Toestemming en machtiging voor AzureAD v 2.0 geconvergeerde apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
