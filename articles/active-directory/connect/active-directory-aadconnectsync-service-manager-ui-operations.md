---
title: "Azure AD Connect Synchronization Service Manager-åtgärder | Microsoft Docs"
description: "Förstå hello Operations-fliken i hello Synchronization Service Manager för Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: decbc53613d456a71cd116c40c5e1fd761efd4af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-sync-service-manager-operations-tab"></a>Med hjälp av hello fliken synkronisering Service Manager-åtgärder

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

hello visar operations hello resultaten från de senaste hello-åtgärder. Den här fliken är viktiga toounderstand och felsöka problem.

## <a name="understand-hello-information-visible-in-hello-operations-tab"></a>Förstå hello information visas i hello operations fliken
hello övre delen visas alla körs i kronologisk ordning. Som standard hello operations loggen sparas information om hello senaste sju dagarna, men den här inställningen kan ändras med hello [scheduler](active-directory-aadconnectsync-feature-scheduler.md). Vill du toolook för alla kör som inte visar statusen lyckades. Du kan ändra hello sortering genom att klicka på hello-huvuden.

Hej **Status** kolumnen är hello viktigaste informationen och visar hello mest allvarliga problem för en körning. Här är en kort sammanfattning av hello vanligaste status efter prioritet tooinvestigate (där * ange flera möjliga felsträngar).

| Status | Kommentar |
| --- | --- |
| Stoppad-* |hello kör kunde inte slutföras. Till exempel om hello fjärrsystemet är inte tillgänglig och kan inte kontaktas. |
| stoppats felgränsen |Det finns fler än 5 000 fel. hello kör stoppades automatiskt på grund av toohello många fel. |
| slutförda -\*-fel |hello kör slutfördes, men det finns fel (färre än 5 000) som bör undersökas. |
| slutförda -\*-varningar |hello kör slutfördes, men vissa data är inte i hello förväntat tillstånd. Om du har fel sedan är det här meddelandet vanligtvis bara ett symtom. Du bör inte undersöka varningar förrän du har åtgärdat felen. |
| lyckades |Inga problem. |

När du har valt en rad uppdateras hello nedre tooshow hello information om som körs. toohello längst till vänster i hello längst ned, kanske du har en lista som säger **steg #**. Den här listan visas bara om du har flera domäner i skogen där varje domän som representeras av ett steg. hello domännamn finns under rubriken hello **Partition**. Under **Synkroniseringsstatistik**, du kan hitta mer information om hello antalet ändringar som har bearbetats. Du kan klicka på hello länkar tooget en lista över hello ändras objekt. Om du har objekt med fel felen visas **synkroniseringsfel**.

Mer information finns i [felsökning av ett objekt som inte synkroniseras](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)

## <a name="next-steps"></a>Nästa steg
Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
