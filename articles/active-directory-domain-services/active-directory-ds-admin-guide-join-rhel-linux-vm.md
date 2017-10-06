---
title: "Azure Active Directory Domain Services: Anslut till en hanterad domän för RHEL VM-tooa | Microsoft Docs"
description: "Ansluta till en virtuell dator för Red Hat Enterprise Linux tooAzure AD DS"
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
ms.openlocfilehash: 41ca2aaf2eefbf9c403d2b834d61a1aa0943d950
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-tooa-managed-domain"></a>Anslut till en hanterad domän för Red Hat Enterprise Linux 7 virtuella tooa
Den här artikeln visar hur toojoin en Azure AD Domain Services med Red Hat Enterprise Linux (RHEL) 7 virtuella tooan hanterade domän.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Etablera en virtuell dator med Red Hat Enterprise Linux
Utföra hello följande steg tooprovision en RHEL 7 virtuell dator med hello Azure-portalen.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

    ![Azure portalens instrumentpanel](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. Klicka på **ny** på hello vänster fönsterruta och typen **Red Hat** i sökfältet hello som visas i följande skärmbild hello. Poster för Red Hat Enterprise Linux visas i sökresultaten hello. Klicka på **Red Hat Enterprise Linux 7.2**.

    ![Välj RHEL i resultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. hello sökresultat i hello **allt** rutan bör innehålla hello Red Hat Enterprise Linux 7.2 avbildningen. Klicka på **Red Hat Enterprise Linux 7.2** tooview mer information om hello avbildning av virtuell dator.

    ![Välj RHEL i resultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. I hello **Red Hat Enterprise Linux 7.2** rutan du bör se mer information om hello avbildning av virtuell dator. I hello **Välj en distributionsmodell** listrutan, Välj **klassiska**. Klicka på hello **skapa** knappen.

    ![Visa information om bild](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. I hello **grunderna** sidan hello **Skapa virtuell dator** guiden ange hello **värdnamn** för hello nya virtuella datorn. Ange ett användarnamn för lokal administratör också i hello **användarnamn** fältet och en **lösenord**. Du kan också välja toouse SSH key tooauthenticate hello lokala administratörsanvändare. Också välja en **prisnivån** för hello virtuella datorn.

    ![Skapa VM - grunderna sida](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. I hello **storlek** sidan hello **Skapa virtuell dator** guiden, Välj hello storlek för hello virtuella datorn.

    ![Skapa VM - väljer storlek](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. I hello **inställningar** sidan hello **Skapa virtuell dator** guiden, Välj hello storage-konto för hello virtuella datorn. Klicka på **virtuellt nätverk** tooselect hello virtuellt nätverk toowhich hello Linux VM ska distribueras. I hello **virtuellt nätverk** bladet, Välj hello virtuellt nätverk där Azure AD Domain Services är tillgängligt. I det här exemplet väljer vi hello 'MyPreviewVNet' virtuellt nätverk.

    ![Skapa VM - väljer virtuellt nätverk](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. På hello **sammanfattning** sidan hello **Skapa virtuell dator** guiden, granska och klickar på hello **OK** knappen.

    ![Skapa VM - virtuella nätverk som har valts](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. Distribution av hello nya virtuella datorn baserat på hello RHEL 7.2 avbildningen ska starta.

    ![Skapa VM - distributionen har startat](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. Efter några minuter ska hello virtuella datorn har distribuerats och redo för användning.

    ![Skapa VM - distribueras](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-toohello-newly-provisioned-linux-virtual-machine"></a>Fjärransluta toohello nyligen etablerats virtuell Linux-dator
hello RHEL 7.2 virtuella datorn har etablerats i Azure. hello nästa uppgift är tooconnect via fjärranslutning toohello virtuella datorn.

**Ansluta toohello RHEL 7.2 virtuella** följer du anvisningarna hello i hello artikel [hur toolog på tooa virtuell dator som kör Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

hello resten av hello steg förutsätter att du använder hello PuTTY SSH tooconnect toohello RHEL virtuella klientdatorn. Mer information finns i hello [PuTTY-hämtningssida](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Öppna hello PuTTY program.
2. Ange hello **värdnamn** för hello nyskapad RHEL virtuell dator. I det här exemplet har vår virtuella värdnamn för hello ”contoso-rhel.cloudapp .net'. Om du inte är säker på hello värdnamnet för den virtuella datorn läser du toohello VM instrumentpanelen på hello Azure-portalen.

    ![PuTTY ansluta](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. Logga in toohello virtuell dator med hello lokal administratörsautentiseringsuppgifter du angav när hello virtuella datorn skapades. I det här exemplet användes hello lokalt administratörskonto ”mahesh”.

    ![PuTTY-inloggning](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-hello-linux-virtual-machine"></a>Installera nödvändiga paket på hello virtuell Linux-dator
Efter anslutning toohello virtuella är hello nästa uppgift tooinstall paket som krävs för domänanslutning på hello virtuella datorn. Utför följande steg hello:

1. **Installera realmd:** hello realmd paketet används för domänanslutning. Skriv följande kommando hello i PuTTY terminalen:

    sudo yum installera realmd

    ![Installera realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Efter några minuter bör hello realmd paketet installeras på hello virtuella datorn.

    ![realmd installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. **Installera sssd:** hello realmd paket som beror på sssd tooperform domän join-operationer. Skriv följande kommando hello i PuTTY terminalen:

    sudo yum installera sssd

    ![Installera sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Efter några minuter bör hello sssd paketet installeras på hello virtuella datorn.

    ![realmd installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. **Installera kerberos:** i terminalen PuTTY skriver du följande kommando hello:

    sudo yum installera krb5 arbetsstation krb5-bibliotek

    ![Installera kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Efter några minuter bör hello realmd paketet installeras på hello virtuella datorn.

    ![Kerberos installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-hello-linux-virtual-machine-toohello-managed-domain"></a>Anslut till hello Linux virtuella toohello hanterad domän
Nu när hello krävs paket som är installerade på hello virtuell Linux-dator, är hello nästa uppgift toojoin hello virtuella toohello hanterad domän.

1. Identifiera hello AAD Domain Services-hanterad domän. Skriv följande kommando hello i PuTTY terminalen:

    sudo sfär identifiera CONTOSO100.COM

    ![Identifiera sfär](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Om **sfär identifiera** är toofind din hanterade domän Kontrollera hello domänen kan nås från hello virtuell dator (försök ping). Kontrollera också att hello den virtuella datorn har faktiskt distribuerade toohello samma virtuella nätverk i vilka hello hanterade domänen är tillgänglig.
2. Initiera kerberos. Skriv följande kommando hello i PuTTY terminalen. Kontrollera att du anger en användare som tillhör toohello ' AAD DC-administratörsgruppen. Endast dessa användare kan ansluta till datorer toohello hanterade domän.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Se till att du anger hello domännamn i versaler annan kinit misslyckas.
3. Ansluta till hello datorn toohello domän. Skriv följande kommando hello i PuTTY terminalen. Ange hello samma användare som du angav i föregående steg (kinit) hello.

    sudo sfär koppling--utförlig CONTOSO100.COM -U 'bob@CONTOSO100.COM'

    ![Anslut till domän](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Du bör få ett meddelande (”har registrerad dator i sfär”) när hello datorn är har anslutits toohello hanterad domän.

## <a name="verify-domain-join"></a>Kontrollera domänanslutning
Du kan snabbt kontrollera om har hello datorn har anslutits toohello hanterad domän. Ansluta toohello nyligen domänanslutna RHEL VM som använder SSH och ett domänanvändarkonto och sedan kontrollera toosee om hello-användarkontot är borta på rätt sätt.

1. Din PuTTY terminal och typen hello efter kommandot tooconnect toohello nyligen domänanslutna RHEL virtuell dator med SSH. Använda ett domänkonto som tillhör toohello hanterad domän (till exempel 'bob@CONTOSO100.COM' i det här fallet.)

    SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net
2. Ange hello efter kommandot toosee om hello arbetskatalog har initierats korrekt i terminalen PuTTY.

    pwd
3. Ange hello efter kommandot toosee om hello gruppmedlemskap som matchas korrekt i terminalen PuTTY.

    id

Ett exempel på utdata från de här kommandona visas nedan:

![Kontrollera domänanslutning](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a>Felsöka domänanslutning
Se toohello [felsökning domänanslutning](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) artikel.

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Ansluta till en Windows Server virtuella tooan Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md)
* [Hur toolog på tooa virtuell dator som kör Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Installera Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Guide för Windows-integrering](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
