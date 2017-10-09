---
title: 'Azure Active Directory Domain Services: Administrationsguide | Microsoft Docs'
description: "Skapa en organisationsenhet (OU) i Azure AD Domain Services hanterade domäner"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: ce7539e5d5c7c1bf9505ef229f2d31d84c00da05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Skapa en organisationsenhet (OU) på en Azure AD Domain Services-hanterad domän
Azure AD Domain Services-hanterade domäner omfatta två inbyggda behållare som kallas för respektive AADDC-datorer och AADDC-användare. hello anslutit 'AADDC datorer' behållaren har datorobjekt för alla datorer som har toohello hanterade domän. hello ' AADDC' användarbehållaren innehåller användare och grupper i hello Azure AD-klient. Ibland kan vara det nödvändigt toocreate tjänstkonton på hello hanterade domänen toodeploy arbetsbelastningar. Du kan skapa en egen organisationsenhet (OU) på hello-hanterad domän och skapa tjänstkonton i denna Organisationsenhet för detta ändamål. Den här artikeln beskrivs hur du toocreate en Organisationsenhet i din hanterade domän.

## <a name="before-you-begin"></a>Innan du börjar
tooperform hello uppgifterna som listas i den här artikeln, behöver du:

1. En giltig **Azure-prenumeration**.
2. En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.
3. **Azure AD Domain Services** måste vara aktiverat för hello Azure AD-katalogen. Om du inte gjort det, följ alla hello uppgifter som beskrivs i hello [Kom igång-guiden](active-directory-ds-getting-started.md).
4. En domänansluten virtuell dator från vilken du administrerar hello Azure AD Domain Services hanterade domän. Om du inte har sådan en virtuell dator, följ alla hello uppgifter som beskrivs i artikeln hello [ansluta till en hanterad domän för Windows virtuella tooa](active-directory-ds-admin-guide-join-windows-vm.md).
5. Du behöver hello autentiseringsuppgifterna för en **kontot tillhör toohello AAD DC-administratörer användargrupp** i katalogen toocreate en anpassad Organisationsenhet på din hanterade domän.

## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Installera AD Administrationsverktyg på en domänansluten dator för fjärradministration
Azure AD Domain Services-hanterade domäner kan hanteras med vanliga Active Directory administrativa verktyg som hello Active Directory administrativa Center (ADAC) eller AD PowerShell. Innehavaradministratörer har inte privilegier tooconnect toodomain domänkontrollanter på hello hanterade domänen via fjärrskrivbord. tooadminister Hej hanterad domän, installera hello AD administration tools-funktionen på en virtuell dator kopplade toohello hanterad domän. Läs artikeln toohello [administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md) anvisningar.

## <a name="create-an-organizational-unit-on-hello-managed-domain"></a>Skapa en organisationsenhet på hello-hanterad domän
Hello AD Administrationsverktyg är installerat på hello domänanslutna virtuell dator, vi kan använda dessa verktyg toocreate en organisationsenhet i hello hanterade domän. Utför följande steg hello:

> [!NOTE]
> Bara medlemmar i hello AAD DC-administratörer har behörighet som krävs för toocreate hello en anpassad Organisationsenhet. Se till att du utför följande steg som en användare som tillhör gruppen toothis hello.
>
>

1. Från startskärmen hello klickar du på **Administrationsverktyg**. Du bör se hello AD administrativa verktyg som är installerad på hello virtuell dator.

    ![Administrativa verktyg som installerats på servern](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Klicka på **Active Directory Administrationscenter**.

    ![Active Directory Administrationscenter](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. tooview hello domän, klickar du på domännamnet för hello hello vänster (till exempel ”contoso100.com”).

    ![ADAC - Visa domän](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)
4. Hello höger **uppgifter** rutan klickar du på **ny** under hello domännod namn. I det här exemplet vi klickar du på **ny** under hello 'contoso100(local)'-nod hello höger **uppgifter** fönstret.

    ![ADAC - ny Organisationsenhet](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)
5. Du bör se hello alternativet toocreate en organisationsenhet. Klicka på **organisationsenhet** toolaunch hello **skapa organisationsenhet** dialogrutan.
6. I hello **skapa organisationsenhet** dialogrutan, ange en **namn** för hello ny Organisationsenhet. Ange en kort beskrivning för hello Organisationsenhet. Du kan också ange hello **hanterad av** för hello Organisationsenhet. toocreate Hej anpassad Organisationsenhet, klicka på **OK**.

    ![ADAC - OU dialogrutan Skapa](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)
7. hello nyskapad OU: N bör nu visas i hello AD Administrative Center (ADAC).

    ![ADAC - Organisationsenhet som har skapats](./media/active-directory-domain-services-admin-guide/create-ou-done.png)

## <a name="permissionssecurity-for-newly-created-ous"></a>Behörigheter/säkerhet för nyskapade organisationsenheter
Som standard hello hello användare (medlem hello ' AAD DC-administratörsgruppen) som skapade hello anpassad Organisationsenhet beviljas administrativa privilegier (fullständig behörighet) över Organisationsenhet. hello-användare kan sedan gå vidare och bevilja behörighet tooother användare eller toohello ' AAD DC-administratörsgruppen efter behov. Som det visas i följande skärmbild hello hello användaren 'bob@domainservicespreview.onmicrosoft.com' som skapade hello ny 'MyCustomOU' organisationsenhet beviljas fullständig kontroll över den.

 ![ADAC - säkerhet för ny Organisationsenhet](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)

## <a name="notes-on-administering-custom-ous"></a>Information om hur du administrerar anpassade organisationsenheter
Nu när du har skapat en anpassad Organisationsenhet kan du gå vidare och skapa användare, grupper, datorer och tjänstkonton i Organisationsenheten. Du kan inte flytta användare eller grupper från hello AADDC-användare OU toocustom organisationsenheter.

> [!WARNING]
> Användarkonton, grupper, tjänstkonton och datorobjekt som du skapar under anpassade organisationsenheter är inte tillgängliga i Azure AD-klienten. Dessa objekt visas med andra ord inte konfigurera med hello Azure AD Graph API eller hello Azure AD-Gränssnittet. Objekten är bara tillgängliga i din Azure AD Domain Services-hanterad domän.
>
>

## <a name="related-content"></a>Relaterat innehåll
* [Administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
* [Konfigurera en Grupprincip på en hanterad domän](active-directory-ds-admin-guide-administer-group-policy.md)
* [Active Directory Administrationscenter: Komma igång](https://technet.microsoft.com/library/dd560651.aspx)
* [Stegvisa instruktioner för tjänstkonton](https://technet.microsoft.com/library/dd548356.aspx)
