---
title: "aaaAdd ditt anpassade domännamn tooAzure Active Directory | Microsoft Docs"
description: "Hur tooadd ditt företags domännamn tooAzure Active Directory och hur tooverify hello domännamn."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 35a6e20a-9907-432b-9d36-16b916a5c249
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: eb208138f2633aaecc54f68dc947caf80d856d23
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
> 

Du har en eller flera domännamn som din organisation använder toodo företag och dina användare loggar in tooyour företagsnätverket med hjälp av företagets domännamn. Nu när du använder Azure Active Directory (Azure AD) kan du lägga till din företagsdomän namn tooAzure AD samt. Detta gör att du tooassign användarnamn i hello katalog som är bekant tooyour användare, till exempel ”alice@contoso.com”. hello-processen är enkel:

1. Lägg till hello domänen namnet tooyour katalog
2. Lägg till en DNS-post för hello domännamn i hello domännamnsregistratorn
3. Kontrollera hello domännamn i Azure AD

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. För hur tooadd företagets domännamn i administrationscentret för hello Azure AD, se [Tilldela administratörsroller i Azure Active Directory](active-directory-domains-add-azure-portal.md).

Om du planerar tooconfigure din anpassade domän namn toobe används med Active Directory Federation Services (AD FS) eller en annan säkerhetstokentjänst (STS) i företagsnätverket, följer du anvisningarna för hello i [Lägg till och konfigurera en domän för Federation med Azure Active Directory](active-directory-add-domain-federated.md). Detta är användbart om du planerar toosynchronize användare från din företagets directory tooAzure AD, och [lösenordshashsynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md) inte uppfyller dina krav.

## <a name="add-a-custom-domain-name-tooyour-directory"></a>Lägga till en anpassad domän namn tooyour katalog
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) med ett användarkonto som är en global administratör i Azure AD-katalogen.
2. I **Active Directory**, öppna katalogen och välj hello **domäner** fliken.
3. Markera på hello kommandofältet **Lägg till**. Ange hello namnet på din domän, till exempel ”contoso.com”. Kontrollera att tooinclude hello .com, .net eller ett annat tillägg för toppnivån och lämna hello kryssrutan för ”enkel inloggning” (federation) rensas.
4. Välj **Lägg till**.
5. Hämta hello DNS-posten som Azure AD ska använda tooverify att din organisation äger hello domännamn på hello andra sidan i guiden för hello lägga till domäner.

Nu när du har lagt till domännamnet för hello Kontrollera Azure AD att din organisation äger hello domännamn. Innan Azure AD kan utföra denna verifiering måste du lägga till en DNS-post i hello DNS-zonfilen för hello domännamn. Den här uppgiften utförs på hello webbplatsen för domännamnsregistratorn för domännamnet hello.

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Lägg till hello DNS-posten i hello domännamnsregistratorn för hello domän
hello nästa steg toouse ditt anpassade domännamn med Azure AD är tooupdate hello DNS-zonfilen för hello domän. Detta gör att Azure AD tooverify att din organisation äger hello domännamn.

1. Logga in toohello domännamnsregistratorn för hello domän. Om du inte har åtkomst tooupdate hello DNS-post, be hello person eller team som har den här åtkomst toocomplete steg 2 och toolet som du vet när den har slutförts.
2. Uppdatera hello DNS-zonfilen för domänen hello genom att lägga till tooyou för hello DNS-posten som tillhandahålls av Azure AD. Den här DNS-posten gör Azure AD tooverify ditt ägarskap för hello domän. hello DNS-posten ändrar inga beteenden, till exempel e-postdirigering eller webbhosting.

Hjälp med att lägga till hello DNS-posten, läsa [för att lägga till en DNS-post på populära DNS-registratorer](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-hello-domain-name-with-azure-ad"></a>Kontrollera hello domännamnet med Azure AD
När du har lagt till hello DNS-post, är du redo tooverify hello domännamnet med Azure AD.

Om du fortfarande har hello **Lägg till domän** guiden öppen väljer **Kontrollera** på hello tredje sidan i guiden hello. När du väljer **Kontrollera**, Azure AD ser för hello DNS-posten i hello DNS-zonfilen för hello domän. Azure AD kan verifiera hello domännamn förrän hello DNS-poster har spridits. Spridningen tar ofta bara några sekunder, men kan ibland ta en timme eller mer. Om verifieringen inte fungerar hello första gången, försök igen senare.

Om hello **Lägg till domän** inte är öppen kan du verifiera hello domän i hello [klassiska Azure-portalen](https://manage.windowsazure.com/):

1. Logga in med ett användarkonto som är en global administratör i Azure AD-katalogen.
2. Öppna din katalog och markera hello **domäner** fliken.
3. Välj hello domännamn som du vill använda tooverify och välj **Kontrollera** i hello kommandofält.
4. Välj **Kontrollera** hello dialogrutan rutan toocomplete hello verifieringen.

Nu kan du [tilldela användarnamn som innehåller ditt domännamn](active-directory-add-domain-add-users.md).

## <a name="troubleshooting"></a>Felsökning
Om du inte kan verifiera ett eget domännamn kan du försöka hello följande. Vi börjar med hello vanlig och arbets ned toohello minst vanliga.

1. **Vänta en timma**. DNS-poster måste toopropagate innan Azure AD kan verifiera hello domän. Detta kan ta en timma eller mer.
2. **Kontrollera hello DNS-posten har angetts och att den är korrekt**. Slutför det här steget på hello webbplatsen för hello domännamnsregistratorn för hello domän. Azure AD kan inte verifiera hello domännamnet om hello DNS-posten inte finns i hello DNS-zonfilen eller om det inte är en exakt matchning med hello DNS-posten som Azure AD tillhandahåller. Om du inte har åtkomst tooupdate DNS-poster för domänen som hello hello domännamnsregistratorn dela hello DNS-post med hello person eller det team i din organisation som har nödvändig behörighet och be dem tooadd hello DNS-post.
3. **Ta bort hello domännamn från en annan katalog i Azure AD**. Ett domännamn kan bara verifieras i en enskild katalog. Om ett domännamn tidigare verifierades i en annan katalog måste du ta bort det därifrån innan du kan verifiera det i din nya katalog. toolearn om att ta bort domännamn, läsa [hantera egna domännamn](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Lägga till fler anpassade domännamn
Om din organisation använder flera anpassade domännamn, till exempel ”contoso.com” och ”contosobank.com”, du kan lägga till dem upp tooa upp till 900 domännamn. Använd hello samma steg i den här artikeln tooadd varje av ditt domännamn.

## <a name="next-steps"></a>Nästa steg
* [Tilldela användarnamn som innehåller ditt domännamn](active-directory-add-domain-add-users.md)
* [Hantera egna domännamn](active-directory-add-manage-domain-names.md)
* [Lär dig mer om domänhanteringsbegrepp i Azure AD](active-directory-add-domain-concepts.md)
* [Visa en företagsanpassad sida när användarna loggar in](active-directory-add-company-branding.md)
* [Använda PowerShell toomanage domännamn i Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

