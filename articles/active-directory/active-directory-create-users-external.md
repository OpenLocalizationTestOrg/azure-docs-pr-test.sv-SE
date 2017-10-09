---
title: "aaaAdd användare från andra kataloger eller partnerföretag i Azure Active Directory | Microsoft Docs"
description: "Förklarar hur tooadd användare eller ändrar användarinformation i Azure Active Directory, inklusive externa användare och gästanvändare."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 564a04ec-53c1-470b-9ab9-f3db57da0a89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 92099e5792365c307b0f3d4f2dff5dd8424aeab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory"></a>Lägga till användare från andra kataloger eller partnerföretag i Azure Active Directory

Den här artikeln förklarar hur tooadd användare från andra kataloger i Azure Active Directory eller lägga till användare från partnerföretag. Information om att lägga till nya användare i din organisation och lägga till användare som har Microsoft-konton finns [lägga till nya användare tooAzure Active Directory](active-directory-create-users.md). 

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. Hur tooadd B2B-samarbete gästanvändare i hello Azure AD admin center finns [vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)

Tillagda användare har inte administratörsbehörighet som standard, men du kan tilldela roller toothem när som helst.

## <a name="add-a-user"></a>Lägga till en användare
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **Active Directory** och öppna katalogen.
3. Välj hello **användare** fliken och markera i kommandofältet hello **Lägg till användare**.
4. På hello **berätta om den här användaren** sidan under **typ av användare**, väljer du antingen:

   * **Användare i en annan Azure AD-katalog** – lägger till en användarens konto tooyour katalog som kommer från en annan Azure AD-katalog. Du kan bara välja en användare i en annan katalog om du är medlem i den katalogen.
   * **Användare i partnerföretag** -tooinvite och auktorisera partner företagets användare tooyour katalog (se [Azure Active Directory B2B-samarbete](active-directory-b2b-what-is-azure-ad-b2b.md)). Du behöver för[överför en CSV-fil med e-postadresser](active-directory-b2b-references-csv-file-format.md).
5. Hello användaren **profil** anger du ett första och sista namn, ett användarvänligt namn och en användarroll från hello **roller** lista. Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md). Ange om för**aktivera Multifaktorautentisering** för hello användare.
6. På hello **skaffa tillfälligt lösenord** väljer **skapa**.

> [!IMPORTANT]
> Om din organisation använder mer än en domän, bör du känna till om hello följande problem när du lägger till ett användarkonto:
>
> * tooadd användarkonton med hello samma användarens huvudnamn (UPN) över domäner, **första** lägga till, till exempel geoffgrisso@contoso.onmicrosoft.com, **följt av** geoffgrisso@contoso.com.
> * Lägg **inte** till geoffgrisso@contoso.com innan du lägger till geoffgrisso@contoso.onmicrosoft.com.
>

Om du ändrar informationen för en användare vars identitet synkroniseras med din lokala Active Directory-tjänst kan du inte ändra hello användarinformation i hello klassiska Azure-portalen. toochange Hej användarinformation, använda din lokala Active Directory-hanteringsverktyg.

## <a name="add-external-users"></a>Lägga till externa användare
Du kan också lägga till användare från en annan Azure AD directory toowhich som du tillhör, eller från partnerföretag genom att ladda upp en CSV-fil. tooadd en extern användare för **typ av användare**, ange **användare i en annan Microsoft Azure AD-katalog** eller **användare i partnerföretag**.

Båda typer av användare kommer från en annan katalog och läggs till som **externa användare**. Externa användare kan samarbeta med andra användare i en katalog utan några krav tooadd nya konton och autentiseringsuppgifter. Externa användare autentiseras med deras hemkatalog när de loggar in och autentiseringen fungerar med alla andra kataloger toowhich de har lagts till.

## <a name="external-user-management-and-limitations"></a>Extern användarhantering och begränsningar
När du lägger till en användare från en annan katalog tooyour katalog är som användaren en extern användare i din katalog. hello visningsnamnet och användarnamnet kopieras från hemkatalogen och används för hello extern användare i din katalog. Därefter är egenskaperna för hello externa användarkontot helt oberoende. Om egenskapsändringar görs toohello användaren i dennes hemkatalog, sprids inte dessa ändringar toohello externa användarkontot i din katalog.

hello enda kopplingen mellan hello två konton är hello användaren alltid autentiseras mot sin hemkatalog eller med sitt Microsoft-konto. Det är därför du inte ser ett alternativet tooreset hello lösenord eller aktivera multifaktorautentisering för en extern användare. Hej autentiseringsprincipen för hello hemkatalog eller Microsoft-konto är för närvarande hello enda som utvärderas när hello användaren loggar in.

> [!NOTE]
> Du kan dock inaktivera hello extern användare i hello directory som blockerar åtkomst tooyour directory.
>
>

Om en användare tas bort i sin hemkatalog eller om användaren säger upp sitt Microsoft-konto, kvar hello externa användaren fortfarande i din katalog. Emellertid hello användare i katalogen inte komma åt resurser eftersom de inte kan autentiseras med en hemkatalog eller Microsoft-konto.

### <a name="services-that-currently-support-access-by-azure-ad-external-users"></a>Tjänster som för närvarande stöder åtkomst av externa Azure AD-användare
* **Klassiska Azure-portalen**: gör att en extern användare är administratör för flera kataloger toomanage varje dessa kataloger.
* **SharePoint Online**: om extern delning är aktiverat kan en extern användare tooaccess SharePoint Online auktoriserad personal.
* **Dynamics CRM**: om hello användaren är licensierad via PowerShell kan en extern användare tooaccess behörighet resurser i Dynamics CRM.
* **Dynamics AX**: om hello användaren är licensierad via PowerShell kan en extern användare tooaccess behörighet resurser i Dynamics AX. Hej begränsningar för [externa Azure AD-användare](#known-limitations-of-azure-ad-external-users) gäller tooexternal användare i Dynamics AX samt.

## <a name="next-steps"></a>Nästa steg
* [Lägga till nya användare tooAzure Active Directory](active-directory-create-users.md)
* [Administrera Azure AD](active-directory-administer.md)
* [Hantera lösenord i Azure AD](active-directory-manage-passwords.md)
* [Hantera grupper i Azure AD](active-directory-manage-groups.md)
