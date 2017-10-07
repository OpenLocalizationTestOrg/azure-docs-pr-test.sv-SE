---
title: "Azure Active Directory Domain Services: Administrera DNS på hanterade domäner | Microsoft Docs"
description: "Administrera DNS på Azure Active Directory Domain Services hanterade domäner"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: f2085283649eadd3c9e89f708b0eecf10b2d7d70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Administrera DNS på en Azure AD Domain Services-hanterad domän
Azure Active Directory Domain Services innehåller en server, DNS (Domain Name Resolution) som ger DNS-matchning för hello-hanterad domän. Ibland kan behöva du tooconfigure DNS på hello hanterade domän. Du kan behöva toocreate DNS-poster för datorer som inte är domänansluten toohello domän, konfigurera den virtuella IP-adresser för belastningsutjämnare eller konfigurera externa DNS-vidarebefordrare. Därför kan beviljas användare som tillhör toohello ' AAD DC-administratörsgruppen DNS administratörsbehörighet för hello-hanterad domän.

## <a name="before-you-begin"></a>Innan du börjar
tooperform hello uppgifterna som listas i den här artikeln, behöver du:

1. En giltig **Azure-prenumeration**.
2. En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.
3. **Azure AD Domain Services** måste vara aktiverat för hello Azure AD-katalogen. Om du inte gjort det, följ alla hello uppgifter som beskrivs i hello [Kom igång-guiden](active-directory-ds-getting-started.md).
4. En **domänanslutna virtuella** från vilken du administrerar hello Azure AD Domain Services-hanterad domän. Om du inte har sådan en virtuell dator, följ alla hello uppgifter som beskrivs i artikeln hello [ansluta till en hanterad domän för Windows virtuella tooa](active-directory-ds-admin-guide-join-windows-vm.md).
5. Du behöver hello autentiseringsuppgifterna för en **kontot tillhör toohello AAD DC-administratörer användargrupp** i katalogen tooadminister DNS för din hanterade domän.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-tooremotely-administer-dns-for-hello-managed-domain"></a>Uppgift 1 - etablera en domänansluten virtuella tooremotely administrera DNS för hello-hanterad domän
Azure AD Domain Services-hanterade domäner kan hanteras med vanliga Active Directory administrativa verktyg som hello Active Directory administrativa Center (ADAC) eller AD PowerShell. På liknande sätt DNS för hello-hanterad domän kan du administrera med hello administrationsverktygen för DNS-Server.

Administratörer i din Azure AD-katalog har inte privilegier tooconnect toodomain domänkontrollanter på hello hanterade domänen via fjärrskrivbord. Medlemmar i gruppen för hello AAD DC-administratörer kan administrera DNS för hanterade domäner med DNS-Server-verktyg från en Windows Server/klientdator som är kopplade toohello hanterad domän. Verktyg för DNS-Server kan installeras som en del av hello Remote verktyg för fjärrserveradministration (RSAT) valfri funktion i Windows Server och klientdatorer anslutit toohello hanterade domän.

hello första uppgiften är tooprovision en virtuell dator för Windows Server som är kopplade toohello hanterad domän. Instruktioner finns i artikeln toohello [ansluta till en Windows Server virtuella tooan Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-dns-server-tools-on-hello-virtual-machine"></a>Uppgift 2 – installera DNS-Server-verktyg på hello virtuell dator
Utföra hello följa steg tooinstall hello DNS-Administration tools på hello domänanslutna virtuell dator. Mer information om [installera och använda Administrationsverktyg för fjärrserver](https://technet.microsoft.com/library/hh831501.aspx), finns på Technet.

1. Navigera för**virtuella datorer** nod i hello klassiska Azure-portalen. Välj hello virtuella dator som du skapade i uppgift 1 och klicka på **Anslut** på hello kommandofältet längst hello hello-fönstret.

    ![Ansluta tooWindows virtuell dator](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. efterfrågar tooopen hello klassiska portalen eller spara en fil med tillägget 'RDP-', vilket är används tooconnect toohello virtuell dator. Klicka på hello filen när den har hämtats.
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
9. På hello **funktioner** klickar du på tooexpand hello **Remote Server Administration Tools** noden och klicka sedan på tooexpand hello **Rolladministrationsverktyg** nod. Välj **DNS-serververktyg** funktionen hello listan över Rolladministrationsverktyg.

    ![Sidan funktioner](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)
10. På hello **bekräftelse** klickar du på **installera** tooinstall hello DNS-serververktyg funktion på hello virtuella datorn. När för funktionsinstallationen är klar klickar du på **Stäng** tooexit hello **Lägg till roller och funktioner** guiden.

    ![Bekräftelsesida](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)

## <a name="task-3---launch-hello-dns-management-console-tooadminister-dns"></a>Uppgift 3 – Starta hello DNS management console tooadminister DNS
Funktionen installeras på hello DNS-serververktyg hello domänanslutna virtuell dator, använda vi hello DNS verktyg tooadminister DNS hello hanterade domän.

> [!NOTE]
> Du måste toobe medlem av hello ' AAD DC-administratörsgruppen tooadminister DNS på hello-hanterad domän.
>
>

1. Från startskärmen hello klickar du på **Administrationsverktyg**. Du bör se hello **DNS** konsolen har installerats på hello virtuell dator.

    ![Administrativa verktyg - DNS-konsolen](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)
2. Klicka på **DNS** toolaunch hello DNS-hanteringskonsolen.
3. I hello **ansluta tooDNS Server** dialogrutan klickar du på alternativet hello **hello följande datorn**, och ange hello DNS-domännamnet för hello-hanterad domän (till exempel ”contoso100.com”).

    ![DNS-konsolen – ansluta toodomain](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)
4. hello DNS-konsolen ansluter toohello hanterad domän.

    ![DNS-konsolen – administrera domän](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)
5. Du kan nu använda hello DNS-konsolen tooadd DNS-poster för datorer i hello virtuella nätverk som du har aktiverat AAD Domain Services.

> [!WARNING]
> Var försiktig när du administrerar DNS för hello hanteras med hjälp av Administrationsverktyg för DNS-domän. Se till att du **inte ta bort eller ändra hello inbyggda DNS-poster som används av Domain Services i domänen hello**. Inbyggda DNS-poster är DNS-poster för domänen, namnserverposter och andra poster som används för DC-plats. Om du ändrar dessa poster upplöst domäntjänster hello virtuella nätverket.
>
>

Se hello [DNS-verktyg artikel på Technet](https://technet.microsoft.com/library/cc753579.aspx) för mer information om hur du hanterar DNS.

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Ansluta till en Windows Server virtuella tooan Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md)
* [Administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
* [Administrationsverktyg för DNS](https://technet.microsoft.com/library/cc753579.aspx)
