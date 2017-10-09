---
title: "aaaSetting in Azure Active Directory för hantering av självbetjäningsportalen åtkomst | Microsoft Docs"
description: "Grupphantering via självbetjäning kan användare toocreate och hantera säkerhetsgrupper eller Office 365-grupper i Azure Active Directory och ger användarna hello möjligheten toorequest säkerhetsgrupp eller Office 365-gruppmedlemskap"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 904d5c70-c34a-46c4-a9a7-d1efecf4821c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/27/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 2a73f4ed2532d41143fe5abe2fef1322d971a5c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Konfigurera Azure Active Directory för grupphantering via självbetjäning
Grupphantering via självbetjäning kan användare toocreate och hantera säkerhetsgrupper eller Office 365-grupper i Azure Active Directory (AD Azure). Användarna kan också begära säkerhetsgrupp eller Office 365-gruppmedlemskap och sedan hello ägare hello gruppen kan godkänna eller neka medlemskap. På så sätt kan kan dagliga kontrollen av gruppmedlemskap vara delegerade toopeople som förstår hello affärskontexten för medlemskapet. Funktioner för grupphantering via självbetjäning är bara tillgängliga för säkerhetsgrupper och Office 365-grupper, men inte för e-postaktiverade säkerhetsgrupper eller distributionslistor.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.

Grupphantering via självbetjäning består för närvarande av två viktiga scenarier: delegerad grupphantering och grupphantering via självbetjäning.

* **Delegerad grupphantering** ett exempel är en administratör som hanterar åtkomst tooa SaaS-program som hello företaget använder. Har blivit svårt att hantera dessa åtkomstbehörigheter ber administratören hello företag ägare toocreate en ny grupp. Hej administratör tilldelas åtkomst för hello toohello ny programgrupp och lägger till toohello grupp alla personer som redan kommer åt toohello program. hello företagsägaren sedan kan lägga till fler användare och användarna har automatiskt etablerade toohello program. hello företagsägaren behöver inte toowait för hello toomanage administratörsåtkomst för användare. Om Hej administratör ger hello samma behörighet tooa manager i en annan affärsgrupp sedan personen kan också hantera åtkomsten för sina egna användare. Varken hello företagsägaren eller hello manager kan visa eller hantera varandras användare. Hej administratör kan fortfarande se alla användare som har åtkomst toohello programmet och blockera behörigheten om det behövs.
* **Grupphantering via självbetjäning** Ett exempel på det här scenariot är två användare som båda har SharePoint Online-platser som de har konfigurerat oberoende av varandra. De vill toogive varje andras team åtkomst tootheir platser. tooaccomplish, de kan skapa en grupp i Azure AD och i SharePoint Online väljer var och en av dem som gruppen tooprovide åtkomst tootheir platser. När någon vill ha åtkomst begär de det från hello åtkomstpanelen och efter godkännande får de åtkomst tooboth SharePoint Online-platserna automatiskt. Senare beslutar en av dem att alla användare åtkomst till hello plats även ska få åtkomst till tooa visst SaaS-program. Hej administratör hello SaaS-program kan lägga till åtkomstbehörigheter för hello programmet toohello SharePoint Online-webbplats. Därefter ger alla begäranden som godkännas åtkomst toohello två SharePoint Online-platserna och toothis SaaS-program.

## <a name="making-a-group-available-for-end-user-self-service"></a>Göra en grupp tillgänglig för självbetjäning av slutanvändare
1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), öppna Azure AD-katalogen.
2. På hello **konfigurera** ställer du in **delegerad grupphantering** tooEnabled.
3. Ange **användare kan skapa säkerhetsgrupper** eller **användare kan skapa grupper för Office** tooEnabled.

När **användare kan skapa säkerhetsgrupper** är aktiverat kan alla användare i din katalog tillåts toocreate nya säkerhetsgrupper och lägga till medlemmar toothese grupper. Dessa nya grupper visas också i hello åtkomstpanelen för alla andra användare. Om hello principinställningen för gruppen hello kan kan andra användare skapa förfrågningar toojoin dessa grupper. Om du har inaktiverat **Användare kan skapa säkerhetsgrupper** så kan användarna inte skapa grupper eller ändra de befintliga grupper som de äger. De kan fortfarande hantera hello medlemskap i dessa grupper och godkänna förfrågningar från andra användare toojoin deras grupper.

Du kan också använda **användare som kan använda Självbetjäning för säkerhetsgrupper** tooachieve en mer detaljerad åtkomstkontroll över grupphantering via Självbetjäning för dina användare. När **användare kan skapa grupper** är aktiverat kan alla användare i din katalog tillåts toocreate nya grupper och lägga till medlemmar toothese grupper. Genom att även ange **användare som kan använda Självbetjäning för säkerhetsgrupper** tooSome, du är att begränsa gruppen management tooonly en begränsad grupp användare. När den här växeln anges tooSome, måste du lägga till användargruppen toohello SSGMSecurityGroupsUsers innan de kan skapa nya grupper och lägga till medlemmar toothem. Genom att ange **användare som kan använda Självbetjäning för säkerhetsgrupper** tooAll, användarna i din katalog toocreate nya grupper.

Du kan också använda hello **grupp som kan använda Självbetjäning för säkerhetsgrupper** rutan toospecify ett eget namn för en grupp vars medlemmar kan använda självbetjäning.

## <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Vad är Azure Active Directory?](active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
