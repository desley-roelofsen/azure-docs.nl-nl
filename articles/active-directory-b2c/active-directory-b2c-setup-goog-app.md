---
title: Registratie instellen en aanmelden met een Google-account
titleSuffix: Azure AD B2C
description: Gebruik Azure Active Directory B2C om u aan te melden en u aan te melden bij klanten met Google accounts in uw toepassingen.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 1e23c79b1e09f3e3a7aaa21b9257bfe6bd43f7e8
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74950462"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Registratie instellen en aanmelden met een Google-account met behulp van Azure Active Directory B2C

## <a name="create-a-google-application"></a>Een Google-toepassing maken

Als u een Google-account wilt gebruiken als een [ID-provider](active-directory-b2c-reference-oauth-code.md) in Azure Active Directory B2C (Azure AD B2C), moet u een toepassing maken in uw Tenant waarmee deze wordt vertegenwoordigd. Als u nog geen Google-account hebt, kunt u zich aanmelden bij [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Meld u aan bij de [Google developers-console](https://console.developers.google.com/) met uw Google-account referenties.
1. Selecteer in de linkerbovenhoek van de pagina de project lijst en selecteer vervolgens **Nieuw project**.
1. Voer een **project naam**in, klik op **maken**en zorg ervoor dat u het nieuwe project gebruikt.
1. Selecteer **referenties** in het linkermenu en selecteer vervolgens **referenties maken** > **OAuth-client-id**.
1. Onder **toepassings type**selecteert u **Web Application**.
1. Voer een **naam** in voor uw toepassing, voer `https://your-tenant-name.b2clogin.com` in **geautoriseerde java script-oorsprong**in en `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in **geautoriseerde omleidings-uri's**. Vervang `your-tenant-name` door de naam van uw Tenant. U moet alle kleine letters gebruiken bij het invoeren van de naam van uw Tenant, zelfs als de Tenant is gedefinieerd met hoofd letters in Azure AD B2C.
1. Klik op **Maken**.
1. Kopieer de waarden van de **client-id** en het **client geheim**. U hebt beide nodig om Google te configureren als een id-provider in uw Tenant. **Client geheim** is een belang rijke beveiligings referentie.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Een Google-account configureren als een id-provider

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
1. Zorg ervoor dat u de map met uw Azure AD B2C-Tenant gebruikt door het filter **Directory + abonnement** te selecteren in het bovenste menu en de map te kiezen die uw Tenant bevat.
1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Selecteer **id-providers**en selecteer vervolgens **Google**.
1. Voer een **naam**in. Bijvoorbeeld *Google*.
1. Voer voor de **client-id**de client-id in van de Google-toepassing die u eerder hebt gemaakt.
1. Voer voor het **client geheim**het client geheim in dat u hebt vastgelegd.
1. Selecteer **Opslaan**.
