---
title: "Nytt körs hello Azure AD Connect-installationsguiden | Microsoft Docs"
description: "Beskriver hur fungerar hello installationsguiden hello andra gången du kör den."
keywords: "hello Azure AD Connect-installationsguiden kan du konfigurera Underhåll inställningar hello andra gången du kör den"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 83cc74aca471ef9b4f65f7f3582e3e48d3d81cfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-running-hello-installation-wizard-a-second-time"></a>Azure AD Connect-synkronisering: hello installationsguiden körs en gång
hello första gången du kör installationsguiden för hello Azure AD Connect den vägleder dig igenom hur tooconfigure din installation. Om du kör installationsguiden för hello igen erbjuder alternativ för underhåll.

Du kan hitta hello installationsguiden hello start-menyn med namnet **Azure AD Connect**.

![Start-menyn](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

När du startar installationsguiden för hello visas en sida med dessa alternativ:

![Sida med en lista över ytterligare aktiviteter](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Om du har installerat AD FS med Azure AD Connect måste ha du ytterligare alternativ. Hej ytterligare alternativ som du har för AD FS finns dokumenterade i [AD FS management](active-directory-aadconnect-federation-management.md#manage-ad-fs).

Välj en av hello uppgifter och klicka på **nästa** toocontinue.

> [!IMPORTANT]
> När du har hello installationsguiden öppna har alla åtgärder i hello Synkroniseringsmotorn avbrutits. Kontrollera att du stänger installationsguiden hello så snart du har slutfört konfigurationsändringarna.
>
>

## <a name="view-current-configuration"></a>Visa aktuell konfiguration
Det här alternativet ger dig en snabb överblick över dina för tillfället konfigurerade alternativ.

![Sida med en lista över alla alternativ och deras tillstånd](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Klicka på **föregående** toogo tillbaka. Om du väljer **avsluta**, Stäng av hello installationsguiden.

## <a name="customize-synchronization-options"></a>Anpassa synkroniseringsalternativ
Det här alternativet är används toomake ändringar toohello synkroniseringskonfiguration. Du ser en delmängd av alternativ från hello installationssökväg för anpassad konfiguration. Det visas här alternativet även om du har använt Snabbinstallation först.

* [Lägga till flera kataloger](active-directory-aadconnect-get-started-custom.md#connect-your-directories). För att ta bort en katalog, se [ta bort en koppling](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
* [Ändra domän och Organisationsenhet filtrering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
* Ta bort gruppfiltrering.
* [Ändra valfria funktioner](active-directory-aadconnect-get-started-custom.md#optional-features).

hello andra alternativ från hello första installationen kan inte ändras och är inte tillgängliga. Dessa alternativ är:

* Ändra hello attributet toouse för userPrincipalName och sourceAnchor.
* Ändra hello koppla metod för objekt från en annan skog.
* Aktivera gruppbaserade filtrering.

## <a name="refresh-directory-schema"></a>Uppdatera directory-schemat
Det här alternativet används om du har ändrat hello schema i en av dina lokala AD DS-skogar. Till exempel du kanske har installerat Exchange eller uppgraderat tooa Windows Server 2012-schemat med enhetsobjekt. I det här fallet behöver Azure AD Connect tooinstruct tooread hello schema igen från AD DS och uppdatera sin cache. Den här åtgärden även återskapar hello Sync regler. Om du lägger till hello Exchange-schemat, som exempel läggs hello Sync regler för Exchange toohello konfiguration.

När du väljer det här alternativet visas alla hello kataloger i konfigurationen. Du kan behålla hello standardinställningen och uppdatera alla skogar eller avmarkera vissa av dessa.

![Sida med en lista över alla kataloger i hello-miljö](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Konfigurera mellanlagringsläge
Det här alternativet kan du tooenable och inaktivera mellanlagringsläge på hello-servern. Mer information om mellanlagrade läge och hur de används finns i [Operations](active-directory-aadconnectsync-operations.md#staging-mode).

hello alternativet visar om mellanlagring är aktiverat eller inaktiverat:  
![Alternativ som visas också hello aktuell status för mellanlagringsläge](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

toochange Hej tillstånd, markerar du det här alternativet och markera eller avmarkera hello.  
![Alternativ som visas också hello aktuell status för mellanlagringsläge](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Ändra användarens inloggning
Det här alternativet kan du toochange från toofederation för synkronisering av lösenord eller hello tvärtom. Du kan inte ändra för**konfigurerar inte**.

Mer information om det här alternativet, se [användarinloggning](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).

## <a name="next-steps"></a>Nästa steg
* Mer information om hello Konfigurationsmodell som används av Azure AD Connect-synkronisering i [förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
