---
title: "Azure AD Connect: Nästa steg och hur toomanage Azure AD Connect | Microsoft Docs"
description: "Lär dig hur tooextend hello standardkonfigurationen och operativa uppgifter för Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4404aaff24d54d76b83baca3b331a6a250ba4c03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="next-steps-and-how-toomanage-azure-ad-connect"></a>Nästa steg och hur toomanage Azure AD Connect
Använd hello operativa procedurer i den här artikeln toocustomize Azure Active Directory (AD Azure) Anslut toomeet din organisations behov och krav.  

## <a name="add-additional-sync-admins"></a>Lägg till ytterligare sync-administratörer
Som standard är endast hello-användaren som hello installation och lokala administratörer kan toomanage hello installerat Synkroniseringsmotorn. För andra personer toobe kan tooaccess och hantera hello Synkroniseringsmotorn, leta upp hello-gruppen ADSyncAdmins på hello lokala servern och lägga till dem toothis grupp.

## <a name="assign-licenses-tooazure-ad-premium-and-enterprise-mobility-suite-users"></a>Tilldela licenser tooAzure AD Premium och Enterprise Mobility Suite-användare
Nu när användarna har synkroniserats toohello moln, behöver du tooassign dem en licens så att de kan komma igång med molnappar som Office 365.

### <a name="tooassign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>tooassign en Azure AD Premium eller Enterprise Mobility Suite-licens

1. Logga in toohello Azure-portalen som en administratör.
2. Hello vänster markerar **Active Directory**.
3. På hello **Active Directory** dubbelklickar hello directory som har hello-användare som du vill att tooset.
4. Överst hello i hello katalogsidan, Välj **licenser**.
5. På hello **licenser** väljer **Active Directory Premium** eller **Enterprise Mobility Suite**, och klicka sedan på **tilldela**.
6. Välj hello användare tooassign licenser till, och klicka sedan på hello markerat ikonen toosave hello ändringar hello i dialogrutan.

## <a name="verify-hello-scheduled-synchronization-task"></a>Verifiera hello schemalagd synkroniseringsaktiviteten
Använd hello Azure portal toocheck hello status för en synkronisering.

### <a name="tooverify-hello-scheduled-synchronization-task"></a>tooverify hello schemalagd synkronisering aktivitet
1. Logga in toohello Azure-portalen som en administratör.
2. Hello vänster markerar **Active Directory**.
3. På hello **Active Directory** dubbelklickar hello directory som har hello-användare som du vill att tooset.
4. Överst hello i hello katalogsidan, Välj **katalogintegrering**.
5. Under **integrering med lokala active directory**, Observera hello tid för senaste synkronisering.

<center>![Tid för Directory-synkronisering](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="start-a-scheduled-synchronization-task"></a>Starta en schemalagd synkronisering-aktivitet
Om du behöver toorun en synkroniseringsuppgiften kan göra du detta genom att köra hello Azure AD Connect-guiden igen.  Du måste tooprovide dina autentiseringsuppgifter för Azure AD.  Hello i guiden väljer du hello **anpassa synkroniseringsalternativ** och på **nästa** toomove hello guiden. Se till att hello i slutet av hello **startar hello synkroniseringsprocessen så snart hello initiala konfigurationen är klar** är markerad.

<center>![Starta synkroniseringen](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Mer information om hello Azure AD Connect sync Scheduler finns [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Ytterligare uppgifter som är tillgängliga i Azure AD Connect
Efter den inledande installationen av Azure AD Connect, kan du alltid starta hello guiden igen från hello Azure AD Connect start sida eller fjärrskrivbord snabbmenyn.  Du kommer att märka att gå igenom guiden hello igen ger nya alternativ i hello form av ytterligare uppgifter.  

hello följande tabell innehåller en sammanfattning av dessa uppgifter och en kort beskrivning av varje aktivitet.

![Lista över ytterligare aktiviteter](./media/active-directory-aadconnect-whats-next/addtasks.png)

| Ytterligare uppgifter | Beskrivning |
| --- | --- |
| **Visa hello valda scenario** |Visa din aktuella Azure AD Connect-lösning.  Detta inkluderar allmänna inställningar synkroniseras kataloger och synkroniseringsinställningar. |
| **Anpassa synkroniseringsalternativ** |Ändra hello aktuella konfigurationen som lägger till ytterligare Active Directory-skogar toohello konfiguration eller aktivera synkroniseringsalternativ, till exempel användare, grupp, enhet eller tillbakaskrivning av lösenord. |
| **Aktivera Mellanlagringsläge** |Steg-information som synkroniseras inte omedelbart och kan inte exporteras tooAzure AD eller lokala Active Directory.  Med den här funktionen kan du förhandsgranska hello synkroniseringar innan de inträffar. |

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
