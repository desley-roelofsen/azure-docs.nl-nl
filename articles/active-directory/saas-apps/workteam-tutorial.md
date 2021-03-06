---
title: 'Zelfstudie: Azure Active Directory-integratie met eenmalige aanmelding (SSO) met Workteam | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Workteam.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 41df17a1-ba69-414f-8ec3-11079b030df6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0b14c08604fcc0f6d2550127ca1f1aa053172b75
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2019
ms.locfileid: "72026731"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-workteam"></a>Zelfstudie: Azure Active Directory-integratie met eenmalige aanmelding (SSO) met Workteam

In deze zelf studie leert u hoe u Workteam integreert met Azure Active Directory (Azure AD). Wanneer u Workteam integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot Workteam.
* Zorg ervoor dat uw gebruikers automatisch worden aangemeld bij Workteam met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* Workteam-abonnement dat is ingeschakeld voor eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

* Workteam ondersteunt SSO die door **SP en IDP** is geïnitieerd

## <a name="adding-workteam-from-the-gallery"></a>Workteam toevoegen uit de galerie

Als u de integratie van Workteam in azure AD wilt configureren, moet u Workteam uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** **Workteam** in het zoekvak.
1. Selecteer **Workteam** uit het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.

## <a name="configure-and-test-azure-ad-single-sign-on-for-workteam"></a>Eenmalige aanmelding voor Azure AD configureren en testen voor Workteam

Azure AD SSO met Workteam configureren en testen met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Workteam.

Als u Azure AD SSO wilt configureren en testen met Workteam, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
    1. **[Een Azure AD-test gebruiker maken](#create-an-azure-ad-test-user)** : u kunt eenmalige aanmelding voor Azure AD testen met B. Simon.
    1. **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe, zodat B. Simon de eenmalige aanmelding van Azure AD kan gebruiken.
1. **[WORKTEAM SSO configureren](#configure-workteam-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
    1. **[Maak een Workteam-test gebruiker](#create-workteam-test-user)** -om een equivalent van B. Simon in Workteam te hebben dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
1. **[SSO testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in het [Azure Portal](https://portal.azure.com/)op de pagina Toepassings integratie van **Workteam** de sectie **beheren** en selecteer **eenmalige aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In het gedeelte **basis configuratie van SAML** is de toepassing vooraf geconfigureerd in de gestarte modus **IDP** en de benodigde url's zijn al vooraf ingevuld met Azure. De gebruiker moet de configuratie opslaan door te klikken op de knop **Opslaan** .

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://app.workte.am`

1. Zoek op de pagina **eenmalige aanmelding met SAML instellen** , in de sectie **SAML-handtekening certificaat** , naar **certificaat (base64)** en selecteer **downloaden** om het certificaat te downloaden en op uw computer op te slaan.

    ![De downloadkoppeling certificaat](common/certificatebase64.png)

1. Op de sectie **Workteam instellen** kopieert u de gewenste URL ('s) op basis van uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt u een test gebruiker in de Azure Portal met de naam B. Simon.

1. Selecteer in het linkerdeel venster van de Azure Portal **Azure Active Directory**, selecteer **gebruikers**en selecteer vervolgens **alle gebruikers**.
1. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.
1. Voer de volgende stappen uit in de eigenschappen van de **gebruiker** :
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. Voer in het veld **gebruikers naam** de username@companydomain.extension in. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan Workteam.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst toepassingen de optie **Workteam**.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="configure-workteam-sso"></a>Workteam SSO configureren

1. Als u de configuratie wilt automatiseren in Workteam, moet u de **uitbrei ding mijn apps Secure Sign-in browser** installeren door te klikken op **de uitbrei ding installeren**.

    ![Uitbrei ding voor mijn apps](common/install-myappssecure-extension.png)

2. Nadat u de extensie aan de browser hebt toegevoegd, klikt u op **Setup Workteam** gaat u naar de Workteam-toepassing. Geef de beheerders referenties op om u aan te melden bij Workteam. Met de browser uitbreiding wordt de toepassing automatisch voor u geconfigureerd en wordt stap 3-6 geautomatiseerd.

    ![Configuratie van Setup](common/setup-sso.png)

3. Als u Workteam hand matig wilt instellen, opent u een nieuw webbrowser venster en meldt u zich aan bij uw Workteam-bedrijfs site als beheerder en voert u de volgende stappen uit:

4. Klik in de rechter bovenhoek op **profiel logo** en klik vervolgens op **organisatie-instellingen**. 

    ![Workteam-instellingen](./media/workteam-tutorial/tutorial_workteam_settings.png)

5. Klik onder **verificatie** op **instellingen logo**.

     ![Azure Workteam](./media/workteam-tutorial/tutorial_workteam_azure.png)

6. Voer de volgende stappen uit op de pagina **SAML-instellingen** :

     ![Workteam SAML](./media/workteam-tutorial/tutorial_workteam_saml.png)

    a. Selecteer **SAML IDP** als **ad Azure**.

    b. Plak in het tekstvak **SAML-URL voor eenmalige aanmelding** de waarde van de **aanmeldings-URL**die u hebt gekopieerd van de Azure Portal.

    c. Plak in het tekstvak **id van SAML-entiteit** de waarde van de **Azure ad-id**, die u hebt gekopieerd van de Azure Portal.

    d. Open in Klad blok het door **Base-64 gecodeerde certificaat** dat u hebt gedownload van de Azure Portal, kopieer de inhoud en plak deze in het vak **SAML-handtekening certificaat (base64)** .

    e. Klik op **OK**.

### <a name="create-workteam-test-user"></a>Workteam-test gebruiker maken

Om ervoor te zorgen dat Azure AD-gebruikers zich kunnen aanmelden bij Workteam, moeten ze worden ingericht in Workteam. In Workteam is inrichten een hand matige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij Workteam als een beveiligings beheerder.

2. Klik bovenaan op de pagina **organisatie-instellingen** op **gebruikers** en klik vervolgens op **nieuwe gebruiker**.

    ![Workteam-gebruiker](./media/workteam-tutorial/tutorial_workteam_user.png)

3. Voer op de pagina **nieuwe werk nemer** de volgende stappen uit:

    ![Workteam nieuwe gebruiker](./media/workteam-tutorial/tutorial_workteam_newuser.png)

    a. Voer in het tekstvak **naam** de voor naam van de gebruiker in zoals **B. Simon**.

    b. Voer in het tekstvak **e-mail** het e-mail adres van de gebruiker in, zoals `B.Simon\@contoso.com`.

    c. Klik op **OK**.

## <a name="test-sso"></a>SSO testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Workteam in het toegangs venster klikt, moet u automatisch worden aangemeld bij de Workteam waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Probeer Workteam met Azure AD](https://aad.portal.azure.com/)

