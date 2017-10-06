---
title: "aaaUse grupper toomanage åtkomst tooresources i Azure Active Directory | Microsoft Docs"
description: "Hur toouse grupper i Azure Active Directory toomanage användaren åtkomst till lokala tooon och molnprogram och resurser."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 714120d0-cdf9-465d-afee-39bef591c6b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 876a356c8095505432e9346721f35c7943819e9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-access-tooresources-with-azure-active-directory-groups"></a>Hantera åtkomst tooresources med Azure Active Directory-grupper
Azure Active Directory (AD Azure) är en omfattande identitets- och hanteringslösning som tillhandahåller stabila funktioner toomanage åtkomst tooon lokala och molnprogram och resurser inklusive Microsofts onlinetjänster som Office 365 och en värld av icke - Microsoft SaaS-program. Den här artikeln innehåller en översikt, men om du vill använda Azure AD toostart grupper just nu, följer du anvisningarna hello i [hantera säkerhetsgrupper i Azure AD](active-directory-accessmanagement-manage-groups.md). Om du vill toosee hur du kan använda PowerShell toomanage grupper i Azure Active directory kan du läsa mer i [Azure Active Directory-cmdlets för grupphantering](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

> [!NOTE]
> toouse Azure Active Directory, du behöver ett Azure-konto. Om du inte har ett konto kan du [registrera dig för ett kostnadsfritt Azure-konto](https://azure.microsoft.com/pricing/free-trial/).
>
>

En av hello huvudfunktioner är hello möjlighet toomanage åtkomst tooresources i Azure AD. Dessa resurser kan vara en del av hello directory som hello fallet med behörigheter toomanage objekt genom roller i hello katalog eller resurser som är externa toohello katalog, t.ex SaaS-program, Azure-tjänster, och SharePoint-webbplatser eller lokala resurser. Det finns fyra olika sätt en användare kan tilldelas åtkomst rättigheter tooa resurs:

1. Direkttilldelning

    Användare kan tilldelas direkt tooa resurs av hello ägare av den här resursen.
2. Gruppmedlemskap

    En grupp kan tilldelas tooa resurs genom hello resurs-ägare och genom att göra det, bevilja hello medlemmar i gruppen åtkomst toohello resursen. Medlemskap i gruppen hello kan sedan hanteras av hello ägare hello grupp. Hello resurs ägare delegater hello effektivt, behörighet tooassign användare tootheir resurs toohello ägare hello gruppen.
3. Regelbaserad

    Hej resursägare kan använda en regel tooexpress vilka användare som ska tilldelas åtkomst tooa resurs. hello resultatet av hello regel beror på hello-attribut som används i regeln och deras värden för specifika användare, och därigenom hello resursägaren effektivt hello rätt toomanage åtkomst tootheir resurs toohello auktoritativ datakälla för hello attribut som används i hello regeln. Hej resursägare fortfarande hanterar själva hello regeln och avgör vilka attribut och värden ange åtkomst tootheir resurs.
4. Externa utfärdare

    hello åtkomst tooa resursen har härletts från en extern källa; till exempel en grupp som ska synkroniseras från en betrodd källa, till exempel en lokal katalog eller en SaaS-app, till exempel WorkDay. Hej resursägare tilldelar hello grupp tooprovide åtkomst toohello resurs och hello extern källa hanterar hello medlemmar i gruppen för hello.

   ![Översikt över access management diagram](./media/active-directory-access-management-groups/access-management-overview.png)

## <a name="watch-a-video-that-explains-access-management"></a>Se en video som förklarar åtkomsthantering
Du kan titta på en kort video som förklarar mer om detta:

**Azure AD: Introduktion toodynamic medlemskap för grupper**

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]
>
>

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Hur har tillgång till hantering i Azure Active Directory arbete?
Vid hello är mittpunkt hello Azure AD åtkomst hanteringslösning hello säkerhetsgrupp. Med hjälp av en säkerhet grupp toomanage åtkomst tooresources är en välkänd paradigmet som möjliggör en flexibel och lätt att förstå hur tooprovide åtkomst tooa resurs för hello avsedd användargrupp. Hej resursägare (eller Hej administratör för hello katalog) tilldela en grupp tooprovide en vissa åtkomst rätt toohello resurser de äger. hello medlemmar i hello gruppen kommer att tillhandahållas hello åtkomst och hello resursägare kan delegera hello rätt toomanage hello medlemslista för en grupp toosomeone annan, till exempel en avdelningschef eller administratör supportavdelningen.

![Azure Active Directory access management diagram](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

hello ägare för en grupp kan också göra gruppen tillgänglig för självbetjäning begäranden. På så sätt kan en slutanvändare söka efter och hitta hello grupperna och gör en begäran toojoin effektivt sökning behörighet tooaccess hello resurser som hanteras genom hello grupp. hello ägare hello gruppen kan ställa in hello gruppen så att anslutningsbegäranden godkänns automatiskt eller kräver godkännande av hello ägaren av hello grupp. När en användare gör en begäran toojoin en grupp, vidarebefordras hello kopplingsbegäran toohello ägare för hello grupp. Om en hello ägare godkänner hello begäran hello användaren meddelas och hello användare är anslutna toohello. Om en hello ägare nekar hello begäran hello användaren meddelas men ansluten inte toohello grupp.

## <a name="getting-started-with-access-management"></a>Komma igång med åtkomsthantering
Redo tooget igång? Du bör testa vissa av hello grundläggande uppgifter som du kan göra med Azure AD-grupper. Använd dessa funktioner tooprovide specialiserade access toodifferent grupper av personer för olika resurser i din organisation. Nedan visas en lista över grundläggande första steg.

* [Skapa en enkel regel tooconfigure dynamiskt medlemskap för en grupp](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)
* [Med hjälp av en grupp toomanage åtkomst tooSaaS program](active-directory-accessmanagement-group-saasapps.md)
* [Göra en grupp tillgänglig för slutanvändaren självbetjäning](active-directory-accessmanagement-self-service-group-management.md)
* [Synkroniserar en lokal grupp tooAzure med Azure AD Connect](active-directory-aadconnect.md)
* [Hantera ägare för en grupp](active-directory-accessmanagement-managing-group-owners.md)

## <a name="next-steps"></a>Nästa steg
Nu när du har förstått hello grunderna i åtkomsthantering finns här några ytterligare avancerade funktioner i Azure Active Directory för att hantera åtkomst tooyour program och resurser.

* [Med hjälp av attributen toocreate avancerade regler](active-directory-accessmanagement-groups-with-advanced-rules.md)
* [Hantera säkerhetsgrupper i Azure AD](active-directory-accessmanagement-manage-groups.md)
* [Ställa in dedikerade grupper i Azure AD](active-directory-accessmanagement-dedicated-groups.md)
* [Referens för Graph API för grupper](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)
* [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](active-directory-accessmanagement-groups-settings-cmdlets.md)
