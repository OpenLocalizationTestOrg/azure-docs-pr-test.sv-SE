---
title: "Azure Active Directory Domain Services: Anslut en virtuell CoreOS Linux-dator till en hanterad domän | Microsoft Docs"
description: Anslut en virtuell CoreOS Linux-dator till Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: maheshu
ms.openlocfilehash: 790ad85df0dbf68674e2b9c6254858100ddfd0fd
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="join-a-coreos-linux-virtual-machine-to-a-managed-domain"></a>Anslut en virtuell CoreOS Linux-dator till en hanterad domän
Den här artikeln visar hur du ansluter till en virtuell CoreOS Linux-dator i Azure till en hanterad Azure AD DS-domän.

## <a name="before-you-begin"></a>Innan du börjar
Om du vill utföra åtgärderna i den här artikeln behöver du:
1. En giltig **Azure-prenumeration**.
2. En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.
3. **Azure AD Domain Services** måste vara aktiverat för Azure AD-katalog. Om du inte gjort det, följer du de uppgifter som beskrivs i den [Kom igång-guiden](active-directory-ds-getting-started.md).
4. Se till att du har konfigurerat IP-adresserna för den hanterade domänen som DNS-servrar för det virtuella nätverket. Mer information finns i [hur du uppdaterar DNS-inställningarna för virtuella Azure-nätverket](active-directory-ds-getting-started-dns.md)
5. Slutför steg som krävs för att [synkronisera lösenord till din Azure AD Domain Services-hanterad domän](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-coreos-linux-virtual-machine"></a>Etablera en virtuell CoreOS Linux-dator
Etablera en virtuell CoreOS virtuell dator i Azure, med någon av följande metoder:
* [Azure Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Den här artikeln används den **CoreOS Linux (stabil)** avbildning av virtuell dator i Azure.

> [!IMPORTANT]
> * Distribuera den virtuella datorn till den **samma virtuella nätverk som du har aktiverat Azure AD Domain Services**.
> * Välj en **annat undernät** än det som du har aktiverat Azure AD Domain Services.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Fjärransluta till den nyetablerade virtuella Linux-datorn
Virtuell CoreOS virtuella datorn har etablerats i Azure. Nästa uppgift är att fjärransluta till den virtuella datorn med det lokala administratörskontot som skapades när etablerar den virtuella datorn.

Följ anvisningarna i artikeln [så att logga in på en virtuell dator som kör Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Konfigurera värdfilen på Linux-dator
Redigera/etc/hosts-filen i terminalen SSH och uppdatera datorns IP-adress och värdnamn.

```
sudo vi /etc/hosts
```

Ange följande värde i hosts-filen:

```
127.0.0.1 contoso-coreos.contoso100.com contoso-coreos
```
Här är ”contoso100.com” DNS-domännamnet för din hanterade domän. ”contoso-virtuell coreos' är värdnamnet på den virtuella datorn för virtuell CoreOS som du ansluter till den hanterade domänen.


## <a name="configure-the-sssd-service-on-the-linux-virtual-machine"></a>Konfigurera tjänsten SSSD på Linux-dator
Uppdatera konfigurationsfilen SSSD i ('/ etc/sssd/sssd.conf ”) enligt följande exempel:

 ```
 [sssd]
 config_file_version = 2
 services = nss, pam
 domains = CONTOSO100.COM

 [domain/CONTOSO100.COM]
 id_provider = ad
 auth_provider = ad
 chpass_provider = ad

 ldap_uri = ldap://contoso100.com
 ldap_search_base = dc=contoso100,dc=com
 ldap_schema = rfc2307bis
 ldap_sasl_mech = GSSAPI
 ldap_user_object_class = user
 ldap_group_object_class = group
 ldap_user_home_directory = unixHomeDirectory
 ldap_user_principal = userPrincipalName
 ldap_account_expire_policy = ad
 ldap_force_upper_case_realm = true
 fallback_homedir = /home/%d/%u

 krb5_server = contoso100.com
 krb5_realm = CONTOSO100.COM
 ```
Ersätt ”CONTOSO100. COM-med DNS-domännamnet för din hanterade domän. Kontrollera att du anger domännamnet i kapital fall i filen conf.


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Anslut den virtuella Linux-datorn till den hanterade domänen
De nödvändiga paketen är installerat på den virtuella Linux-datorn, är nästa uppgift att ansluta den virtuella datorn till den hanterade domänen.

```
sudo adcli join -D CONTOSO100.COM -U bob@CONTOSO100.COM -K /etc/krb5.keytab -H contoso-coreos.contoso100.com -N coreos
```


> [!NOTE]
> **Felsöka:** om *adcli* gick inte att hitta din hanterade domän:
  * Se till att domänen kan nås från den virtuella datorn (försök ping).
  * Kontrollera att den virtuella datorn faktiskt har distribuerats till samma virtuella nätverk som den hanterade domänen är tillgänglig.
  * Kontrollera om du har uppdaterat DNS-serverinställningarna för det virtuella nätverket så att den pekar till domänkontrollanterna i den hanterade domänen.
>

Starta tjänsten SSSD. Skriv följande kommando i terminalen SSH:
  ```
  sudo systemctl start sssd.service
  ```


## <a name="verify-domain-join"></a>Kontrollera domänanslutning
Kontrollera om datorn har anslutit till den hanterade domänen. Ansluta till domänanslutna CoreOS VM med hjälp av en annan SSH-anslutning. Använd ett domänanvändarkonto och kontrollera sedan om användarkontot är borta på rätt sätt.

1. Skriv följande kommando för att ansluta till domänen ansluten din terminal SSH virtuell CoreOS virtuell dator med SSH. Använda ett domänkonto som tillhör den hanterade domänen (till exempel 'bob@CONTOSO100.COM' i det här fallet.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-coreos.contoso100.com
    ```

2. Skriv följande kommando för att se om arbetskatalogen har initierats korrekt i terminalen SSH.
    ```
    pwd
    ```

3. Skriv följande kommando för att se om gruppmedlemskap som matchas korrekt i terminalen SSH.
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>Felsöka domänanslutning
Referera till den [felsökning domänanslutning](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshooting-domain-join) artikel.

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Anslut en virtuell dator med Windows Server till en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md)
* [Logga in till en virtuell dator som kör Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
