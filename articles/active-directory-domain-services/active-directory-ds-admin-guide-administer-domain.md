---
title: "Azure Active Directory Domain Services: Administrera en hanterad domän | Microsoft Docs"
description: "Administrera Azure Active Directory Domain Services hanterade domäner"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4fdbc75-3e6b-4e20-8494-5dcc3bf2220a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 11acc79e06163e3193b1aa981f2cd28af812789d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Administrera en Azure Active Directory Domain Services-hanterad domän
Den här artikeln visar hur tooadminister Azure Active Directory (AD)-domäntjänster hanterade domän.

## <a name="before-you-begin"></a>Innan du börjar
tooperform hello uppgifterna som listas i den här artikeln, behöver du:

1. En giltig **Azure-prenumeration**.
2. En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.
3. **Azure AD Domain Services** måste vara aktiverat för hello Azure AD-katalogen. Om du inte gjort det, följ alla hello uppgifter som beskrivs i hello [Kom igång-guiden](active-directory-ds-getting-started.md).
4. En **domänanslutna virtuella** från vilken du administrerar hello Azure AD Domain Services-hanterad domän. Om du inte har sådan en virtuell dator, följ alla hello uppgifter som beskrivs i artikeln hello [ansluta till en hanterad domän för Windows virtuella tooa](active-directory-ds-admin-guide-join-windows-vm.md).
5. Du behöver hello autentiseringsuppgifterna för en **kontot tillhör toohello AAD DC-administratörer användargrupp** i katalogen tooadminister din hanterade domän.

<br>

## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Administrativa uppgifter som du kan utföra på en hanterad domän
Medlemmar i gruppen för hello AAD DC-administratörer beviljas behörighet för hello-hanterad domän som gör dem tooperform uppgifter som:

* Ansluta till datorer toohello hanterade domän.
* Konfigurera hello inbyggda GPO för hello 'AADDC datorer' och 'AADDC användare-behållare i hello hanterade domän.
* Administrera DNS på hello-hanterad domän.
* Skapa och administrera anpassade organisationsenheter (OU) i hello hanterade domän.
* Få administrativ åtkomst toocomputers anslutit toohello hanterade domän.

## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Administratörsbehörighet behöver du inte på en hanterad domän
hello domän hanteras av Microsoft, inklusive aktiviteter, till exempel korrigering, övervakning och utföra säkerhetskopieringar. Därför hello domän är låst och du har inte privilegier tooperform vissa administrativa uppgifter i hello domän. Det är några exempel på uppgifter som du inte kan utföra nedan.

* Du har inte beviljats domänadministratör eller företagsadministratör behörighet för hello-hanterad domän.
* Du kan inte utöka hello schemat för hello-hanterad domän.
* Du kan inte ansluta toodomain styrenheter för hello hanterade domänen med hjälp av fjärrskrivbord.
* Du kan inte lägga till domänen domänkontrollanter toohello hanterad domän.

## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-tooremotely-administer-hello-managed-domain"></a>Uppgift 1 - etablera en domänansluten Windows Server virtuella tooremotely administrera hello hanterad domän
Azure AD Domain Services-hanterade domäner kan hanteras med hjälp av välbekanta Active Directory administrativa verktyg, till exempel hello Active Directory administrativa Center (ADAC) eller AD PowerShell. Innehavaradministratörer har inte privilegier tooconnect toodomain domänkontrollanter på hello hanterade domänen via fjärrskrivbord. Därför hanterade medlemmar i hello ' AAD DC-administratörsgruppen kan administrera domäner med Administrationsverktyg för AD från en Windows Server/klientdator som är kopplade toohello hanterad domän. Administrationsverktyg för AD kan installeras som en del av hello valfri funktion för fjärråtkomst verktyg för fjärrserveradministration (RSAT) på datorer med Windows Server och klient ansluten toohello hanterade domän.

hello första steget är tooset upp en virtuell dator för Windows Server som är kopplade toohello hanterade domän. Instruktioner finns i artikeln toohello [ansluta till en Windows Server virtuella tooan Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-hello-managed-domain-from-a-client-computer-for-example-windows-10"></a>Fjärradministrera hello-hanterad domän från en klientdator (till exempel Windows 10)
hello instruktionerna i den här artikeln används en Windows Server virtuella tooadminister hello AAD-DS hanterade domän. Men kan du också välja toouse en Windows-klienter (till exempel Windows 10) virtuella toodo så.

Du kan [installera fjärråtkomst verktyg för fjärrserveradministration (RSAT)](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) på en Windows-klient virtuell dator genom att följa hello anvisningar på TechNet.

## <a name="task-2---install-active-directory-administration-tools-on-hello-virtual-machine"></a>Uppgift 2 – Installera Active Directory-Administrationsverktyg på hello virtuell dator
Utföra hello följa steg tooinstall hello Active Directory-Administration tools på hello domänanslutna virtuell dator. Mer information finns på Technet [information om hur du installerar och använder Remote Server Administration Tools](https://technet.microsoft.com/library/hh831501.aspx).

1. Navigera för**virtuella datorer** nod i hello klassiska Azure-portalen. Välj hello virtuella dator som du skapade i uppgift 1 och klicka på **Anslut** på hello kommandofältet längst hello hello-fönstret.

    ![Ansluta tooWindows virtuell dator](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. efterfrågar tooopen hello klassiska portalen eller spara en fil med tillägget 'RDP-', vilket är används tooconnect toohello virtuell dator. Klicka på tooopen hello filen när den har hämtats.
3. Använd hello autentiseringsuppgifterna för en användare som tillhör toohello ' AAD DC-administratörsgruppen på hello inloggningsskärm. Exempelvis kan vi använda 'bob@domainservicespreview.onmicrosoft.com' i vårt fall.
4. Hello startskärmen öppna **Serverhanteraren**. Klicka på **Lägg till roller och funktioner** hello centrala ruta hello Serverhanteraren.

    ![Starta Serverhanteraren på den virtuella datorn](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. På hello **innan du börjar** sidan hello **guiden Lägg till roller och funktioner**, klickar du på **nästa**.

    ![Innan du börjar sida](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. På hello **installationstyp** lämnar hello **rollbaserad eller funktionsbaserad installation** alternativet som är markerat och klicka på **nästa**.

    ![Installationstyp](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. På hello **Serverval** , Välj hello aktuella virtuella datorn från hello-serverpoolen och klicka på **nästa**.

    ![Sidan för val av Server](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. På hello **serverroller** klickar du på **nästa**. Vi hoppar över den här sidan eftersom vi inte installerar några roller på hello-servern.
9. På hello **funktioner** klickar du på tooexpand hello **Remote Server Administration Tools** noden och klicka sedan på tooexpand hello **Rolladministrationsverktyg** nod. Välj **AD DS och AD LDS-verktyg** funktionen hello listan över Rolladministrationsverktyg.

    ![Sidan funktioner](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)
10. På hello **bekräftelse** klickar du på **installera** tooinstall hello AD och AD LDS-verktyg funktion på hello virtuella datorn. När för funktionsinstallationen är klar klickar du på **Stäng** tooexit hello **Lägg till roller och funktioner** guiden.

    ![Bekräftelsesida](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)

## <a name="task-3---connect-tooand-explore-hello-managed-domain"></a>Uppgift 3 – ansluta tooand utforska hello-hanterad domän
Hello AD Administrationsverktyg är installerat på hello domänanslutna virtuell dator, kan vi använda dessa verktyg tooexplore och administrera hello-hanterad domän.

> [!NOTE]
> Du behöver toobe medlem av hello ' AAD DC-administratörsgruppen tooadminister hello hanterade domän.
>
>

1. Från startskärmen hello klickar du på **Administrationsverktyg**. Du bör se hello AD administrativa verktyg som är installerad på hello virtuell dator.

    ![Administrativa verktyg som installerats på servern](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Klicka på **Active Directory Administrationscenter**.

    ![Active Directory Administrationscenter](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. tooexplore hello domän, klickar du på domännamnet för hello hello vänster (till exempel ”contoso100.com”). Meddelande två behållare kallas AADDC-datorer och AADDC-användare.

    ![ADAC - Visa domän](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)
4. Klicka på hello-behållare som kallas **AADDC användare** toosee alla användare och grupper som tillhör toohello hanterade domän. Du bör se användarkonton och grupper från din Azure AD-klient visa upp i den här behållaren. I det här exemplet, ett användarkonto för hello användare som kallas ”bob” och en grupp med namnet AAD DC-administratörer är tillgängliga i den här behållaren.

    ![ADAC - domänanvändare](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)
5. Klicka på hello-behållare som kallas **AADDC datorer** toosee hello datorerna anslutna toothis hanterad domän. Du bör se en post för hello aktuella virtuella datorn, som är kopplade toohello domänen. Datorkontona för alla datorer som är anslutna toohello Azure AD Domain Services-hanterad domän lagras i den här behållaren AADDC-datorer.

    ![ADAC - domänanslutna datorer](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Ansluta till en Windows Server virtuella tooan Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md)
* [Distribuera fjärradministrationsverktyg för Server](https://technet.microsoft.com/library/hh831501.aspx)
