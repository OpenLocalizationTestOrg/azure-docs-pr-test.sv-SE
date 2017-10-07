---
title: aaaHow tooget en Azure AD-klient | Microsoft Docs
description: "Hur tooget ett Azure Active Directory-klient för registrering och utveckling av program."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: dcc6b3109528cf763bda9bd527344ea9ab5c0d69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-an-azure-active-directory-tenant"></a>Hur tooget ett Azure Active Directory-klient
I Azure Active Directory (Azure AD) representerar en [klient](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) en organisation.  Det är en dedikerad instans av hello Azure AD-tjänsten som en organisation tilldelas och äger när den registrerar sig för en Microsoft-molntjänst som Azure, Microsoft Intune eller Office 365.  Varje Azure AD-klient är separat och åtskild från andra Azure AD-klienter.  

En klient inrymmer hello användare i en företags- och hello informationen om dem – deras lösenord, användarprofildata, behörigheter och så vidare.  Den innehåller också grupper, program och annan information som rör tooan organisation och dess säkerhet.

tooallow Azure AD-användare toosign i tooyour program, måste du registrera ditt program i en egen klient.  Det är **helt kostnadsfritt** att publicera ett program i en Azure AD-klient.  De flesta utvecklare skapar i själva verket flera klienter och program för experimentering, utveckling, mellanlagring och testning.  Organisationer som registrera sig för och använder ditt program kan välja toopurchase licenser om de vill tootake utnyttja avancerade katalogfunktioner.

Så, hur skaffar du en Azure AD-klient?  hello process kan vara en lite olika om du:

* [du har en befintlig Office 365-prenumeration](#use-an-existing-office-365-subscription)
* [du har en befintlig Azure-prenumeration som är associerad med ett Microsoft-konto](#use-an-msa-azure-subscription)
* [du har en befintlig Azure-prenumeration som är associerad med ett organisationskonto](#use-an-organizational-azure-subscription)
* [Inte har något av ovanstående hello & vill toostart från grunden](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Använda en befintlig Office 365-prenumeration
Om du har en befintlig prenumeration på Office 365 kan har du redan en Azure AD-klientorganisation! Du kan logga in toohello [Azure-portalen](https://portal.azure.com) med ditt O365-konto och börja använda Azure AD.

## <a name="use-an-msa-azure-subscription"></a>Använda en MSA Azure-prenumeration
Om du redan har registrerat dig för en Azure-prenumeration med ditt individuella Microsoft-konto har du redan en klient!  När du loggar in i toohello [Azure Portal](https://portal.azure.com), du loggas automatiskt i tooyour standard klient. Du är ledigt toouse finns i den här innehavaren som du anpassa - men du kanske vill toocreate ett administratörskonto för din organisation.

toodo så, Följ dessa steg.  Du kan också vill toocreate en ny klient och skapa en administratör i den klienten med hjälp av en liknande process.

1. Logga in på hello [Azure Portal](https://portal.azure.com) med ditt individuella konto
2. Navigera toohello ”Azure Active Directory” avsnittet av hello-portal (finns i hello vänstra navigeringsfältet under **fler tjänster**)
3. Du automatiskt att loggas i toohello ”standardkatalog”, om inte du kan växla kataloger genom att klicka på namnet på ditt konto i hello övre högra hörnet.
4. Från hello **Snabbuppgifter** väljer **lägga till en användare**.
5. I Hej formuläret Lägg till användare, ange hello följande information:

   * Namn: (välj ett lämpligt värde)
   * Användarnamn: (välj ett användarnamn för den här administratören)
   * Profil: (Fyll i hello lämpliga värden för förnamn, senaste namn, befattning och avdelning)
   * Roll: Global administratör
6. När du har slutfört hello formuläret Lägg till användare, och ta emot hello tillfälliga lösenord för hello ny administrativ användare kan vara säker på att toorecord lösenordet eftersom du behöver toologin med den nya användaren i ordning toochange hello lösenord. Du kan också skicka hello lösenordet direkt toohello användare med en alternativ e-postadress.
7. Klicka på **skapa** toocreate hello ny användare.
8. toochange hello tillfälliga lösenord, logga in på [https://login.microsoftonline.com](https://login.microsoftonline.com) med den nya användaren konto och ändra hello lösenordet när.

## <a name="use-an-organizational-azure-subscription"></a>Använda en Azure-prenumeration för en organisation
Om du redan har registrerat dig för en Azure-prenumeration med ditt organisationskonto har du redan en klient.  I hello [Azure Portal](https://portal.azure.com), bör du hitta en klient när du navigerar för ”fler tjänster” och ”Azure Active Directory”.  Du är ledigt toouse som den här klienten som du ser får plats.

## <a name="start-from-scratch"></a>Börja från början
Om alla hello ovan är meningslös text tooyou oroa dig inte.  Gå bara till [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) toosign för Azure med en ny organisation.  När du har slutfört hello process, måste din egen Azure AD-klient med hello domännamn som du valde när du registrerade dig.  I hello [Azure Portal](https://portal.azure.com), du kan hitta din klient genom att navigera för ”Azure Active Directory” i hello vänstra navigeringsfältet.

Du kommer att nödvändiga tooprovide kreditkortsinformation inom ramen för hello registrerar dig för Azure.  Oroa dig inte: du debiteras inte när du publicerar program i Azure AD eller skapar nya klienter.
