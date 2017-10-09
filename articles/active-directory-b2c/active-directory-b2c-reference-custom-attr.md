---
title: 'Azure Active Directory B2C: Anpassade attribut som | Microsoft Docs'
description: "Hur toouse anpassade attribut i Azure Active Directory B2C toocollect information om dina användare"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 055ffb0a-197b-4716-8dad-1fd8a01e174f
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: fb1bff77ad54c246c6d2f69f39c03eafb76fe6bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-toocollect-information-about-your-consumers"></a>Azure Active Directory B2C: Använd anpassade attribut toocollect information om dina användare för
Din Azure Active Directory (AD Azure) B2C-katalog som levereras med en inbyggd uppsättning information (attribut): Förnamn, efternamn, ort, postnummer och andra attribut. Men har alla konsumentinriktade program unika krav på vilka attribut toogather från konsumenterna. Du kan utöka hello uppsättning attribut som lagras på varje konsumentkonto med Azure AD B2C. Du kan skapa anpassade attribut på hello [Azure-portalen](https://portal.azure.com/) och använda den i din anmälan principer som visas nedan. Du kan också läsa och skriva dessa attribut med hjälp av hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [!NOTE]
> Använd för anpassade attribut [Azure AD Graph API Directory-schemautökningar](https://msdn.microsoft.com/library/azure/dn720459.aspx).
> 
> 

## <a name="create-a-custom-attribute"></a>Skapa ett anpassat attribut
1. [Följ dessa steg toonavigate toohello B2C-funktionsbladet på hello Azure-portalen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicka på **användarattribut**.
3. Klicka på **+ Lägg till** hello överst i hello-bladet.
4. Ange en **namn** för hello anpassat attribut (till exempel ”ShoeSize”) och eventuellt en **beskrivning**. Klicka på **Skapa**.
   
   > [!NOTE]
   > Endast hello ”sträng” **datatyp** är tillgänglig.
   > 
   > 

hello-attributet är nu tillgänglig hello listan över **användarattribut**, och för användning i dina principer för registrering.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Använd ett anpassat attribut i principen för registrering
1. [Följ dessa steg toonavigate toohello B2C-funktionsbladet på hello Azure-portalen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicka på **Registreringsprinciper**.
3. Klicka på principen för registrering (till exempel ”B2C_1_SiUp”)-tooopen den. Klicka på **redigera** hello överst i hello-bladet.
4. Klicka på **registreringsattribut** och välj hello anpassat attribut (till exempel ”ShoeSize”). Klicka på **OK**.
5. Klicka på **Programanspråk** och välj hello anpassade attribut. Klicka på **OK**.
6. Klicka på **spara** hello överst i hello-bladet.

Du kan använda funktionen för hello ”kör nu” på hello princip tooverify hello användarfunktioner. Du bör nu se ”ShoeSize” i hello lista över attribut som samlas in under registreringen konsumenten och finns i hello token skickade tillbaka tooyour program.

## <a name="notes"></a>Anteckningar
* Tillsammans med principer för registrering kan anpassade attribut också användas i principer för registrering eller inloggning och redigera principer profiler.
* Det finns en känd begränsning för anpassade attribut. Det är bara skapade hello första gången den används i princip och inte när du lägger till den toohello lista över **användarattribut**.

