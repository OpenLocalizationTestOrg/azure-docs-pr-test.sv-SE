---
title: "aaaAdd ditt eget domännamn och konfigurera federerad inloggning tooAzure Active Directory | Microsoft Docs"
description: "Hur tooadd ditt företags domännamn tooAzure Active Directory tooset in federerad inloggning mellan Azure Active Directory och lokal federation-lösning"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 27126c7e-e6d6-4ef3-a4fb-f5f0706e749d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: 77f8cc87c27871ac96581360762aaa8d24c0eb8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-your-custom-domain-name-tooazure-active-directory"></a>Lägg till din anpassade domän namn tooAzure Active Directory
Du kan konfigurera ett anpassat domännamn, t.ex contoso.com, så att användare i contoso.com kan använda federerad enkel inloggning från företagsnätverket. Om du redan har Active Directory Federation Services (AD FS) eller en annan federationsserver som körs på företagets nätverk, kan du konfigurera Azure AD toouse ditt domännamn med hello Azure AD Connect-verktyget. Du kan också använda Azure AD Connect toodeploy en ny AD FS-miljön och konfigurerar som för federerad enkel inloggning tooAzure AD.

Följ dessa instruktioner om du inte har och inte planerar toodeploy AD FS eller en annan federationsserver: [lägga till en anpassad domän namn tooAzure Active Directory](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-tooyour-directory"></a>Lägga till en anpassad domän namn tooyour katalog
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) med ett användarkonto som är en global administratör i Azure AD-katalogen.
2. I **Active Directory**, öppna katalogen och välj hello **domäner** fliken.
3. Markera på hello kommandofältet **Lägg till**, och ange hello namnet på din domän, till exempel ”contoso.com”. Vara säker på att tooinclude hello .com, .net eller andra tillägg för toppnivån.
4. Välj hello **jag planerar tooconfigure den här domänen för enkel inloggning med min lokala Active Directory** kryssrutan.
5. Välj **Lägg till**.

Kör hello Azure AD Connect-verktyget tooget hello DNS-posten använder som Azure AD tooverify hello domän. Du ser hello DNS-posten i hello **Azure AD Domain** steg i hello guiden. Du kan se vilka steget i hello guiden ser ut som [i de här anvisningarna](connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Om du inte har hello Azure AD Connect-verktyget kan du [ladda ned den här](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Lägg till hello DNS-posten i hello domännamnsregistratorn för hello domän
hello nästa steg toouse ditt anpassade domännamn med Azure AD är tooupdate hello DNS-zonfilen för hello domän. Detta gör att Azure AD tooverify att din organisation äger hello domännamn.

1. Logga in toohello webbplatsen för domännamnsregistratorn för domännamnet. Om du inte har åtkomst till toodo, be hello person eller det team i din organisation som har den här åtkomst toocomplete steg 2 och toolet som du vet när den har slutförts.
2. Uppdatera hello DNS-zonfilen för domänen hello genom att lägga till tooyou för hello DNS-posten som tillhandahålls av Azure AD. Den här DNS-posten gör Azure AD tooverify ditt ägarskap för hello domän. hello DNS-posten ändrar inga beteenden, till exempel e-postdirigering eller webbhosting.

Om du behöver hjälp med det här steget läser du [Instruktioner för hur du lägger till en DNS-post hos populära DNS-registratorer](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-hello-domain-name-with-azure-ad"></a>Kontrollera hello domännamnet med Azure AD
När du har lagt till hello DNS-post, är du redo tooverify hello domännamnet med Azure AD.

tooverify hello domän, väljer **nästa** på hello **Azure AD Domain** steg i hello Azure AD Connect-guiden. Azure AD ser för hello DNS-posten i hello DNS-zonfilen för hello domän. Azure AD Kontrollera endast hello domännamnet när hello DNS-poster har spridits. Distributionen tar ofta bara några sekunder, men kan ibland ta en timme eller mer. Om verifieringen inte fungerar hello första gången, försök igen senare.

Gå sedan vidare med hello återstående stegen i hello Azure AD Connect-guiden. Detta synkroniserar användare från din Windows Server AD-tooAzure AD. Synkroniserade användare i hello-domän som du har konfigurerat för federation ska kunna tooget en federerad enkel inloggning upplevelse som påminner om ditt företagsnätverk tooAzure AD.

## <a name="troubleshooting"></a>Felsökning
Om du inte kan verifiera ett eget domännamn kan du försöka hello följande. Vi börjar med hello vanlig och arbets ned toohello minst vanliga.

1. **Vänta en timma**. DNS-poster måste toopropagate innan Azure AD kan verifiera hello domän. Detta kan ta en timma eller mer.
2. **Kontrollera hello DNS-posten har angetts och att den är korrekt**. Slutför det här steget på hello webbplatsen för hello domännamnsregistratorn för hello domän. Azure AD kan inte verifiera hello domännamnet om hello DNS-posten inte finns i hello DNS-zonfilen eller om det inte är en exakt matchning med hello DNS-posten som Azure AD tillhandahåller. Om du inte har åtkomst tooupdate DNS-poster för domänen som hello hello domännamnsregistratorn dela hello DNS-post med hello person eller det team i din organisation som har nödvändig behörighet och be dem tooadd hello DNS-post.
3. **Ta bort hello domännamn från en annan katalog i Azure AD**. Ett domännamn kan bara verifieras i en enskild katalog. Om ett domännamn tidigare verifierades i en annan katalog måste du ta bort det därifrån innan du kan verifiera det i din nya katalog. toolearn om att ta bort domännamn, läsa [hantera egna domännamn](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Lägga till fler anpassade domännamn
Om din organisation använder flera anpassade domännamn, till exempel ”contoso.com” och ”contosobank.com”, du kan lägga till dem upp tooa upp till 900 domännamn. Använd hello samma steg i den här artikeln tooadd varje av ditt domännamn.

## <a name="next-steps"></a>Nästa steg
* [Hantera egna domännamn](active-directory-add-manage-domain-names.md)
* [Lär dig mer om domänhanteringsbegrepp i Azure AD](active-directory-add-domain-concepts.md)
* [Visa en företagsanpassad sida när användarna loggar in](active-directory-add-company-branding.md)
* [Använda PowerShell toomanage domännamn i Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

