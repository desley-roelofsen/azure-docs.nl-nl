---
title: 'Zelf studie: integratie Azure Active Directory met Shmoop voor scholen | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Shmoop voor scholen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1d75560a-55b3-42e9-bda1-92b01c572d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06ce2e51f72b9b4f709bf26ce6d92ad3861d36db
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2019
ms.locfileid: "74081650"
---
# <a name="tutorial-integrate-shmoop-for-schools-with-azure-active-directory"></a>Zelf studie: Shmoop voor scholen integreren met Azure Active Directory

In deze zelf studie leert u hoe u Shmoop voor scholen integreert met Azure Active Directory (Azure AD). Wanneer u Shmoop voor scholen integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot Shmoop voor scholen.
* Zorg ervoor dat uw gebruikers automatisch worden aangemeld bij Shmoop voor scholen met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* Shmoop for scholen (single sign-on) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

* Shmoop for scholen ondersteunt door **SP** GEÏNITIEERDe SSO
* Shmoop voor scholen ondersteunt **just-in-time** -gebruikers inrichting

## <a name="adding-shmoop-for-schools-from-the-gallery"></a>Shmoop voor scholen toevoegen vanuit de galerie

Als u de integratie van Shmoop voor scholen wilt configureren in azure AD, moet u Shmoop voor scholen vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** **Shmoop voor scholen** in het zoekvak.
1. Selecteer **Shmoop voor scholen** uit het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.

## <a name="configure-and-test-azure-ad-single-sign-on-for-shmoop-for-schools"></a>Eenmalige aanmelding voor Azure AD configureren en testen voor Shmoop voor scholen

Azure AD SSO met Shmoop voor scholen configureren en testen met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Shmoop voor scholen.

Als u Azure AD SSO wilt configureren en testen met Shmoop voor scholen, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
    * **[Een Azure AD-test gebruiker maken](#create-an-azure-ad-test-user)** : u kunt eenmalige aanmelding voor Azure AD testen met B. Simon.
    * **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe, zodat B. Simon de eenmalige aanmelding van Azure AD kan gebruiken.
2. **[Shmoop configureren voor scholen-SSO](#configure-shmoop-for-schools-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
    * **[Shmoop maken voor scholen test gebruiker](#create-shmoop-for-schools-test-user)** : als u een tegen hanger wilt hebben van B. Simon in Shmoop voor scholen die zijn gekoppeld aan de Azure AD-representatie van de gebruiker.
3. **[SSO testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in de [Azure Portal](https://portal.azure.com/)op de pagina **Shmoop for scholen** Application Integration de sectie **beheren** en selecteer **eenmalige aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://schools.shmoop.com/public-api/saml2/start/<uniqueid>`

    b. In het tekstvak **Id (entiteits-id)** typt u een URL met het volgende patroon: `https://schools.shmoop.com/<uniqueid>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en id. Neem contact op met [Shmoop for scholen client support team](mailto:support@shmoop.com) om deze waarden op te halen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Shmoop for scholen-toepassing verwacht de SAML-beweringen in een specifieke indeling. hiervoor moet u aangepaste kenmerk toewijzingen toevoegen aan de configuratie van uw SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven.

    ![installatiekopie](common/default-attributes.png)

    > [!NOTE]
    > Shmoop for school ondersteunt twee rollen voor gebruikers: **docent** en **student**. Stel deze rollen in azure AD in zodat gebruikers de juiste rollen kunnen toewijzen. Zie [hier](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management)voor meer informatie over het configureren van rollen in azure AD.

1. Daarnaast verwacht Shmoop voor scholen toepassing nog maar weinig kenmerken die kunnen worden door gegeven in de SAML-respons die hieronder worden weer gegeven. Deze kenmerken worden ook vooraf ingevuld, maar u kunt ze controleren volgens uw vereisten.

    | Naam |  Bronkenmerk|
    | --------- | --------------- |
    | role      | user.assignedroles |

1. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

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

In deze sectie schakelt u B. Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan Shmoop voor scholen.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer **Shmoop voor scholen**in de lijst toepassingen.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

## <a name="configure-shmoop-for-schools-sso"></a>Shmoop configureren voor scholen-SSO

Als u eenmalige aanmelding wilt configureren **voor Shmoop voor scholen** , moet u de URL voor de **federatieve meta gegevens** van de app verzenden naar [Shmoop voor scholen-ondersteunings team](mailto:support@shmoop.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-shmoop-for-schools-test-user"></a>Shmoop maken voor scholen test gebruiker

In deze sectie wordt een gebruiker met de naam B. Simon gemaakt in Shmoop voor scholen. Shmoop voor scholen ondersteunt just-in-time-gebruikers inrichting, die standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker niet al aanwezig is in Shmoop for scholen, wordt er na verificatie een nieuwe gemaakt.

> [!NOTE]
> Als u hand matig een gebruiker moet maken, neemt u contact op met het [ondersteunings team van Shmoop for scholen](mailto:support@shmoop.com).

## <a name="test-sso"></a>SSO testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel Shmoop voor scholen in het toegangs venster klikt, moet u automatisch worden aangemeld bij de Shmoop voor scholen waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Probeer Shmoop voor scholen met Azure AD](https://aad.portal.azure.com/)