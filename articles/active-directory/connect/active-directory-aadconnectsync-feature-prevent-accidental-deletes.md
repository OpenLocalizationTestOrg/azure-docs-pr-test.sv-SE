---
title: "Azure AD Connect-synkronisering: förhindra oavsiktliga borttagningar | Microsoft Docs"
description: "Det här avsnittet beskrivs hello förhindra oavsiktliga borttagningar (förhindra oavsiktliga borttagningar)-funktionen i Azure AD Connect."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 159597f8354806fcaea1430e0ff84956338592a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect-synkronisering: Förhindra oavsiktliga borttagningar
Det här avsnittet beskrivs hello förhindra oavsiktliga borttagningar (förhindra oavsiktliga borttagningar)-funktionen i Azure AD Connect.

När du installerar Azure AD Connect förhindra oavsiktliga borttagningar är aktiverad som standard och konfigurerade toonot Tillåt export med mer än 500 borttagningar. Den här funktionen är utformad tooprotect du från oavsiktliga konfiguration ändras och ändras tooyour på lokal katalog som påverkar många användare och andra objekt.

## <a name="what-is-prevent-accidental-deletes"></a>Vad är förhindra oavsiktliga borttagningar
Vanliga scenarier när du ser många borttagningar inkluderar:

* Ändringar för[filtrering](active-directory-aadconnectsync-configure-filtering.md) där en hel [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) eller [domän](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) är avmarkerat.
* Alla objekt i en Organisationsenhet tas bort.
* En Organisationsenhet byter namn så att alla objekt i den betraktas toobe utanför omfånget för synkronisering.

hello standardvärdet 500 objekt kan ändras med PowerShell med hjälp av `Enable-ADSyncExportDeletionThreshold`. Du bör konfigurera det här värdet toofit hello storlek på din organisation. Eftersom hello sync scheduler körs var 30: e minut, är hello-värdet hello antal borttagningar visas inom 30 minuter.

Om det finns exporteras för många borttagningar mellanlagrad toobe tooAzure AD, hello export stoppar och du får ett e-postmeddelande så här:

![Förhindra oavsiktliga borttagningar e-post](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hello (tekniska kontakt). Kl hello identitet synkroniseringstjänsten har upptäckt att hello antal borttagningar överskridit hello konfigurerade borttagning tröskeln för (organisationsnamn). Totalt (nummer) objekt har skickats för borttagning i den här identitetssynkronisering kör. Detta uppnåtts eller överskridits hello konfigurerats borttagning tröskelvärdet (nummer)-objekt. Vi behöver tooprovide bekräftelse som borttagningarna ska bearbetas innan vi fortsätter. Se hello förhindra oavsiktliga borttagningar mer information om hello fel som anges i e-postmeddelandet.*
>
> 

Du kan också se status för hello `stopped-deletion-threshold-exceeded` när du tittar i hello **Synchronization Service Manager** Gränssnittet för hello Exportera profil.
![Förhindra oavsiktliga borttagningar Sync Service Manager UI](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Om det var oväntat kan undersöka och vidta åtgärder. toosee vilka objekt som är om toobe bort, hello följande:

1. Starta **synkroniseringstjänsten** från hello Start-menyn.
2. Gå för**kopplingar**.
3. Välj hello Connector med typen **Azure Active Directory**.
4. Under **åtgärder** toohello höger, Välj **söka Anslutarplats**.
5. I popup-under hello **omfång**väljer **kopplas från eftersom** och väljer en tid i hello tidigare. Klicka på **Sök**. Den här sidan innehåller en vy över alla objekt om toobe tas bort. Genom att klicka på varje objekt, kan du få mer information om hello-objektet. Du kan också klicka på **kolumnen inställningen** tooadd ytterligare attribut visas i rutnätet hello toobe.

![Söka Anslutarplats](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Om alla hello borttagningar önskas, och sedan hello följande:

1. tooretrieve hello aktuella tröskelvärde för borttagning, Kör PowerShell-cmdlet för hello `Get-ADSyncExportDeletionThreshold`. Ange en Global administratör för Azure AD-konto och lösenord. hello standardvärdet är 500.
2. tootemporarily inaktivera skyddet och gör dessa borttagningar gå igenom, kör hello PowerShell-cmdlet: `Disable-ADSyncExportDeletionThreshold`. Ange en Global administratör för Azure AD-konto och lösenord.
   ![Autentiseringsuppgifter](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
3. Med hello Azure Active Directory-kopplingen fortfarande markerat, Välj hello åtgärd **kör** och välj **exportera**.
4. Aktivera toore hello skydd, kör hello PowerShell-cmdlet: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`. Ersätt 500 med hello-värde som du har lagt märke vid hämtning av hello aktuella tröskelvärdet för borttagning. Ange en Global administratör för Azure AD-konto och lösenord.

## <a name="next-steps"></a>Nästa steg
**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
