---
title: "Azure Active Directory Domain Services: Anslut en RHEL VM till en hanterad domän | Microsoft Docs"
description: Anslut en virtuell Red Hat Enterprise Linux-dator till Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 87291c47-1280-43f8-8fb2-da1bd61a4942
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 69f1850bfed90392e9a4695e2443ffaa6bfc746d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Ansluta en Red Hat Enterprise Linux 7-virtuell dator till en hanterad domän
Den här artikeln visar hur du ansluter till en virtuell dator för Red Hat Enterprise Linux (RHEL) 7 till en hanterad Azure AD DS-domän.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Etablera en virtuell dator med Red Hat Enterprise Linux
Utför följande steg för att etablera en virtuell dator för RHEL 7 som använder Azure portal.

1. Logga in på [Azure Portal](https://portal.azure.com).

    ![Azure portalens instrumentpanel](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. Klicka på **ny** på den vänstra rutan och skriv **Red Hat** i sökfältet som visas i följande skärmbild. Poster för Red Hat Enterprise Linux visas i sökresultaten. Klicka på **Red Hat Enterprise Linux 7.2**.

    ![Välj RHEL i resultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. Sökresultat i den **allt** fönstret ska visa en lista med Red Hat Enterprise Linux 7.2 avbildningen. Klicka på **Red Hat Enterprise Linux 7.2** att visa mer information om den virtuella datoravbildningen.

    ![Välj RHEL i resultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. I den **Red Hat Enterprise Linux 7.2** rutan du bör se mer information om den virtuella datoravbildningen. I den **Välj en distributionsmodell** listrutan, Välj **klassiska**. Klicka på den **skapa** knappen.

    ![Visa information om bild](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. I den **grunderna** sida av den **Skapa virtuell dator** guiden ange de **värdnamn** för den nya virtuella datorn. Också ange ett användarnamn för lokal administratör i den **användarnamn** fältet och en **lösenord**. Du kan också välja att använda en SSH-nyckel för att autentisera användaren för lokal administratör. Också välja en **prisnivån** för den virtuella datorn.

    ![Skapa VM - grunderna sida](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. I den **storlek** sida av den **Skapa virtuell dator** guiden, Välj storlek för den virtuella datorn.

    ![Skapa VM - väljer storlek](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. I den **inställningar** sida av den **Skapa virtuell dator** guiden, Välj lagring kontot för den virtuella datorn. Klicka på **virtuellt nätverk** att välja det virtuella nätverket som Linux VM ska distribueras. I den **virtuellt nätverk** bladet väljer virtuella nätverk där Azure AD Domain Services är tillgängligt. I det här exemplet väljer vi det virtuella nätverket 'MyPreviewVNet'.

    ![Skapa VM - väljer virtuellt nätverk](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. På den **sammanfattning** sida av den **Skapa virtuell dator** guiden, granska och klickar på den **OK** knappen.

    ![Skapa VM - virtuella nätverk som har valts](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. Distribution av den nya virtuella datorn baserat på den RHEL 7.2 avbildningen ska starta.

    ![Skapa VM - distributionen har startat](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. Efter några minuter, ska den virtuella datorn distribueras har och redo för användning.

    ![Skapa VM - distribueras](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Fjärransluta till den nyetablerade virtuella Linux-datorn
RHEL 7.2 virtuella datorn har etablerats i Azure. Nästa uppgift är att fjärransluta till den virtuella datorn.

**Ansluta till den virtuella datorn RHEL 7.2** följer du anvisningarna i artikeln [så att logga in på en virtuell dator som kör Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Resten av stegen förutsätter att du använder PuTTY SSH-klienten för att ansluta till den virtuella datorn RHEL. Mer information finns i [PuTTY-hämtningssida](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Öppna PuTTY-programmet.
2. Ange den **värdnamn** för den nyligen skapade RHEL virtuella datorn. I det här exemplet har vår virtuella värdnamn ”contoso-rhel.cloudapp .net'. Om du inte är säker på namnet på den virtuella datorn finns på VM-instrumentpanelen på Azure-portalen.

    ![PuTTY ansluta](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. Logga in på den virtuella datorn med lokal administratörsbehörighet som du angav när den virtuella datorn har skapats. I det här exemplet används vi det lokala administratörskontot ”mahesh”.

    ![PuTTY-inloggning](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Installera nödvändiga paket på den virtuella Linux-datorn
När du har anslutit till den virtuella datorn, är nästa uppgift att installera paket som krävs för domänanslutning på den virtuella datorn. Utför följande steg:

1. **Installera realmd:** realmd paketet används för domänanslutning. Skriv följande kommando i PuTTY terminalen:

    sudo yum installera realmd

    ![Installera realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Efter några minuter ska realmd paketet installeras på den virtuella datorn.

    ![realmd installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. **Installera sssd:** realmd paketet är beroende av sssd att utföra kopplingsåtgärder för domänen. Skriv följande kommando i PuTTY terminalen:

    sudo yum installera sssd

    ![Installera sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Efter några minuter ska sssd paketet installeras på den virtuella datorn.

    ![realmd installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. **Installera kerberos:** i terminalen PuTTY skriver du följande kommando:

    sudo yum installera krb5 arbetsstation krb5-bibliotek

    ![Installera kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Efter några minuter ska realmd paketet installeras på den virtuella datorn.

    ![Kerberos installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Anslut den virtuella Linux-datorn till den hanterade domänen
De nödvändiga paketen är installerat på den virtuella Linux-datorn, är nästa uppgift att ansluta den virtuella datorn till den hanterade domänen.

1. Identifiera den hanterade domänen AAD Domain Services. Skriv följande kommando i PuTTY terminalen:

    sudo sfär identifiera CONTOSO100.COM

    ![Identifiera sfär](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Om **sfär identifiera** gick inte att hitta din hanterade domän, se till att domänen kan nås från den virtuella datorn (försök ping). Se också till att den virtuella datorn faktiskt har distribuerats till samma virtuella nätverk som den hanterade domänen är tillgänglig.
2. Initiera kerberos. Skriv följande kommando i terminalen PuTTY. Kontrollera att du anger en användare som tillhör gruppen AAD DC-administratörer. Endast dessa användare kan ansluta datorer till den hanterade domänen.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Se till att du anger domännamnet i versaler annan kinit misslyckas.
3. Anslut datorn till domänen. Skriv följande kommando i terminalen PuTTY. Ange samma användare som du angav i föregående steg (kinit).

    sudo sfär koppling--utförlig CONTOSO100.COM -U 'bob@CONTOSO100.COM'

    ![Anslut till domän](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Du bör få ett meddelande (”har registrerad dator i sfär”) när datorn har anslutit till den hanterade domänen.

## <a name="verify-domain-join"></a>Kontrollera domänanslutning
Du kan snabbt kontrollera om datorn har anslutit till den hanterade domänen. Ansluta till den nya domänanslutna RHEL VM som använder SSH och ett domänanvändarkonto och kontrollera sedan för att se om användarkontot är borta på rätt sätt.

1. I terminalen PuTTY skriver du följande kommando för att ansluta till den nya domänanslutna RHEL virtuell dator med SSH. Använda ett domänkonto som tillhör den hanterade domänen (till exempel 'bob@CONTOSO100.COM' i det här fallet.)

    SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net
2. Skriv följande kommando för att se om arbetskatalogen har initierats korrekt i terminalen PuTTY.

    pwd
3. Skriv följande kommando för att se om gruppmedlemskap som matchas korrekt i terminalen PuTTY.

    id

Ett exempel på utdata från de här kommandona visas nedan:

![Kontrollera domänanslutning](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a>Felsöka domänanslutning
Referera till den [felsökning domänanslutning](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) artikel.

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Anslut en virtuell dator med Windows Server till en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md)
* [Logga in till en virtuell dator som kör Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Installera Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Guide för Windows-integrering](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
