---
title: "Azure Active Directory B2C: Självbetjäning för lösenordsåterställning | Microsoft Docs"
description: "Ett avsnitt som visar hur tooset självbetjäning lösenord återställa för dina användare i Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: ff87eea42259b610702da73af35d0a38eb7dd33d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Konfigurera Självbetjäning för återställning av lösenord för dina användare
Med hello Självbetjäning för återställning av lösenord funktion, dina användare (som har registrerat sig för lokala konton) kan återställa sina lösenord på egen hand. Detta minskar avsevärt hello belastningen på supportpersonal, särskilt om programmet har miljontals konsumenter med hjälp av den regelbundet. För närvarande stöder vi bara med en verifierad e-postadress som en återställningsmetod för. Vi lägger till ytterligare återställningsmetoder (verifierade telefonnummer, säkerhetsfrågor osv.) hello framtiden.

> [!NOTE]
> Den här artikeln gäller tooself service lösenord återställning som används i hello kontexten för en princip för inloggning. Om du behöver helt anpassningsbar Återställ lösenordsprinciper anropas från din app Se [i den här artikeln](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Som standard i katalogen inte självbetjäning lösenord återställning aktiverat. Använd hello följande steg tooturn på:

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) som Hej administratör för prenumeration. Detta är hello samma arbets- eller skolkonto eller hello samma Microsoft-konto som du använde toocreate din katalog.
2. Navigera toohello Active Directory-tillägget hello navigeringsfältet hello vänster.
3. Hitta din katalog under hello **Directory** och klicka på den.
4. Klicka på hello **konfigurera** fliken.
5. Bläddra nedåt toohello **princip för lösenordsåterställning för användare** avsnittet och växla hello **användare som har aktiverats för lösenordsåterställning** alternativet för**Ja**. Observera att hello **alternativ e-postadress** alternativet är markerat; lämna den som den är.
   
    ![Återställning av lösenord för självbetjäning](./media/active-directory-b2c-reference-sspr/sspr.png)
6. Klicka på **spara** på hello hello sidans nederkant. Du är klar!

tootest Använd hello ”kör nu” funktioner på Logga in princip som har lokala konton som en identitetsleverantör. På hello lokalt konto logga in sidan (där du anger en e-postadress och lösenord, eller ett användarnamn och lösenord), klicka på **kan inte komma åt ditt konto?** tooverify hello användarfunktioner.

> [!NOTE]
> hello självbetjäning lösenord Återställ sidor kan anpassas med hjälp av hello [funktionen för företagsanpassning](../active-directory/active-directory-add-company-branding.md).
> 
> 

