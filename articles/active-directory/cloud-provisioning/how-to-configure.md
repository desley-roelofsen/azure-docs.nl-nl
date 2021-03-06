---
title: Nieuwe agent configuratie voor het inrichten van de Cloud Azure AD Connect
description: In dit artikel wordt beschreven hoe u Cloud inrichting installeert.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 853950cf441007eac0170f32f28f2d9c16a71292
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/31/2019
ms.locfileid: "75549428"
---
# <a name="azure-ad-connect-cloud-provisioning-new-agent-configuration"></a>Nieuwe agent configuratie voor het inrichten van de Cloud Azure AD Connect

Nadat u de agent hebt geïnstalleerd, moet u zich aanmelden bij de Azure Portal en Azure Active Directory (Azure AD) Connect Cloud Provisioning configureren. Volg deze stappen om de agent in te scha kelen.

## <a name="configure-provisioning"></a>Inrichting configureren
Volg deze stappen voor het configureren van de inrichting.

1.  Selecteer in de Azure-portal **Azure Active Directory**.
1.  Selecteer **Azure AD Connect**.
1.  Selecteer **inrichting beheren (preview)** .

    ![Inrichting beheren (preview-versie)](media/how-to-configure/manage1.png)

1.  Selecteer **nieuwe configuratie**.
1.  Op het scherm configuratie is het on-premises domein vooraf ingevuld.
1.  Voer een **e-mail melding**in. Deze e-mail wordt gewaarschuwd wanneer het inrichten niet in orde is.
1.  Verplaats de selector om deze in te **scha kelen**en selecteer **Opslaan**.

    ![Azure AD-inrichting (preview-versie)](media/tutorial-single-forest/configure2.png)

## <a name="scope-provisioning-to-specific-users-and-groups"></a>Scopeing inrichten voor specifieke gebruikers en groepen
U kunt het bereik van de agent voor het synchroniseren van specifieke gebruikers en groepen met behulp van on-premises Active Directory groepen of organisatie-eenheden. U kunt geen groepen en organisatie-eenheden configureren binnen een configuratie. 

1.  Selecteer in de Azure-portal **Azure Active Directory**.
1.  Selecteer **Azure AD Connect**.
1.  Selecteer **inrichting beheren (preview)** .
1.  Selecteer uw configuratie onder **configuratie**.

    ![Configuratie sectie](media/how-to-configure/scope1.png)

1.  Selecteer onder **configureren** **alle gebruikers** om het bereik van de configuratie regel te wijzigen.

    ![Optie alle gebruikers](media/how-to-configure/scope2.png)

1. Aan de rechter kant kunt u het bereik wijzigen zodat alleen beveiligings groepen worden opgenomen. Voer de DN-naam van de groep in en selecteer **toevoegen**.

    ![Optie geselecteerde beveiligings groepen](media/how-to-configure/scope3.png)

1.  Of u kunt het bereik wijzigen zodat alleen specifieke organisatie-eenheden worden opgenomen. Selecteer **gereed** en **Sla**het bestand op.

    ![Optie geselecteerde organisatie-eenheden](media/how-to-configure/scope4.png)


## <a name="restart-provisioning"></a>Inrichting opnieuw starten 
Als u niet wilt wachten op de volgende geplande uitvoering, moet u de inrichtings uitvoering activeren met behulp van de knop **inrichting opnieuw opstarten** . 
1.  Selecteer in de Azure-portal **Azure Active Directory**.
1.  Selecteer **Azure AD Connect**.
1.  Selecteer **inrichting beheren (preview)** .
1.  Selecteer uw configuratie onder **configuratie**.

    ![Configuratie selectie voor het opnieuw opstarten van de inrichting](media/how-to-configure/scope1.png)

1.  Selecteer aan de bovenkant de optie **inrichting opnieuw opstarten**.

## <a name="remove-a-configuration"></a>Een configuratie verwijderen
Voer de volgende stappen uit om een configuratie te verwijderen.

1.  Selecteer in de Azure-portal **Azure Active Directory**.
1.  Selecteer **Azure AD Connect**.
1.  Selecteer **inrichting beheren (preview)** .
1.  Selecteer uw configuratie onder **configuratie**.

    ![Configuratie selectie voor het verwijderen van de configuratie](media/how-to-configure/scope1.png)

1.  Selecteer boven aan het configuratie scherm de optie **verwijderen**.

    ![De knop Verwijderen](media/how-to-configure/remove1.png)

>[!IMPORTANT]
>Er is geen bevestiging voordat een configuratie wordt verwijderd. Zorg ervoor dat dit de actie is die u wilt uitvoeren voordat u **verwijderen**selecteert.


## <a name="next-steps"></a>Volgende stappen 

- [Wat is inrichten?](what-is-provisioning.md)
- [Wat is Azure AD Connect Cloud inrichting?](what-is-cloud-provisioning.md)
