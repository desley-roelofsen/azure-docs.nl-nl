---
title: Een RHEL-VM toevoegen aan Azure AD Domain Services | Microsoft Docs
description: Meer informatie over het configureren en toevoegen van een Red Hat Enterprise Linux virtuele machine aan een Azure AD Domain Services beheerd domein.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 16100caa-f209-4cb0-86d3-9e218aeb51c6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 09/15/2019
ms.author: iainfou
ms.openlocfilehash: 9472abd7a16c887a796e36b8190e8530c84dafa9
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2019
ms.locfileid: "72755714"
---
# <a name="join-a-red-hat-enterprise-linux-virtual-machine-to-an-azure-ad-domain-services-managed-domain"></a>Een virtuele Red Hat Enterprise Linux-computer toevoegen aan een beheerd domein van Azure AD Domain Services

Als u wilt dat gebruikers zich met één set referenties aanmelden bij virtuele machines (Vm's) in azure, kunt u Vm's toevoegen aan een door Azure Active Directory Domain Services (AD DS) beheerd domein. Wanneer u een virtuele machine koppelt aan een door Azure AD DS beheerd domein, kunnen gebruikers accounts en referenties van het domein worden gebruikt voor het aanmelden en beheren van servers. Groepslid maatschappen van het door Azure AD DS beheerde domein worden ook toegepast om u de toegang tot bestanden of services op de virtuele machine te kunnen beheren.

In dit artikel wordt beschreven hoe u een Red Hat Enterprise Linux-VM (RHEL) kunt toevoegen aan een beheerd domein van Azure AD DS.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende resources en bevoegdheden nodig om deze zelf studie te volt ooien:

* Een actief Azure-abonnement.
    * Als u geen Azure-abonnement hebt, [maakt u een account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Een Azure Active Directory Tenant die aan uw abonnement is gekoppeld, gesynchroniseerd met een on-premises Directory of een alleen-Cloud Directory.
    * Als dat nodig is, [maakt u een Azure Active Directory-Tenant][create-azure-ad-tenant] of [koppelt u een Azure-abonnement aan uw account][associate-azure-ad-tenant].
* Een Azure Active Directory Domain Services beheerd domein ingeschakeld en geconfigureerd in uw Azure AD-Tenant.
    * Als dat nodig is, [maakt en configureert][create-azure-ad-ds-instance]de eerste zelf studie een Azure Active Directory Domain Services-exemplaar.
* Een gebruikers account dat lid is van de groep *Azure AD DC-Administrators* in uw Azure AD-Tenant.

## <a name="create-and-connect-to-a-rhel-linux-vm"></a>Maken en verbinding maken met een RHEL Linux-VM

Als u een bestaande virtuele machine met RHEL Linux in azure hebt, kunt u er verbinding mee maken via SSH. Ga vervolgens verder met de volgende stap om te beginnen met het [configureren van de virtuele machine](#configure-the-hosts-file).

Als u een virtuele machine met RHEL Linux wilt maken of een test-VM wilt maken voor gebruik met dit artikel, kunt u een van de volgende methoden gebruiken:

* [Azure-portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Wanneer u de virtuele machine maakt, moet u aandacht best Eden aan de instellingen voor virtueel netwerk om ervoor te zorgen dat de virtuele machine kan communiceren met het door Azure AD DS beheerde domein:

* Implementeer de virtuele machine in dezelfde of een peered virtueel netwerk waarin u Azure AD Domain Services hebt ingeschakeld.
* Implementeer de virtuele machine in een ander subnet dan uw Azure AD Domain Services-exemplaar.

Wanneer de VM is geïmplementeerd, volgt u de stappen om via SSH verbinding te maken met de VM.

## <a name="configure-the-hosts-file"></a>Het hosts-bestand configureren

Om ervoor te zorgen dat de hostnaam van de virtuele machine correct is geconfigureerd voor het beheerde domein, bewerkt u het bestand *bestand/etc/hosts* en stelt u de hostnaam in:

```console
sudo vi /etc/hosts
```

Werk in het bestand *hosts* het *localhost* -adres bij. In het volgende voor beeld:

* *contoso.com* is de DNS-domein naam van uw door Azure AD DS beheerde domein.
* *RHEL* is de hostnaam van de RHEL-VM die u aan het beheerde domein toevoegt.

Werk deze namen bij met uw eigen waarden:

```console
127.0.0.1 rhel rhel.contoso.com
```

Als u klaar bent, slaat u het *hosts* -bestand op en sluit u het af met de `:wq` opdracht van de editor.

## <a name="install-required-packages"></a>De vereiste pakketten installeren

De VM moet enkele extra pakketten hebben om de virtuele machine toe te voegen aan het door Azure AD DS beheerde domein. Als u deze pakketten wilt installeren en configureren, moet u de hulpprogram ma's voor domein deelname bijwerken en installeren met behulp van `yum`:

 **RHEL 7** 

```console
sudo yum install realmd sssd krb5-workstation krb5-libs oddjob oddjob-mkhomedir samba-common-tools
```

 **RHEL 6** 

```console
sudo yum install adcli sssd authconfig krb5-workstation
```

## <a name="join-vm-to-the-managed-domain"></a>Virtuele machine toevoegen aan het beheerde domein

Nu de vereiste pakketten zijn geïnstalleerd op de VM, voegt u de virtuele machine toe aan het beheerde domein van Azure AD DS.
 
  **RHEL 7**
     
1. Gebruik de `realm discover` opdracht om het door Azure AD DS beheerde domein te detecteren. In het volgende voor beeld wordt de realm- *CONTOSO.com*gedetecteerd. Geef in alle hoofd letters uw eigen Azure AD DS beheerde domein naam op:

    ```console
    sudo realm discover CONTOSO.COM
    ```

   Als de `realm discover` opdracht uw door Azure AD DS beheerde domein niet kan vinden, raadpleegt u de volgende stappen voor probleem oplossing:
   
    * Zorg ervoor dat het domein bereikbaar is vanaf de VM. Probeer `ping contoso.com` om te zien of een positief antwoord wordt geretourneerd.
    * Controleer of de virtuele machine is geïmplementeerd op hetzelfde of een peered virtueel netwerk waarin het beheerde domein van Azure AD DS beschikbaar is.
    * Controleer of de DNS-server instellingen voor het virtuele netwerk zijn bijgewerkt zodat ze verwijzen naar de domein controllers van het door Azure AD DS beheerde domein.

1. Initialiseer nu Kerberos met de opdracht `kinit`. Geef een gebruiker op die deel uitmaakt van de groep *Aad DC-Administrators* . Voeg, indien nodig, [een gebruikers account toe aan een groep in azure AD](../active-directory/fundamentals/active-directory-groups-members-azure-portal.md).

    De Azure AD DS Managed domain name moet in alle hoofd letters worden ingevoerd. In het volgende voor beeld wordt het account met de naam `contosoadmin@contoso.com` gebruikt voor het initialiseren van Kerberos. Voer uw eigen gebruikers account in dat lid is van de groep *Aad DC-Administrators* :
    
    ```console
    kinit contosoadmin@CONTOSO.COM
    ``` 

1. Voeg ten slotte de computer toe aan het beheerde domein van Azure AD DS met behulp van de `realm join` opdracht. Gebruik hetzelfde gebruikers account dat lid is van de groep *Aad DC Administrators* die u hebt opgegeven in de vorige `kinit` opdracht, zoals `contosoadmin@CONTOSO.COM`:

    ```console
    sudo realm join --verbose CONTOSO.COM -U 'contosoadmin@CONTOSO.COM'
    ```

Het kan even duren voordat de virtuele machine wordt toegevoegd aan het beheerde domein van Azure AD DS. In de volgende voorbeeld uitvoer ziet u dat de virtuele machine is toegevoegd aan het door Azure AD DS beheerde domein:

```output
Successfully enrolled machine in realm
```

  **RHEL 6** 

1. Gebruik de `adcli info` opdracht om het door Azure AD DS beheerde domein te detecteren. In het volgende voor beeld wordt de realm- *CONTOSO.com*gedetecteerd. Geef in alle hoofd letters uw eigen Azure AD DS beheerde domein naam op:

    ```console
    sudo adcli info contoso.com
    ```
    
   Als de `adcli info` opdracht uw door Azure AD DS beheerde domein niet kan vinden, raadpleegt u de volgende stappen voor probleem oplossing:
   
    * Zorg ervoor dat het domein bereikbaar is vanaf de VM. Probeer `ping contoso.com` om te zien of een positief antwoord wordt geretourneerd.
    * Controleer of de virtuele machine is geïmplementeerd op hetzelfde of een peered virtueel netwerk waarin het beheerde domein van Azure AD DS beschikbaar is.
    * Controleer of de DNS-server instellingen voor het virtuele netwerk zijn bijgewerkt zodat ze verwijzen naar de domein controllers van het door Azure AD DS beheerde domein.

1. U moet eerst lid worden van het domein met behulp van de `adcli join`-opdracht. met deze opdracht wordt ook de keytab gemaakt om de computer te verifiëren. Gebruik een gebruikers account dat lid is van de groep *Aad DC-Administrators* . 

    ```console
    sudo adcli join contoso.com -U contosoadmin
    ```

1. Configureer nu de `/ect/krb5.conf` en maak de `/etc/sssd/sssd.conf` bestanden om het domein `contoso.com` Active Directory te gebruiken. 
   Zorg ervoor dat `CONTOSO.COM` wordt vervangen door uw eigen domein naam:

    Open het `/ect/krb5.conf`-bestand met een editor:

    ```console
    sudo vi /etc/krb5.conf
    ```

    Werk het `krb5.conf`-bestand bij zodat dit overeenkomt met het volgende voor beeld:

    ```console
    [logging]
     default = FILE:/var/log/krb5libs.log
     kdc = FILE:/var/log/krb5kdc.log
     admin_server = FILE:/var/log/kadmind.log
    
    [libdefaults]
     default_realm = CONTOSO.COM
     dns_lookup_realm = true
     dns_lookup_kdc = true
     ticket_lifetime = 24h
     renew_lifetime = 7d
     forwardable = true
    
    [realms]
     CONTOSO.COM = {
     kdc = CONTOSO.COM
     admin_server = CONTOSO.COM
     }
    
    [domain_realm]
     .CONTOSO.COM = CONTOSO.COM
     CONTOSO.COM = CONTOSO.COM
    ```
    
   Maak het `/etc/sssd/sssd.conf`-bestand:
    
    ```console
    sudo vi /etc/sssd/sssd.conf
    ```

    Werk het `sssd.conf`-bestand bij zodat dit overeenkomt met het volgende voor beeld:

    ```console
    [sssd]
     services = nss, pam, ssh, autofs
     config_file_version = 2
     domains = CONTOSO.COM
    
    [domain/CONTOSO.COM]
    
     id_provider = ad
    ```

1. Zorg ervoor dat `/etc/sssd/sssd.conf` machtigingen 600 zijn en eigendom zijn van de hoofd gebruiker:

    ```console
    sudo chmod 600 /etc/sssd/sssd.conf
    sudo chown root:root /etc/sssd/sssd.conf
    ```

1. Gebruik `authconfig` om de VM te instrueren over de integratie van AD Linux:

    ```console
    sudo authconfig --enablesssd --enablesssdauth --update
    ```
    
1. Start de SSSD-service en schakel deze in:

    ```console
    sudo service sssd start
    sudo chkconfig sssd on
    ```

Als uw virtuele machine het proces voor domein deelname niet kan volt ooien, moet u ervoor zorgen dat door de netwerk beveiligings groep van de virtuele machine uitgaand Kerberos-verkeer op TCP + UDP-poort 464 naar het subnet van het virtuele netwerk voor uw Azure AD DS beheerde domein is toegestaan.

Controleer nu of u informatie over gebruikers-AD kunt opvragen met `getent`

```console
sudo getent passwd contosoadmin
```

## <a name="allow-password-authentication-for-ssh"></a>Wachtwoord verificatie voor SSH toestaan

Standaard kunnen gebruikers zich alleen aanmelden bij een VM met behulp van open bare SSH-sleutel verificatie. Verificatie op basis van wacht woorden mislukt. Wanneer u de virtuele machine koppelt aan een door Azure AD DS beheerd domein, moeten die domein accounts gebruikmaken van verificatie op basis van wacht woorden. Werk de SSH-configuratie als volgt bij om verificatie op basis van wacht woorden toe te staan.

1. Open het *sshd_conf* -bestand met een editor:

    ```console
    sudo vi /etc/ssh/sshd_config
    ```

1. Werk de regel voor *PasswordAuthentication* bij naar *Ja*:

    ```console
    PasswordAuthentication yes
    ```

    Als u klaar bent, slaat u het *sshd_conf* -bestand op en sluit u het af met de `:wq` opdracht van de editor.

1. Als u de wijzigingen wilt Toep assen en gebruikers wilt laten aanmelden met een wacht woord, start u de SSH-service opnieuw:

   **RHEL 7** 
    
    ```console
    sudo systemctl restart sshd
    ```

   **RHEL 6** 
    
    ```console
    sudo service sshd restart
    ```

## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>De groep ' AAD DC Administrators ' sudo privileges verlenen

Als u leden van de groep *Aad DC-Administrators* wilt toewijzen aan de RHEL-VM, voegt u een vermelding toe aan de */etc/sudoers*. Eenmaal toegevoegd, kunnen leden van de groep *Aad DC-Administrators* de `sudo`-opdracht gebruiken op de RHEL-VM.

1. Open het *sudo* -bestand om het te bewerken:

    ```console
    sudo visudo
    ```

1. Voeg de volgende vermelding toe aan het einde van het */etc/sudoers* -bestand. De groep *Aad DC-Administrators* bevat witruimte in de naam, dus neem het escape-teken van de back slash op in de groeps naam. Voeg uw eigen domein naam toe, zoals *contoso.com*:

    ```console
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators@contoso.com ALL=(ALL) NOPASSWD:ALL
    ```

    Als u klaar bent, slaat u de editor op en sluit u deze met behulp van de `:wq` opdracht van de editor.

## <a name="sign-in-to-the-vm-using-a-domain-account"></a>Aanmelden bij de virtuele machine met behulp van een domein account

Start een nieuwe SSH-verbinding met een domein gebruikers account om te controleren of de virtuele machine is toegevoegd aan het beheerde Azure AD DS-domein. Bevestig dat er een basismap is gemaakt en dat groepslid maatschap van het domein wordt toegepast.

1. Maak een nieuwe SSH-verbinding vanuit uw-console. Gebruik een domein account dat deel uitmaakt van het beheerde domein met behulp van de opdracht `ssh -l`, zoals `contosoadmin@contoso.com` en voer vervolgens het adres van uw virtuele machine in, bijvoorbeeld *RHEL.contoso.com*. Als u de Azure Cloud Shell gebruikt, gebruikt u het open bare IP-adres van de virtuele machine in plaats van de interne DNS-naam.

    ```console
    ssh -l contosoadmin@CONTOSO.com rhel.contoso.com
    ```

1. Wanneer u verbinding hebt gemaakt met de virtuele machine, controleert u of de basismap correct is geïnitialiseerd:

    ```console
    pwd
    ```

    U moet zich in de */Home* -map met uw eigen map bekomen die overeenkomt met het gebruikers account.

1. Controleer nu of de groepslid maatschappen correct worden omgezet:

    ```console
    id
    ```

    U moet uw groepslid maatschappen van het door Azure AD DS beheerde domein zien.

1. Als u zich bij de VM hebt aangemeld als lid van de groep *Aad DC-Administrators* , controleert u of u de `sudo` opdracht kunt gebruiken:

    ```console
    sudo yum update
    ```

## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt met het verbinden van de virtuele machine met de Azure AD DS beheerde domein of als u zich aanmeldt met een domein account, raadpleegt u problemen [met domein deelname oplossen](join-windows-vm.md#troubleshoot-domain-join-issues).

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
