---
title: "aaaAdd ditt anpassade domännamn tooAzure Active Directory | Microsoft Docs"
description: "Hur tooadd ditt företags domännamn tooAzure Active Directory och hur tooverify hello domännamn."
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
ms.openlocfilehash: 88d5f443cd10b098a9a9ffb3137f5e1ca33b6aad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-custom-domain-name-tooazure-active-directory"></a>Lägg till en anpassad domän namn tooAzure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-domains-add-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-add-domain.md)
> 

Med Azure Active Directory (Azure AD) kan du lägga till din företagsdomän namn tooAzure AD samt. Du kanske har ett domännamn som din organisation använder toodo verksamhet och användare som loggar in med företagets domännamn. Om du lägger till hello domain name tooAzure AD kan tooassign användarnamn i hello katalog som är bekant tooyour användare, till exempel ”alice@contoso.com”. hello-processen är enkel:

1. Lägg till hello domänen namnet tooyour katalog
2. Lägg till en DNS-post för hello domännamn i hello domännamnsregistratorn
3. Kontrollera hello domännamn i Azure AD

## <a name="how-do-i-add-a-domain-name"></a>Hur lägger jag till ett domännamn?
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **Azure Active Directory** i hello textruta och välj sedan **RETUR**.
   
   ![Öppna användarhantering](./media/active-directory-domains-add-azure-portal/user-management.png)
3. På hello ***katalognamn*** bladet väljer **domännamn**.
4. På hello  ***katalognamn* -domännamn** bladet, Välj hello **Lägg till** kommando.
   
   ![Att välja hello tilläggskommando](./media/active-directory-domains-add-azure-portal/add-command.png)
5. På hello **domännamn** bladet anger hello namnet på domänen, till exempel ”contoso.com” i rutan hello och välj sedan **Lägg till domän**. Vara säker på att tooinclude hello .com, .net eller andra tillägg för toppnivån.
6. På hello ***domainname*** bladet (det vill säga hello bladet som öppnas med ett nytt domännamn i hello rubrik) Hämta hello DNS-posten information som Azure AD ska använda tooverify att din organisation äger hello domännamn.
   
   ![Hämta information om DNS-post](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Nu när du har lagt till domännamnet för hello Kontrollera Azure AD att din organisation äger hello domännamn. Innan Azure AD kan utföra denna verifiering måste du lägga till en DNS-post i hello DNS-zonfilen för hello domännamn. Den här uppgiften utförs på hello webbplatsen för domännamnsregistratorn för domännamnet hello.

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Lägg till hello DNS-posten i hello domännamnsregistratorn för hello domän
hello nästa steg toouse ditt anpassade domännamn med Azure AD är tooupdate hello DNS-zonfilen för hello domän. Detta gör att Azure AD tooverify att din organisation äger hello domännamn.

1. Logga in toohello domännamnsregistratorn för hello domän. Om du inte har åtkomst tooupdate hello DNS-post, be hello person eller team som har den här åtkomst toocomplete steg 2 och toolet som du vet när den har slutförts.
2. Uppdatera hello DNS-zonfilen för domänen hello genom att lägga till tooyou för hello DNS-posten som tillhandahålls av Azure AD. Den här DNS-posten gör Azure AD tooverify ditt ägarskap för hello domän. hello DNS-posten ändrar inga beteenden, till exempel e-postdirigering eller webbhosting.

Hjälp med att lägga till hello DNS-posten, läsa [för att lägga till en DNS-post på populära DNS-registratorer](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-hello-domain-name-with-azure-ad"></a>Kontrollera hello domännamnet med Azure AD
När du har lagt till hello DNS-post, är du redo tooverify hello domännamnet med Azure AD.

Ett domännamn kan verifieras endast när hello DNS-poster har spridits. Spridningen tar ofta bara några sekunder, men kan ibland ta en timme eller mer. Om verifieringen inte fungerar hello första gången, försök igen senare.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **Bläddra**, ange användarhantering i hello och väljer sedan **RETUR**.
   
   ![Öppna användarhantering](./media/active-directory-domains-add-azure-portal/user-management.png)
3. På hello **Användarhantering - domännamn** bladet, Välj hello overifierade domännamn som du vill tooverify.
4. På hello ***domainname*** bladet (det vill säga hello bladet som öppnas med ett nytt domännamn i hello rubrik) Välj **Kontrollera** toocomplete hello verifiering.

Nu kan du [tilldela användarnamn som innehåller ditt domännamn](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Felsökning
Om du inte kan verifiera ett eget domännamn kan du försöka hello följande. Vi börjar med hello vanlig och arbets ned toohello minst vanliga.

1. **Vänta en timma**. DNS-poster måste toopropagate innan Azure AD kan verifiera hello domän. Detta kan ta en timma eller mer.
2. **Kontrollera hello DNS-posten har angetts och att den är korrekt**. Slutför det här steget på hello webbplatsen för hello domännamnsregistratorn för hello domän. Azure AD kan inte verifiera hello domännamnet om hello DNS-posten inte finns i hello DNS-zonfilen eller om det inte är en exakt matchning med hello DNS-posten som Azure AD tillhandahåller. Om du inte har åtkomst tooupdate DNS-poster för domänen som hello hello domännamnsregistratorn dela hello DNS-post med hello person eller det team i din organisation som har nödvändig behörighet och be dem tooadd hello DNS-post.
3. **Ta bort hello domännamn från en annan katalog i Azure AD**. Ett domännamn kan bara verifieras i en enskild katalog. Om ett domännamn tidigare verifierades i en annan katalog måste du ta bort det därifrån innan du kan verifiera det i din nya katalog. toolearn om att ta bort domännamn, läsa [hantera egna domännamn](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Lägga till fler anpassade domännamn
Om din organisation använder flera anpassade domännamn, till exempel ”contoso.com” och ”contosobank.com”, du kan lägga till dem upp tooa upp till 900 domännamn. Använd hello samma steg i den här artikeln tooadd varje av ditt domännamn.

## <a name="next-steps"></a>Nästa steg
[Hantera egna domännamn](active-directory-domains-manage-azure-portal.md)

