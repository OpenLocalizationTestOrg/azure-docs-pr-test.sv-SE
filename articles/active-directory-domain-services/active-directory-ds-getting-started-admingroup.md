---
title: "Azure Active Directory Domain Services: Komma igång | Microsoft Docs"
description: "Aktivera Azure Active Directory Domain Services med hello Azure-portalen (förhandsgranskning)"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: maheshu
ms.openlocfilehash: 8bde872a13bc9960d1e62c74017ff78a8953a0a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a>Aktivera Azure Active Directory Domain Services med hello Azure-portalen (förhandsgranskning)


## <a name="task-3-configure-administrative-group"></a>Uppgift 3: Konfigurera administrativ grupp
I den här uppgiften skapar du en administrativ grupp i Azure AD-katalogen. Den här särskilda administrativa gruppen kallas *AAD DC administratörer*. Medlemmar i den här gruppen har beviljats administratörsbehörighet på datorer som är ansluten till domänen toohello hanterad domän. Den här gruppen har lagts till toohello administratörsgruppen på domänanslutna datorer. Medlemmar i den här gruppen kan dessutom använda Fjärrskrivbord tooconnect via fjärranslutning toodomain anslutna datorer.

> [!NOTE]
> Du har inte behörighet som domänadministratör eller företagsadministratör på hello-hanterad domän som du skapat med hjälp av Azure Active Directory Domain Services. Dessa behörigheter är reserverade av hello-tjänsten på hanterade domäner och görs inte tillgängliga toousers inom hello-klient. Du kan dock använda hello särskilda administrativa gruppen skapas i den här konfigurationen uppgiften tooperform vissa Privilegierade åtgärder. Dessa åtgärder är ansluta till datorer toohello domän, som tillhör toohello administratörsgrupp på domänanslutna datorer och konfigurera Grupprincip.
>

hello guiden skapar automatiskt hello administrativ grupp i Azure AD-katalogen. Den här gruppen kallas AAD DC-administratörer. Om du har en befintlig grupp med det här namnet i Azure AD-katalogen väljs hello den här gruppen. Du kan konfigurera gruppmedlemskap med hello **administratörsgruppen** sidan i guiden.

1. tooconfigure gruppmedlemskap, klickar du på **AAD DC administratörer**.

    ![Konfigurera gruppmedlemskap](./media/getting-started/domain-services-blade-admingroup.png)

2. Klicka på hello **lägga till medlemmar** knappen tooadd användare från din Azure AD directory toohello administratörsgruppen.

3. När du är klar klickar du på **OK** toomove på toohello **sammanfattning** hello guiden.

4. På hello **sammanfattning** hello guiden granska hello konfigurationsinställningar för hello-hanterad domän. Du kan gå tillbaka tooany steg i hello guiden toomake ändringar om det behövs. När du är klar klickar du på **OK** toocreate hello nya hanterade domän.

    ![Sammanfattning](./media/getting-started/domain-services-blade-summary.png)

5. Du ser ett meddelande som visar hello förloppet för distributionen av Azure AD Domain Services. Klicka på hello-meddelande toosee beskrivs förloppet för hello-distribution.

    ![Meddelande - distribution pågår](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a>Etablera din hanterade domän
hello processen för etablering din hanterade domän kan ta upp tooan timme.

1. När din distribution pågår, kan du söka efter domain services i hello **söka resurser** sökrutan. Välj **Azure AD Domain Services** från hello sökresultatet. Hej **Azure AD Domain Services** bladet visar hello-hanterad domän som har etablerats.

    ![Hitta hanterad domän som har etablerats](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Klicka på hello hanterade domän (till exempel ”contoso100.com”) toosee hello namn mer information om hello domän.

    ![DS - Etableringsstatus](./media/getting-started/domain-services-provisioning-state.png)

3. Hej **översikt** visar hello domänen håller på att etableras. Du kan konfigurera hello-hanterad domän tills den är helt etablerad. Det kan ta upp tooan timme för din hanterade domän toobe helt etablerad.

    ![DS - översiktsflik under hello Etableringsstatus ](./media/getting-started/domain-services-provisioning-state-details.png)

4. När hello-hanterad domän är helt etablerad, hello **översikt** visar status för hello som **kör**.

    ![Domain Services - översiktsflik vid full etablering](./media/getting-started/domain-services-provisioned.png)

5. På hello **egenskaper** kan du se två IP-adresser på vilken domän domänkontrollanter är tillgängliga för hello virtuellt nätverk.

    ![Domain Services – fliken Egenskaper när helt etablerad](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="next-step"></a>Nästa steg
[Uppgift 4: uppdatera hello DNS-inställningarna för hello Azure-nätverk](active-directory-ds-getting-started-dns.md)
