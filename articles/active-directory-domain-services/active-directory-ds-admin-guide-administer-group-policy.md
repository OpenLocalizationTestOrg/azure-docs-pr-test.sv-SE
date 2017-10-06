---
title: "Azure Active Directory Domain Services: Administrera Grupprincip på hanterade domäner | Microsoft Docs"
description: "Administrera en Grupprincip på Azure Active Directory Domain Services hanterade domäner"
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
ms.openlocfilehash: d56ebf27e3015a7f3385ac5a4ddd77ea2c88387f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="administer-group-policy-on-an-azure-ad-domain-services-managed-domain"></a>Administrera Grupprincip i en Azure AD Domain Services-hanterad domän
Azure Active Directory Domain Services innehåller inbyggda grupprincipobjekt (GPO) för hello AADDC-användare och AADDC-datorer behållare. Du kan anpassa dessa inbyggda grupprincipobjekt tooconfigure Grupprincip i hello hanterade domän. Medlemmar i gruppen för hello AAD DC-administratörer kan dessutom skapa egna anpassade organisationsenheter i hello hanterade domän. De kan också skapa anpassade grupprincipobjekt och länka dem toothese anpassad organisationsenheter. Användare som tillhör toohello ' AAD DC-administratörsgruppen beviljas administratörsbehörighet för Grupprincip för hello-hanterad domän.

## <a name="before-you-begin"></a>Innan du börjar
tooperform hello uppgifterna som listas i den här artikeln, behöver du:

1. En giltig **Azure-prenumeration**.
2. En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.
3. **Azure AD Domain Services** måste vara aktiverat för hello Azure AD-katalogen. Om du inte gjort det, följ alla hello uppgifter som beskrivs i hello [Kom igång-guiden](active-directory-ds-getting-started.md).
4. En **domänanslutna virtuella** från vilken du administrerar hello Azure AD Domain Services-hanterad domän. Om du inte har sådan en virtuell dator, följ alla hello uppgifter som beskrivs i artikeln hello [ansluta till en hanterad domän för Windows virtuella tooa](active-directory-ds-admin-guide-join-windows-vm.md).
5. Du behöver hello autentiseringsuppgifterna för en **kontot tillhör toohello AAD DC-administratörer användargrupp** i katalogen tooadminister Grupprincip för din hanterade domän.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-tooremotely-administer-group-policy-for-hello-managed-domain"></a>Uppgift 1 - etablera en domänansluten virtuella tooremotely administrera Grupprincip för hello-hanterad domän
Azure AD Domain Services-hanterade domäner kan hanteras med vanliga Active Directory administrativa verktyg som hello Active Directory administrativa Center (ADAC) eller AD PowerShell. På liknande sätt Grupprincip för hello-hanterad domän kan du administrera med hello administrationsverktygen för Grupprincip.

Administratörer i din Azure AD-katalog har inte privilegier tooconnect toodomain domänkontrollanter på hello hanterade domänen via fjärrskrivbord. Medlemmar i gruppen för hello AAD DC-administratörer kan fjärradministrera en grupprincip för hanterade domäner. De kan använda Grupprincip verktyg på en hanterad domän för Windows-servern eller-klienten datorn kopplade toohello. Verktyg för gruppen kan installeras som en del av hello valfri funktion för hantering av Grupprincip på datorer med Windows Server och klient ansluten toohello hanterad domän.

hello första uppgiften är tooprovision en virtuell dator för Windows Server som är kopplade toohello hanterad domän. Instruktioner finns i artikeln toohello [ansluta till en Windows Server virtuella tooan Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md).

## <a name="task-2---install-group-policy-tools-on-hello-virtual-machine"></a>Uppgift 2 – installera Grupprincip verktyg på hello virtuell dator
Utföra hello följa steg tooinstall hello grupp princip Administrationsverktyg på hello domänanslutna virtuell dator.

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
9. På hello **funktioner** sidan, Välj hello **Grupprinciphantering** funktion.

    ![Sidan funktioner](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management.png)
10. På hello **bekräftelse** klickar du på **installera** tooinstall hello Grupprinciphantering funktionen på hello virtuella datorn. När för funktionsinstallationen är klar klickar du på **Stäng** tooexit hello **Lägg till roller och funktioner** guiden.

    ![Bekräftelsesida](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-gp-management-confirmation.png)

## <a name="task-3---launch-hello-group-policy-management-console-tooadminister-group-policy"></a>Uppgift 3 – Starta hello Group Policy management console tooadminister Grupprincip
Du kan använda hello konsolen Grupprinciphantering på hello domänanslutna virtuella tooadminister Grupprincip i hello hanterade domän.

> [!NOTE]
> Du måste toobe medlem av hello ' AAD DC-administratörsgruppen tooadminister Grupprincip i hello hanterade domän.
>
>

1. Från startskärmen hello klickar du på **Administrationsverktyg**. Du bör se hello **Grupprinciphantering** konsolen har installerats på hello virtuell dator.

    ![Starta hantering av Grupprincip](./media/active-directory-domain-services-admin-guide/gp-management-installed.png)
2. Klicka på **Grupprinciphantering** toolaunch hello hanteringskonsolen för Grupprincip.

    ![Group Policy Console](./media/active-directory-domain-services-admin-guide/gp-management-console.png)

## <a name="task-4---customize-built-in-group-policy-objects"></a>Uppgift 4 – anpassa inbyggda grupprincipobjekt
Det finns två inbyggda grupprincipobjekt (GPO) – ett för hello 'AADDC datorer' och 'AADDC användare-behållare i din hanterade domän. Du kan anpassa dessa grupprincipobjekt tooconfigure Grupprincip i hello hanterade domän.

1. I hello **Grupprinciphantering** klickar du på tooexpand hello **skog: contoso100.com** och **domäner** noder toosee hello grupprinciper för din hanterade domän.

    ![Inbyggda grupprincipobjekt](./media/active-directory-domain-services-admin-guide/builtin-gpos.png)
2. Du kan anpassa dessa inbyggda grupprincipobjekt tooconfigure grupprinciper på din hanterade domän. Högerklicka på hello Grupprincipobjektet och klicka på **redigera...**  toocustomize hello inbyggda GPO. hello Redigeraren för konfiguration av verktyget kan du toocustomize hello grupprincipobjekt.

    ![Redigera inbyggda GPO](./media/active-directory-domain-services-admin-guide/edit-builtin-gpo.png)
3. Du kan nu använda hello **Redigeraren för Grupprinciphantering** konsolen tooedit hello inbyggda GPO. Till exempel visar hello följande skärmbild som hur toocustomize hello inbyggda AADDC-datorer grupprincipobjekt.

    ![Anpassa GPO](./media/active-directory-domain-services-admin-guide/gp-editor.png)

## <a name="task-5---create-a-custom-group-policy-object-gpo"></a>Uppgift 5 – skapa en anpassad grupp grupprincipobjektet (GPO)
Du kan skapa eller importera dina egna anpassade grupprincipobjekt. Du kan också länka anpassade grupprincipobjekt tooa anpassad Organisationsenhet som du har skapat i din hanterade domän. Mer information om hur du skapar anpassade organisationsenheter finns [skapa en anpassad Organisationsenhet på en hanterad domän](active-directory-ds-admin-guide-create-ou.md).

> [!NOTE]
> Du måste toobe medlem av hello ' AAD DC-administratörsgruppen tooadminister Grupprincip i hello hanterade domän.
>
>

1. I hello **Grupprinciphantering** klickar du på tooselect din egen organisationsenhet (OU). Högerklicka på hello Organisationsenheten och på **skapa ett grupprincipobjekt i den här domänen och länka det här...** .

    ![Skapa en anpassad grupprincipobjekt](./media/active-directory-domain-services-admin-guide/gp-create-gpo.png)
2. Ange ett namn för hello nya Grupprincipobjektet och klicka på **OK**.

    ![Ange ett namn för GPO](./media/active-directory-domain-services-admin-guide/gp-specify-gpo-name.png)
3. Ett nytt grupprincipobjekt har skapats och länkats tooyour anpassad Organisationsenhet. Högerklicka på hello Grupprincipobjektet och klicka på **redigera...**  hello-menyn.

    ![Nyligen skapade GPO](./media/active-directory-domain-services-admin-guide/gp-gpo-created.png)
4. Du kan anpassa hello nyskapad grupprincipobjekt med hello **Redigeraren för Grupprinciphantering**.

    ![Anpassa nytt GPO](./media/active-directory-domain-services-admin-guide/gp-customize-gpo.png)


Mer information om hur du använder [konsolen Grupprinciphantering](https://technet.microsoft.com/library/cc753298.aspx) finns på Technet.

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Ansluta till en Windows Server virtuella tooan Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md)
* [Administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
* [Konsolen Grupprinciphantering](https://technet.microsoft.com/library/cc753298.aspx)
