---
title: "aaaAdd nya användare tooAzure Active Directory | Microsoft Docs"
description: "Förklarar hur tooadd nya användare eller ändrar användarinformation i Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 72f67ad41022fd19fd94c8e1301943b0db1e57bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-or-users-with-microsoft-accounts-tooazure-active-directory"></a>Lägga till nya användare eller användare med Microsoft-konton tooAzure Active Directory
Lägg till användare toopopulate din katalog. Den här artikeln förklarar hur tooadd nya användare i din organisation och hur tooadd användare som har Microsoft-konton. Mer information om hur du lägger till användare från andra kataloger i Azure Active Directory eller hur du lägger till användare från partnerföretag finns i [Lägga till användare från andra kataloger eller partnerföretag i Azure Active Directory](active-directory-create-users-external.md). Tillagda användare har inte administratörsbehörighet som standard, men du kan tilldela roller toothem när som helst.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. För hur tooadd en användare i administrationscentret för hello Azure AD, se [lägga till nya användare tooAzure Active Directory](active-directory-users-create-azure-portal.md).

## <a name="add-a-user"></a>Lägga till en användare
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **Active Directory**, och välj sedan hello namnet på din organisationskatalog.
3. Välj hello **användare** fliken och markera i kommandofältet hello **Lägg till användare**.
4. På hello **berätta om den här användaren** sidan under **typ av användare**, väljer du antingen:

   * **Ny användare i din organisation** – Lägger till ett nytt konto till din katalog.
   * **Användare med ett befintligt microsoftkonto** – lägger till en befintlig Microsoft konsumenten konto tooyour katalog (till exempel ett Outlook-konto)
5. Beroende på **typen av användare** anger du ett användarnamn (för en ny användare) eller en e-postadress (för en användare med ett Microsoft-konto).
6. Hello användaren **profil** anger du ett första och sista namn, ett användarvänligt namn och en användarroll från hello **roller** lista. Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md). Ange om för**aktivera Multifaktorautentisering** för hello användare.
7. På hello **skaffa tillfälligt lösenord** väljer **skapa**.

> [!IMPORTANT]
> Om din organisation använder mer än en domän, bör du känna till om hello följande problem när du lägger till ett användarkonto:
>
> * tooadd användarkonton med hello samma användarens huvudnamn (UPN) över domäner, **första** lägga till, till exempel geoffgrisso@contoso.onmicrosoft.com, **följt av** geoffgrisso@contoso.com.
> * Lägg **inte** till geoffgrisso@contoso.com innan du lägger till geoffgrisso@contoso.onmicrosoft.com. Den här ordningen är viktig och kan vara komplicerade tooundo.
>
>

## <a name="change-user-information"></a>Ändra användarinformation
Du kan ändra valfria användarattribut utom hello objekt-ID.

1. Öppna din katalog.
2. Välj hello **användare** fliken och markera sedan hello visningsnamn hello användare som du vill toochange.
3. Slutför ändringarna och klicka sedan på **Spara**.

Du kan inte ändra hello användarinformationen med den här proceduren om hello-användare som du ändrar synkroniseras med din lokala Active Directory-tjänst. toochange hello användare kan använda din lokala Active Directory-hanteringsverktyg.

## <a name="guest-user-management-and-limitations"></a>Hantering av och begränsningar för gästanvändare
Gästkonton är användare från andra kataloger som var inbjudna tooyour directory tooaccess SharePoint-dokument, program eller andra Azure-resurser. Ett gästkonto i din katalog har dess underliggande UserType-attributet för ”gäst”. Vanliga användare (särskilt medlemmar i din katalog) har hello UserType-attributet ”medlem”.

Gäster har en begränsad uppsättning rättigheter i hello directory. Dessa rättigheter begränsar hello möjligheten för gäster toodiscover information om andra användare i hello directory. Gästanvändare kan dock fortfarande interagera med hello användare och grupper som är associerade med hello resurser som de arbetar med. Gästanvändare kan:

* Visa andra användare och grupper som är associerade med en Azure-prenumeration toowhich som de tilldelats.
* Se hello medlemmar i grupper toowhich som de tillhör
* Leta upp andra användare i hello directory, om de redan känner till hello hello användarens fullständiga e-postadress
* Finns bara en begränsad uppsättning attribut för hello användare de letar upp, begränsat toodisplay namn, e-postadress, användarens huvudnamn (UPN) och miniatyrfoto.
* Hämta en lista över verifierade domäner i hello directory
* Medgivande tooapplications, ger dem hello samma åtkomst som medlemmar har i din katalog

## <a name="set-guest-user-access-policies"></a>Ange åtkomstprinciper för gästanvändare
Hej **konfigurera** för en katalog innehåller alternativ toocontrol åtkomsten för gästanvändare. Dessa alternativ kan bara ändras på den klassiska Azure-portalen av en global katalogadministratör. För närvarande finns det ingen PowerShell- eller API-metod.

tooopen hello **konfigurera** fliken i hello Azure klassiska portal, Välj **Active Directory**, och välj sedan hello namnet på hello katalog.

![Konfigurera fliken i Azure Active Directory][1]

Du kan redigera hello alternativ toocontrol åtkomsten för gästanvändare.

![Alternativ för åtkomstkontroll för gästanvändare][2]

## <a name="whats-next"></a>Nästa steg
* [Lägga till användare från andra kataloger eller partnerföretag i Azure Active Directory](active-directory-create-users-external.md)
* [Administrera Azure AD](active-directory-administer.md)
* [Hantera lösenord i Azure AD](active-directory-manage-passwords.md)
* [Hantera grupper i Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
