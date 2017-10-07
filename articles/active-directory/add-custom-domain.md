---
title: "aaaAdd en anpassad domän tooAzure AD | Microsoft Docs"
description: "Förklarar hur tooadd en anpassad domän i Azure Active Directory."
services: active-directory
author: jeffgilb
manager: femila
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 878cecad364ec47f1c6755d742aaccbce627dc5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-a-custom-domain-name-tooazure-active-directory"></a>Snabbstart: Lägga till en anpassad domän namn tooAzure Active Directory

Varje Azure AD-katalog som medföljer ett första domännamn i hello form av *domainname*. onmicrosoft.com. hello första domännamn kan inte ändras eller tas bort, men du kan lägga till din företagsdomän namn tooAzure AD samt. Din organisation har till exempel förmodligen andra domänens namn används toodo företag och användare som loggar in med företagets domännamn. Om du lägger till anpassade domäner namn tooAzure AD kan tooassign användarnamn i hello katalog som är bekant tooyour användare, till exempel ”alice@contoso.com”. i stället för ”alice @*<domain name>*. onmicrosoft.com'. hello-processen är enkel:

1. Lägg till hello domänen namnet tooyour katalog
2. Lägg till en DNS-post för hello domännamn i hello domännamnsregistratorn
3. Kontrollera hello domännamn i Azure AD

## <a name="add-your-custom-domain"></a>Lägg till den anpassa domänen
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **Azure Active Directory** i hello textruta och välj sedan **RETUR**.
   
   ![Öppna användarhantering](./media/active-directory-domains-add-azure-portal/user-management.png)
3. På hello ***katalognamn*** bladet väljer **domännamn**.
4. På hello  ***katalognamn* -domännamn** bladet, Välj hello **Lägg till** kommando.
   
   ![Att välja hello tilläggskommando](./media/active-directory-domains-add-azure-portal/add-command.png)
5. På hello **domännamn** bladet anger hello namnet på domänen, till exempel ”contoso.com” i rutan hello och välj sedan **Lägg till domän**. Vara säker på att tooinclude hello .com, .net eller andra tillägg för toppnivån.
6. På hello ***domännamn*** bladet (med ett eget domännamn i hello avdelning), hämta hello DNS-posten information toouse tooverify att din organisation äger hello domännamn.
   
   ![Hämta information om DNS-post](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

> [!TIP]
> Om du planerar toofederate lokala Windows Server AD med Azure AD, måste du tooselect hello **jag planerar tooconfigure den här domänen för enkel inloggning med min lokala Active Directory** kryssrutan när du kör hello Azure AD Connect-verktyget toosynchronize dina kataloger. Du måste också tooregister hello samma domännamn som du väljer för federering med din lokala katalog i hello **Azure AD Domain** steg i hello guiden. Du kan se vilka steget i hello guiden ser ut som [i de här anvisningarna](./connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Om du inte har hello Azure AD Connect-verktyget kan du [ladda ned den här](http://go.microsoft.com/fwlink/?LinkId=615771).

Nu när du har lagt till domännamnet för hello Kontrollera Azure AD att din organisation äger hello domännamn. Innan Azure AD kan utföra denna verifiering måste du lägga till en DNS-post i hello DNS-zonfilen för hello domännamn. Den här uppgiften utförs på hello webbplatsen för domännamnsregistratorn för domännamnet hello.

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Lägg till hello DNS-posten i hello domännamnsregistratorn för hello domän
hello nästa steg toouse ditt anpassade domännamn med Azure AD är tooupdate hello DNS-zonfilen för hello domän. Azure AD kan kontrollera att din organisation äger hello domännamn.

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
4. På hello ***domännamn*** bladet (det vill säga hello bladet som öppnas med ett nytt domännamn i hello rubrik) Välj **Kontrollera** toocomplete hello verifiering.

> [!TIP]
> Du kan lägga till upp too900 anpassade domännamn, men endast en kan vara [som hello primära domännamn för din Azure AD-katalog](active-directory-domains-manage-azure-portal.md#set-the-primary-domain-name-for-your-azure-ad-directory) används som standard när du skapar nya konton.

Nu kan du skapa molnbaserade användarkonton eller uppdateringen tidigare synkroniserade lokalt information om användarkonton med hjälp av ditt domännamn. Du kan också ändra tidigare synkroniserade användarkonto domän suffix information med [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) eller hello [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="troubleshooting"></a>Felsökning
Om du inte kan verifiera ett eget domännamn, prova följande felsökningssteg hello:

1. **Vänta en timma**. DNS-poster måste toopropagate innan Azure AD kan verifiera hello domän. Detta kan ta en timma eller mer.
2. **Kontrollera hello DNS-posten har angetts och att den är korrekt**. Slutför det här steget på hello webbplatsen för hello domännamnsregistratorn för hello domän. Azure AD kan inte verifiera hello domännamnet om hello DNS-posten inte finns i hello DNS-zonfilen eller om det inte är en exakt matchning med hello DNS-posten som Azure AD tillhandahåller. Om du inte har åtkomst tooupdate DNS-poster för domänen som hello hello domännamnsregistratorn dela hello DNS-post med hello person eller det team i din organisation som har nödvändig behörighet och be dem tooadd hello DNS-post.
3. **Ta bort hello domännamn från en annan katalog i Azure AD**. Ett domännamn kan bara verifieras i en enskild katalog. Om ett domännamn tidigare verifierades i en annan katalog måste du ta bort det därifrån innan du kan verifiera det i din nya katalog. toolearn om att ta bort domännamn, läsa [hantera egna domännamn](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Lägga till fler anpassade domännamn
Om din organisation använder mer än ett domännamn, till exempel ”contoso.com” och ”contosobank.com”, kan du lägga upp too900 mer genom att upprepa hello stegen i den här artikeln för varje.

### <a name="learn-more"></a>Läs mer
[Översikt över anpassade domännamn i Azure AD](active-directory-add-domain-concepts.md)

[Hantera egna domännamn](active-directory-domains-manage-azure-portal.md)


## <a name="next-steps"></a>Nästa steg
I den här snabbstarten du har lärt dig hur tooadd en anpassad domän tooAzure AD. 

Du kan använda hello följande länk tooadd nya anpassade domäner i Azure AD från hello Azure-portalen.

> [!div class="nextstepaction"]
> [Lägga till en anpassad domän](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/QuickStart) 