---
title: "Lägga till ett eget domännamn i Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du lägger till ditt företags domännamn i Azure Active Directory och hur du verifierar namnet."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d97e57c6-578a-4929-8fb8-42e858a711c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: ad72f768add7edc1d34a85c27dc2aa1b4e4b3a50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-a-custom-domain-name-to-azure-active-directory"></a>Lägga till ett anpassat domännamn i Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure-portal](active-directory-domains-add-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-add-domain.md)
> 

Du kan lägga till företagets domännamn till Azure AD samt använder Azure Active Directory (AD Azure). Du kan ha ett domännamn som du organisation använder för att göra företag och användare som loggar in med företagets domännamn. Lägga till domännamnet i Azure AD kan du tilldela användarnamn i katalogen som dina användare som ”alice@contoso.com”. Processen är enkel:

1. Lägga till det anpassade domännamnet i din katalog
2. Lägga till en DNS-post för domännamnet hos domännamnsregistratorn
3. Verifiera det anpassade domännamnet i Azure AD

## <a name="how-do-i-add-a-domain-name"></a>Hur lägger jag till ett domännamn?
1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.
2. Välj **fler tjänster**, ange **Azure Active Directory** i textrutan och välj sedan **RETUR**.
   
   ![Öppna användarhantering](./media/active-directory-domains-add-azure-portal/user-management.png)
3. På den ***katalognamn*** bladet väljer **domännamn**.
4. På den  ***katalognamn* -domännamn** bladet väljer den **Lägg till** kommando.
   
   ![Att välja kommandot Lägg till](./media/active-directory-domains-add-azure-portal/add-command.png)
5. På den **domännamn** bladet anger du namnet på din domän i rutan, till exempel ”contoso.com” och välj sedan **Lägg till domän**. Glöm inte att ta med .com, .net eller ett annat tillägg för toppnivån.
6. På den ***domainname*** bladet (det vill säga bladet som öppnas som har ett nytt domännamn i namnet), hämta information om DNS-posten som Azure AD för att kontrollera att din organisation äger det anpassade domännamnet.
   
   ![Hämta information om DNS-post](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Nu när du har lagt till domännamnet måste Azure AD kontrollera att din organisation äger domännamnet. Innan Azure AD kan utföra den här kontrollen måste du lägga till en DNS-post i DNS-zonfilen för domännamnet. Den här uppgiften utförs på webbplatsen för domännamnsregistratorn för domännamnet.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Lägg till DNS-posten hos domänens domännamnsregistrator
Nästa steg mot att kunna använda ditt anpassade domännamn med Azure AD är att du uppdaterar domänens DNS-zonfil. Detta gör det möjligt för Azure AD att verifiera att din organisation äger det anpassade domännamnet.

1. Logga in hos domännamnsregistratorn för domänen. Om du inte har den åtkomst som krävs för att uppdatera DNS-posten så be den person eller grupp i din organisation som har denna åtkomst att utföra steg 2 och att meddela dig när det är klart.
2. Uppdatera DNS-zonfilen för domänen genom att lägga till DNS-posten som du fått från Azure AD. Den här DNS-posten gör att Azure AD kan verifiera ditt ägarskap för domänen. DNS-posten ändrar inga beteenden, till exempel e-postdirigering eller webbhosting.

Om du behöver hjälp med att lägga till DNS-posten kan du läsa [Anvisningar om hur man lägger till en DNS-post hos en vanlig DNS-registrator](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Verifiera domännamnet med Azure AD
När du har lagt till DNS-posten är du redo att verifiera domännamnet med Azure AD.

Ett domännamn kan verifieras endast när DNS-poster har spridits. Spridningen tar ofta bara några sekunder, men kan ibland ta en timme eller mer. Om verifieringen inte fungerar första gången försöker du igen lite senare.

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.
2. Välj **Bläddra**, ange användarhantering i textrutan och välj sedan **RETUR**.
   
   ![Öppna användarhantering](./media/active-directory-domains-add-azure-portal/user-management.png)
3. På den **Användarhantering - domännamn** bladet välj overifierade domännamnet som du vill kontrollera.
4. På den ***domainname*** bladet (det vill säga bladet som öppnas som har ett nytt domännamn i namnet), Välj **Kontrollera** Slutför verifieringen.

Nu kan du [tilldela användarnamn som innehåller ditt domännamn](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Felsökning
Om du inte kan verifiera ett anpassat domännamn så pröva med följande. Vi börjar med de vanligaste och arbetar oss vidare till de minst vanliga.

1. **Vänta en timma**. DNS-posterna måste spridas innan Azure AD kan verifiera domänen. Detta kan ta en timma eller mer.
2. **Kontrollera att DNS-posten har angetts och att den är korrekt**. Utför det här steget på webbplatsen för domännamnsregistratorn för domänen. Azure AD kan inte verifiera domännamnet om DNS-posten inte finns i DNS-zonfilen, eller om den inte exakt matchar DNS-posten från Azure AD. Om du inte har den åtkomst som krävs för att uppdatera DNS-poster för domänen hos domännamnsregistratorn så dela DNS-posten med den person eller grupp i din organisation som har den åtkomst som krävs och be dem att lägga till DNS-posten.
3. **Ta bort domännamnet från en annan katalog i Azure AD**. Ett domännamn kan bara verifieras i en enskild katalog. Om ett domännamn tidigare verifierades i en annan katalog måste du ta bort det därifrån innan du kan verifiera det i din nya katalog. Mer information om hur du tar bort domännamn finns i [Hantera anpassade domännamn](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Lägga till fler anpassade domännamn
Om din organisation använder flera anpassade domännamn, t.ex. contoso.com och contosobank.com, kan du lägga till dem. Du kan lägga till upp till 900 domännamn. Lägg till vart och ett av dina domännamn genom att följa samma steg i den här artikeln.

## <a name="next-steps"></a>Nästa steg
[Hantera egna domännamn](active-directory-domains-manage-azure-portal.md)

