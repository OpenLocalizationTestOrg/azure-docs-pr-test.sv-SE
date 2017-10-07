---
title: "aaaHow Azure-prenumerationer är associerade med Azure Active Directory | Microsoft Docs"
description: Logga in tooMicrosoft Azure och relaterad information, till exempel hello relationen mellan en Azure-prenumeration och Azure Active Directory.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/24/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 4f831cfb972efec57083fcaa63adb43fde7b2faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>Hur Azure-prenumerationer är associerade med Azure Active Directory
Den här artikeln innehåller information om hello relationen mellan en Azure-prenumeration och Azure Active Directory (Azure AD), och hur tooadd en befintlig prenumeration tooyour Azure AD-katalog.

## <a name="your-azure-subscriptions-relationship-tooazure-ad"></a>Din Azure-prenumeration relationen tooAzure AD
Azure-prenumerationen har en förtroenderelation med Azure AD, vilket innebär att den litar hello directory tooauthenticate användare, tjänster och enheter. Flera prenumerationer kan lita hello samma katalog, men varje prenumeration litar bara på en katalog. 

hello betrodd relation med en prenumeration med en katalog skiljer sig från hello relation som har med andra resurser i Azure (webbplatser, databaser och så vidare). Om en prenumeration går ut kommer du åt toohello stoppas även andra resurser som är associerade med hello prenumeration. Men en Azure AD-katalog finns kvar i Azure, och du kan associera en annan prenumeration med katalogen och hantera hello directory med hello ny prenumeration.

![diagram över hur prenumerationer är associerade](./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png)

Alla användare har en enda arbetskatalog som autentiserar dem, men de kan även vara gäster i andra kataloger. Du kan se hello kataloger som ditt användarkonto är medlem eller gäst i Azure AD.

## <a name="azure-ad-and-cloud-service-subscriptions"></a>Azure AD och molntjänstprenumerationer
Azure AD innehåller hello kärnor och identitetshanteringsfunktioner bakom de flesta av Microsofts molntjänster, inklusive:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Du får hello kostnadsfria Azure AD-tjänsten när du registrerar dig för någon av dessa Microsoft-molntjänster. Om du vill tooadd en ytterligare Azure-prenumeration tooan Azure AD-katalog, måste du vara inloggad med ett Microsoft-konto. Om du loggar in tooAzure med ett arbets- eller skolkonto, kan du lägga till en befintlig katalog tooan för Azure-prenumeration eftersom ditt arbets- eller skolkonto konto inte kan autentiseras direkt av Azure. 

## <a name="tooadd-an-existing-subscription-tooyour-azure-ad-directory"></a>tooadd en befintlig prenumeration tooyour Azure AD-katalog
Du måste logga in med ett konto som finns i båda hello aktuell katalog prenumerationen är associerad med vilka hello och hello directory du vill att tooadd den. 

1. Logga in toohello [Azure Kontocenter](https://account.windowsazure.com/Home/Index) med ett konto som är hello kontoadministratör för hello prenumerationen vars ägarskap önskade tootransfer.
2. Se till att hello som användaren har toobe hello prenumerationsägaren finns i katalogen för hello mål.
3. Klicka på **överföra äganderätten till prenumerationen**.
4. Ange hello mottagare. hello mottagare får automatiskt ett e-postmeddelande med en länk för godkännande.
5. hello mottagaren klickar hello länken och följer hello instruktioner, inklusive att ange sina betalningsinformation. När hello mottagaren lyckas, överförs hello prenumeration. 
6. hello standardkatalogen hello prenumeration ändras toohello directory hello användaren tillhör.


## <a name="suggestions-toomanage-both-a-subscription-and-a-directory"></a>Förslag toomanage både en prenumeration och en katalog
hello administrativa roller för en Azure-prenumeration hanterar resurser som är knutna toohello Azure-prenumeration. Det här avsnittet beskrivs hello skillnader mellan Azure-prenumerationsadministratörer och Azure AD-katalogadministratörer. Administrativa roller och andra förslag för att använda dem toomanage din prenumeration beskrivs i [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md).

Som standard tilldelas rollen som tjänstadministratör hello när du registrerar dig används. Om andra behöver toosign i och komma åt tjänster med hello samma prenumeration, du kan lägga till dem som medadministratörer. 

Azure AD har en annan uppsättning administrativa roller toomanage hello katalog- och identitetsrelaterade funktioner. Hello global administratör för en katalog kan till exempel lägga till användare och grupper toohello directory eller kräva Multi-Factor authentication för användare. När en användare skapar en katalog tilldelas rollen som global administratör toohello och kan tilldela administratörsroller tooother användare. Administrativa roller i Azure AD används också av andra tjänster som Office 365 och Microsoft Intune. 

Azure-prenumerationsadministratörer och Azure AD-katalogadministratörer är två separata roller. 
* Azure-prenumerationsadministratörer kan hantera resurser i Azure och kan använda Azure AD i hello Azure-portalen (eftersom hello Azure-portalen själva är en Azure-resurs). 
* Katalogadministratörer kan hantera egenskaper endast i hello Azure AD-katalog.

En person kan ha båda rollerna men det är inget krav. En global katalogadministratör kanske inte kan tilldelas rollen som tjänstadministratör eller medadministratör för en Azure-prenumeration eller tvärtom. Utan att vara administratör för prenumeration hello hello användare kan logga in toohello Azure-portalen, men kan inte hantera hello kataloger för den prenumerationen i hello-portalen. Den här användaren kan dock hantera kataloger med andra verktyg, till exempel Azure AD PowerShell eller hello Office 365 Admin Center.

## <a name="next-steps"></a>Nästa steg
* toolearn mer om hur toochange administratörer för en Azure-prenumeration, se [överför ägarskapet för ett Azure-prenumeration tooanother konto](../billing/billing-subscription-transfer.md)
* toolearn mer information om hur resursåtkomsten hanteras i Microsoft Azure finns [förstå resursåtkomst i Azure](active-directory-understanding-resource-access.md)
* Mer information om hur tooassign roller i Azure AD finns [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
