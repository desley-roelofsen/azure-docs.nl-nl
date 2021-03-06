---
title: 'Zelf studie: Azure Active Directory de integratie van eenmalige aanmelding (SSO) met Box | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Box.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3b565c8d-35e2-482a-b2f4-bf8fd7d8731f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: aceec8ad6ca23a1bd89b93a6442d48aa81597e55
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74964171"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-box"></a>Zelf studie: Azure Active Directory-integratie met eenmalige aanmelding (SSO) met Box

In deze zelf studie leert u hoe u Box integreert met Azure Active Directory (Azure AD). Wanneer u Box integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot box.
* Stel uw gebruikers in staat om automatisch te worden aangemeld bij een box met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* Box SSO (single sign-on) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

* Box ondersteunt door **SP** geïnitieerde eenmalige aanmelding
* Biedt ondersteuning voor [ **automatische** gebruikers inrichting en](https://docs.microsoft.com/azure/active-directory/saas-apps/box-userprovisioning-tutorial) het ongedaan maken van de inrichting (aanbevolen)
* Box ondersteunt het **Just-In-Time** inrichten van gebruikers

## <a name="adding-box-from-the-gallery"></a>Box toevoegen uit de galerie

Voor het configureren van de integratie van Box in Azure AD, moet u Box uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** **vak** in het zoekvak.
1. Selecteer **vak** in het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.

## <a name="configure-and-test-azure-ad-single-sign-on-for-box"></a>Eenmalige aanmelding voor Azure AD voor Box configureren en testen

Configureer en test Azure AD SSO met box met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in box.

Als u Azure AD SSO met Box wilt configureren en testen, voert u de volgende bouw stenen uit:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
    1. **[Een Azure AD-test gebruiker maken](#create-an-azure-ad-test-user)** : u kunt eenmalige aanmelding voor Azure AD testen met B. Simon.
    1. **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe, zodat B. Simon de eenmalige aanmelding van Azure AD kan gebruiken.
1. **[Box-SSO configureren](#configure-box-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
    1. Een **[test gebruiker voor een box maken](#create-box-test-user)** -een equivalent van B. Simon hebben in box dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
1. **[SSO testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Ga in het [Azure Portal](https://portal.azure.com/)naar de pagina Toepassings integratie van het **vak** en selecteer de sectie voor het **beheren** van **eenmalige aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Voer in de sectie **basis configuratie van SAML** de waarden in voor de volgende velden:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.account.box.com`

    b. Typ een URL in het vak **Id (Entiteits-id)** : `box.net`

    > [!NOTE]
    > De waarde voor de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met het [Box-Client-ondersteuningsteam](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) om de waarde te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Zoek op de pagina **eenmalige aanmelding met SAML instellen** , in de sectie **SAML-handtekening certificaat** , de **federatieve meta gegevens-XML** en selecteer **downloaden** om het certificaat te downloaden en op uw computer op te slaan.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een test gebruiker in de Azure Portal met de naam B. Simon.

1. Selecteer in het linkerdeel venster van de Azure Portal **Azure Active Directory**, selecteer **gebruikers**en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. Voer de volgende stappen uit in de eigenschappen van de **gebruiker** :
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. Voer in het veld **gebruikers naam** de username@companydomain.extensionin. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon in om Azure eenmalige aanmelding te gebruiken door toegang te verlenen aan box.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst toepassingen de optie **Box**.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

## <a name="configure-box-sso"></a>Box SSO configureren

Volg de procedure in [Zelfstandig eenmalige aanmelding instellen](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown) voor meer informatie over het configureren van een eenmalige aanmelding voor uw toepassing.

> [!NOTE]
> Als u de instellingen voor eenmalige aanmelding voor uw Box-account niet kunt configureren, moet u de gedownloade **federatieve metagegevens-XML** verzenden naar het [Box-ondersteuningsteam](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-box-test-user"></a>Testgebruiker voor Box maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Box. Box biedt ondersteuning voor Just-In-Time-inrichting van gebruikers. Deze functie is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Als er nog geen gebruiker in Box bestaat, wordt er een nieuwe gemaakt nadat deze is geverifieerd.

> [!NOTE]
> Als u een gebruiker handmatig wilt maken, neem dan contact op met het [Box-ondersteuningsteam](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire).

## <a name="test-sso"></a>SSO testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel Box in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van Box waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Vak uitproberen met Azure AD](https://aad.portal.azure.com/)
