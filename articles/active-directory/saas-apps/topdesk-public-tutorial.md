---
title: 'Zelfstudie: Azure Active Directory integratie met TOPdesk-Public | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TOPdesk-openbaar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: jeedes
ms.openlocfilehash: e5575a2e8f776e87fcd4e6f4a7a9244752ebfd9a
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2019
ms.locfileid: "71950412"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Zelfstudie: Integratie met TOPdesk-openbaar Azure Active Directory

In deze zelf studie leert u hoe u TOPdesk-Public integreert met Azure Active Directory (Azure AD).
Het integreren van TOPdesk-Public met Azure AD biedt de volgende voor delen:

* U kunt beheren in azure AD die toegang heeft tot TOPdesk-openbaar.
* U kunt ervoor zorgen dat uw gebruikers automatisch worden aangemeld bij TOPdesk-openbaar (eenmalige aanmelding) met hun Azure AD-accounts.
* U kunt uw accounts in één centrale locatie - Azure portal beheren.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met TOPdesk-Public wilt configureren, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* TOPdesk-openbaar abonnement voor eenmalige aanmelding

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* TOPdesk-openbaar ondersteunt door **SP** GEÏNITIEERDe SSO

## <a name="adding-topdesk---public-from-the-gallery"></a>TOPdesk-openbaar toevoegen vanuit de galerie

Als u de integratie van TOPdesk-Public in azure AD wilt configureren, moet u TOPdesk-Public toevoegen vanuit de galerie aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om TOPdesk-Public toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **TopDesk-Public**, selecteer **TopDesk-Public** uit het deel venster result en klik vervolgens op de knop **toevoegen** om de toepassing toe te voegen.

     ![TOPdesk-openbaar in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u eenmalige aanmelding voor Azure AD configureren en testen met TOPdesk-Public op basis van een test gebruiker met de naam **Julia Simon**.
Voor een goede werking van eenmalige aanmelding moet een koppelings relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in TOPdesk-openbaar worden gemaakt.

Als u eenmalige aanmelding voor Azure AD wilt configureren en testen met TOPdesk-Public, moet u de volgende bouw stenen volt ooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[TopDesk-open bare eenmalige aanmelding configureren](#configure-topdesk---public-single-sign-on)** : Hiermee configureert u de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak een TopDesk-gebruiker voor een open bare test](#create-topdesk---public-test-user)** , zodat deze een soort is van Julia Simon in TopDesk-openbaar dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure AD te configureren met TOPdesk-Public:

1. Selecteer in de [Azure Portal](https://portal.azure.com/)op de pagina **TopDesk-open bare** toepassings integratie de optie **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4.  Voer in de sectie **Standaard SAML-configuratie** de volgende stappen uit als u beschikt over een **bestand met metagegevens van de serviceprovider**:

    >[!NOTE]
    >U krijgt het **META gegevensbestand van de service provider** in het gedeelte **TopDesk-open bare eenmalige aanmelding configureren** , dat verderop in de zelf studie wordt beschreven.

    a. Klik op **Metagegevensbestand uploaden**.
    
    ![Metagegevensbestand uploaden](common/upload-metadata.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![Metagegevensbestand kiezen](common/browse-upload-metadata.png)

    c. Nadat het meta gegevensbestand is geüpload, worden de waarden voor de **id** en de **antwoord-URL** automatisch ingevuld in de basis configuratie sectie voor SAML.

    ![TOPdesk: informatie over eenmalige aanmelding voor open bare domeinen en Url's](common/sp-identifier-reply.png)

    d. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<companyname>.topdesk.net`

    e. In het tekstvak **ID-URL** vult u de URL van de TopDesk-meta gegevens in die u kunt ophalen uit de TopDesk-configuratie. Het moet het volgende patroon gebruiken: `https://<companyname>.topdesk.net/saml-metadata/<identifier>`
    
    f. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.topdesk.net/tas/public/login/verify`
    
    > [!NOTE] 
    > Als de **id** -en **antwoord-URL** -waarden niet automatisch worden ingevuld, moet u deze hand matig invoeren. Voor id volgt u het patroon zoals hierboven wordt vermeld en krijgt u de antwoord-URL-waarde vanuit de sectie **TopDesk-open bare eenmalige aanmelding configureren** , die verderop in de zelf studie wordt uitgelegd. De waarde voor de **aanmeldings-URL** is niet echt, dus u moet de waarde bijwerken met de werkelijke AANMELDINGS-URL. Neem contact op met het [ondersteunings team van TopDesk-Public client](https://help.topdesk.com/saas/enterprise/user/) om de waarde op te halen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De downloadkoppeling certificaat](common/metadataxml.png)

6. Kopieer op de sectie **TopDesk-Public instellen** de gewenste URL ('s) volgens uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-topdesk---public-single-sign-on"></a>TOPdesk-open bare eenmalige aanmelding configureren

1. Meld u aan bij uw **TopDesk-open bare** bedrijfs site als beheerder.

2. Klik in het menu van **TOPdesk** op **Settings**.
   
    ![Settings](./media/topdesk-public-tutorial/ic790598.png "Settings")

3. Klik op **Login Settings**.
   
    ![Login Settings](./media/topdesk-public-tutorial/ic790599.png "Login Settings")

4. Vouw het menu **Login Settings** uit en klik op **General**.
   
    ![General](./media/topdesk-public-tutorial/ic790600.png "General")

5. Voer de volgende stappen uit in de sectie **openbaar** van de **SAML-aanmeld** configuratie:
   
    ![Technical Settings](./media/topdesk-public-tutorial/ic790601.png "Technical Settings")
   
    a. Klik op **Downloaden** om het openbare metagegevensbestand te downloaden en sla het lokaal op uw computer op.
   
    b. Open het gedownloade meta gegevensbestand en zoek het knoop punt **AssertionConsumerService** .

    ![AssertionConsumerService](./media/topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopieer de waarde **AssertionConsumerService** en plak deze waarde in het tekstvak **antwoord-URL** in de sectie basis-SAML- **configuratie** .      
   
6. Als u een certificaatbestand wilt maken, moet u de volgende stappen uitvoeren:
    
    ![Certificaat](./media/topdesk-public-tutorial/ic790606.png "Certificaat")
    
    a. Open het gedownloade metagegevensbestand vanuit de Azure-portal.
    
    b. Vouw het knooppunt **RoleDescriptor** uit waarvan **xsi:type** is ingesteld op **fed:ApplicationServiceType**.
    
    c. Kopieer de waarde van het knooppunt **X509Certificate**.
    
    d. Sla de gekopieerde waarde van **X509Certificate** lokaal op uw computer op in een bestand.

7. Klik in de sectie **Public** op **Add**.
    
    ![SAML-aanmelding](./media/topdesk-public-tutorial/ic790625.png "SAML-aanmelding")

8. Voer de volgende stappen uit in het dialoogvenster **SAML configuration assistant**:
    
    ![SAML Configuration Assistant](./media/topdesk-public-tutorial/ic790608.png "SAML Configuration Assistant")
    
    a. Klik onder **Federatieve metagegevens** op **Bladeren** om het gedownloade metagegevensbestand te uploaden vanuit de Azure-portal.

    b. Klik onder **Certificaat (RSA)** op **Bladeren** om het certificaatbestand te uploaden.

    c. Klik onder het **Logo-pictogram** op **Bladeren** om het logobestand dat u van het TOPdesk-ondersteuningsteam hebt verkregen, te uploaden.

    d. In het tekstvak voor het **gebruikersnaamkenmerk** typt u `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. Typ in het tekstvak **Weergavenaam** een naam voor de configuratie.

    f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. Typ brittasimon@yourcompanydomain.extension in het veld **gebruikers naam** . Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Julia Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan TOPdesk-public.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **TopDesk-Public**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **TopDesk-Public**.

    ![De TOPdesk-open bare koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-topdesk---public-test-user"></a>TOPdesk maken-open bare test gebruiker

Om ervoor te zorgen dat Azure AD-gebruikers zich kunnen aanmelden bij TOPdesk-openbaar, moeten ze worden ingericht in TOPdesk-openbaar. In het geval van TOPdesk-Public is inrichting een hand matige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voer de volgende stappen uit om de inrichting van gebruikers te configureren:

1. Meld u aan bij uw **TopDesk-open bare** bedrijfs site als beheerder.

2. Klik in het menu aan de bovenkant op **TOPdesk \> nieuwe \> ondersteunings bestanden \> persoon**.
   
    ![Person](./media/topdesk-public-tutorial/ic790628.png "Person")

3. Voer de volgende stappen uit in het dialoog venster nieuwe persoon:
   
    ![Nieuwe persoon]nieuwe(./media/topdesk-public-tutorial/ic790629.png "persoon")
   
    a. Klik op het tabblad Algemeen.

    b. Typ in het tekstvak **Achternaam** de tekst achternaam van de gebruiker, zoals Simon
 
    c. Selecteer een **site** voor het account.
 
    d. Klik op **Opslaan**.

> [!NOTE]
> U kunt alle andere TOPdesk-toepassingen voor het maken van open bare gebruikers accounts of Api's die worden geleverd door TOPdesk-Public gebruiken om Azure AD-gebruikers accounts in te richten.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel TOPdesk-Public in het toegangs venster klikt, moet u automatisch worden aangemeld bij de TOPdesk-Public waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
