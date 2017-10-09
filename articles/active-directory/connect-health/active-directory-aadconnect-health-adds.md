---
title: aaaUsing Azure AD Connect Health med AD DS | Microsoft Docs
description: "Det här är hello Azure AD Connect Health sida som innehåller information om hur toomonitor AD DS."
services: active-directory
documentationcenter: 
author: arluca
manager: femila
editor: curtand
ms.assetid: 19e3cf15-f150-46a3-a10c-2990702cd700
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: e2fb6be65407d02c214dcab385b85d6cb54f48de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Använda Azure AD Connect Health med AD DS
hello följande dokumentation är specifik toomonitoring Active Directory Domain Services med Azure AD Connect Health. hello stöds versioner av AD DS: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 och Windows Server 2016.

Mer information om övervakning av AD FS med Azure AD Connect Health finns i [ Använda Azure AD Connect Health med AD FS](active-directory-aadconnect-health-adfs.md). Mer information om övervakning av Azure AD Connect (Sync) med Azure AD Connect Health finns i [Använda Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md).

![Azure AD Connect Health för AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Aviseringar för Azure AD Connect Health för AD DS
Hej aviseringar avsnitt i Azure AD Connect Health för AD DS, ger dig en lista över aktiva och lösta aviseringar relaterade tooyour domänkontrollanter. Om du väljer en aktiv eller åtgärdad avisering öppnas ett nytt blad med ytterligare information, tillsammans med Lösningssteg, och länkar toosupporting dokumentation. Varje typ av avisering kan ha en eller flera instanser som tooeach hello domänkontrollanter som berörs av den specifika aviseringen. Du kan dubbelklicka på en berörda domain controller tooopen ett ytterligare blad med mer information om aviseringen instansen hello nedre delen av aviseringen hello-bladet.

I det här bladet kan du aktivera e-postmeddelanden för aviseringar och ändra hello tidsintervall i vyn. Expandera hello tidsintervallet kan du toosee tidigare lösta aviseringar.

![Azure AD Connect-synkroniseringsfel](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Instrumentpanel för domänkontrollanter
Den här instrumentpanelen innehåller en topologisk vy över din miljö, tillsammans med operativa nyckelvärden och hälsostatus för var och en av dina övervakade domänkontrollanter. hello presenteras mått hjälp tooquickly identifiera, domänkontrollanter som kan kräva ytterligare undersökning. Som standard visas bara en delmängd av hello kolumner. Dock kan du hitta hello hela uppsättningen med tillgängliga kolumner genom att dubbelklicka på hello kolumner kommando. Att välja hello kolumner som mest intresserar dig, aktiverar den här instrumentpanelen till en enda och enkelt placera tooview hello hälsotillståndet för AD DS-miljön.

![Domänkontrollanter](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Domänkontrollanter kan grupperas efter deras respektive domän eller plats, vilket är användbart för att förstå hello miljö topologi. Om du dubbelklickar på hello bladet sidhuvud slutligen maximerar tooutilize hello tillgängliga skärmen-fastigheter hello instrumentpanelen. Den här större vyn är användbar när flera kolumner visas.

## <a name="replication-status-dashboard"></a>Instrumentpanelen Replikeringsstatus
Den här instrumentpanelen innehåller en vy över hello status och replikering replikeringstopologin övervakade domänkontrollanter. Hej senaste replikeringsförsöket hello status visas tillsammans med bra dokumentationen för eventuella fel som hittas. Du kan dubbelklicka på en domänkontrollant med ett fel tooopen ett nytt blad med information som: information om hello fel, rekommenderas Lösningssteg, och länkar tootroubleshooting dokumentation.

![Replikeringsstatus](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Övervakning
Denna funktion tillhandahåller grafiska trender olika prestandaräknare som kontinuerligt samlas in från varje hello övervakade domänkontrollanter. En domänkontrollants prestanda kan enkelt jämföras med alla övriga övervakade domänkontrollanter i skogen. Dessutom visas olika prestandaräknare sida vid sida, vilket är användbart när du felsöker problem i din miljö.

![Övervakning](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Som standard har vi förvald fyra prestandaräknare; Du kan dock innehålla andra genom att klicka på kommandot för hello filter och att markera eller avmarkera alla prestandaräknare som önskade. Du kan dessutom Dubbelklicka på en prestandaräknaren diagram tooopen ett nytt blad som innehåller datapunkter för varje hello övervakade domänkontrollanter.

## <a name="related-links"></a>Relaterade länkar
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Installation av Azure AD Connect Health Agent](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-åtgärder](active-directory-aadconnect-health-operations.md)
* [Använda Azure AD Connect Health med AD FS](active-directory-aadconnect-health-adfs.md)
* [Använda Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md)
* [Vanliga frågor och svar om Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versionshistorik för Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)

