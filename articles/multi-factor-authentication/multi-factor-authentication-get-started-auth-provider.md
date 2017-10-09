---
title: "aaaGet igång Azure leverantör av Multifaktorautent | Microsoft Docs"
description: "Lär dig hur toocreate ett Azure leverantör av Multifaktorautent."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 00ea967a80b43baff38c1de586c54d95c9abac2c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Komma igång med en Azure Multi-Factor Authentication-provider
Tvåstegsverifiering är tillgängligt som standard för globala administratörer med Azure Active Directory och Office 365-användare. Men om du inte vill tootake nytta av [avancerade funktioner](multi-factor-authentication-whats-next.md) sedan bör du köper hello fullständiga versionen av Azure Multi-Factor Authentication (MFA).

Ett Azure leverantör av Multifaktorautent används tootake nytta av funktioner som tillhandahålls av hello fullständiga versionen av Azure MFA. Den är avsedd för användare som **inte har licenser via Azure MFA, Azure AD Premium eller Enterprise Mobility + Security (EMS)**.  Azure MFA, Azure AD Premium och EMS innehåller hello fullständiga versionen av Azure MFA som standard. Om du har licenser behöver du inte någon Azure Multi-Factor Authentication-provider.

En Azure Multi-Factor Authentication-leverantör är obligatoriska toodownload hello SDK.

> [!IMPORTANT]
> toodownload hello SDK måste du toocreate ett Azure leverantör av Multifaktorautent även om du har licenser för Azure MFA eller AAD Premium EMS.  Om du skapar ett Azure leverantör av Multifaktorautent för detta ändamål och redan har licenser kan vara att toocreate hello providern med hello **Per aktiverad användare** modell. Länka hello providern toohello katalog som innehåller hello Azure MFA, Azure AD Premium eller EMS-licenser. Den här konfigurationen garanterar att du endast debiteras om du har flera unika användare som utför tvåstegsverifiering än hello antalet licenser du äger.

## <a name="what-is-an-azure-multi-factor-auth-provider"></a>Vad är en Azure Multi-Factor Auth-provider?

Om du inte har licenser för Azure Multi-Factor Authentication kan skapa du en tvåstegsverifiering för auth providern toorequire för dina användare. Om du utvecklar en anpassad app och vill tooenable Azure MFA, skapar du en leverantör av multifaktorautent och [hämta hello SDK](multi-factor-authentication-sdk.md).

Det finns två typer av autentiseringsleverantörer och hello skillnaden är runt hur din Azure-prenumeration debiteras. alternativ för hello per autentisering beräknar hello antalet autentiseringar utföras mot din klient under en månad. Det här alternativet är bäst om du har ett antal användare som bara autentiserar sig ibland, till exempel om du kräver MFA för en anpassad app. hello per användare alternativet beräknar hello antalet personer i din klient som utför tvåstegsverifiering i en månad. Det här alternativet är bäst om du har några användare med licenser men behöver tooextend MFA toomore användare utanför din antalet programvarulicenser.

## <a name="create-a-multi-factor-auth-provider"></a>Skapa en Multi-Factor Authentication-provider
Använd hello följande steg toocreate ett Azure leverantör av Multifaktorautent. Azure Flerfunktionsautentiseringsleverantörer kan bara skapas i hello klassiska Azure-portalen. Om du inte kan logga in toohello klassiska Azure-portalen, kontrollera att Azure AD-klienten är toomake [som är associerade med en Azure-prenumeration](../active-directory/active-directory-how-subscriptions-associated-directory.md). 

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) som administratör.
2. Hello vänster markerar **Active Directory**.
3. Välj på hello Active Directory-sidan hello överst **Multi-Factor Authentication-Providers**.
   
   ![Skapa en MFA-provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)

4. Hello längst ned i klickar du på **ny**.
   
   ![Skapa en MFA-provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)

5. Under Apptjänster väljer du **Multi-Factor Auth-provider**
   
   ![Skapa en MFA-provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)

6. Välj **Snabbregistrering**.
   
   ![Skapa en MFA-provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)

7. Fyll i följande fält hello och välj **skapa**.
   1. **Namnet** – hello namn på hello leverantör av Multifaktorautent.
   2. **Användningsmodell** – Välj ett av två alternativ:
      * Per autentisering – köpmodell som debiterar per autentisering. Används vanligtvis för scenarier som använder Azure Multi-Factor Authentication i ett program som riktar sig till konsumenter.
      * Per aktiverad användare – köpmodell som debiterar per aktiverad användare. Används vanligtvis för medarbetare åtkomst tooapplications som Office 365. Välj det här alternativet om du har några användare som redan har licens för Azure MFA.
   3. **Directory** – hello Azure Active Directory-klient som hello Flerfunktionsautentiseringsleverantören associeras med. Tänk på följande hello:
      * Du behöver inte en Azure AD directory toocreate en leverantör av Multifaktorautent. Lämnar du rutan tom om du endast planerar toodownload hello Azure Multi-Factor Authentication-servern eller SDK.
      * hello leverantör av Multifaktorautent måste vara kopplad till en Azure AD directory tootake utnyttja hello avancerade funktioner.
      * Det går bara att associera en Multi-Factor Authentication-provider med en Azure AD-katalog.  
      ![Skapa en MFA-provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)

8. När du klickar på Skapa, hello Flerfunktionsautentiseringsleverantören har skapats och du bör se meddelandet: **har skapats Flerfunktionsautentiseringsleverantören**. Klicka på **OK**.  
   
   ![Skapa en MFA-provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)  

## <a name="manage-your-multi-factor-auth-provider"></a>Hantera Multi-Factor Authentication-providern

Du kan inte ändra hello användning modellen (per aktiverad användare eller per autentisering) när en MFA-provider har skapats. Du kan dock ta bort hello MFA-leverantör och sedan skapa en med en annan användningsmodell.

Om hello aktuella leverantör av Multifaktorautent är associerad med en Azure AD-katalog (kallas även en Azure AD-klient) kan du på ett säkert sätt bort hello MFA-leverantör och skapar en som är länkade toohello samma Azure AD-klient. Alternativt, om du har tillräckligt med MFA, Azure AD Premium eller Enterprise Mobility + Security (EMS) licenser toocover alla användare som har aktiverats för MFA måste du kan ta bort hello MFA-leverantören helt och hållet.

Om MFA-leverantören är inte länkade tooan Azure AD-klient eller du länka hello nya MFA-provider tooa olika Azure AD-klient, överförs inte användarinställningar och konfigurationsalternativ. Dessutom måste befintliga Azure MFA-servrar toobe återaktivera med lösenordsaktiveringsuppgifter som skapades genom hello nya MFA-leverantören. Återaktivera hello MFA servrar toolink dem toohello nya MFA-providern påverkar inte telefonsamtal och SMS-autentisering, men mobilappen meddelanden slutar fungera för alla användare tills de återaktivera hello mobila appar.

## <a name="next-steps"></a>Nästa steg

[Hämta hello Multi-Factor Authentication SDK](multi-factor-authentication-sdk.md)

[Konfigurera inställningar för Multi-Factor Authentication](multi-factor-authentication-whats-next.md)
