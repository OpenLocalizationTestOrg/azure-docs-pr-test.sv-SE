---
title: "aaaUsing en grupp toomanage åtkomst tooSaaS program | Microsoft Docs"
description: "Hur toouse grupper i Azure Active Directory Premium eller Basic tooassign åt tooSaaS program som är integrerade med Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro;oldportal
ms.openlocfilehash: f45ea4472b3d88e8ea514af3db31b4cc9ea68d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-group-toomanage-access-toosaas-applications"></a>Med hjälp av en grupp toomanage åtkomst tooSaaS program
Du kan använda grupper tooassign åtkomst tooa SaaS-program som är integrerad med Azure AD med hjälp av Azure Active Directory (AD Azure) med en Azure AD Premium eller Azure AD Basic-licens. Till exempel om du vill tooassign för hello marknadsföring avdelning toouse fem olika SaaS-program, kan du skapa en grupp som innehåller hello användare i marknadsföringsavdelningen hello och tilldela sedan de grupp toothese fem SaaS-program som är behövs av hello marknadsföringsavdelningen. Det här sättet du kan spara tid genom att hantera hello medlemskap hello marknadsföringsavdelningen på ett ställe. Sedan tilldelas användare toohello program när de läggs till som medlemmar i gruppen för hello marknadsföring och har deras tilldelningar tas bort från hello program när de tas bort från hello marknadsföringsgruppen.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. 

Den här funktionen kan användas med hundratals program som du kan lägga till från inom hello Azure AD Application Gallery.

**tooassign åtkomst för en grupp tooa SaaS-program**

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory** hello navigeringsfältet hello vänster.
2. Välj hello **Directory** fliken och sedan öppna hello katalog där du vill att tooassign åtkomst för en grupp tooa SaaS-program.
3. Välj hello **program** fliken. Välj ett program som du har lagt till från hello Application Gallery och klicka sedan på hello **användare och grupper** fliken.
4. På hello **användare och grupper** på fliken hello **från och med** ange hello namn hello grupp toowhich som du vill tooassign åtkomst och sedan väljer hello hello övre högra är markerat. Du behöver bara tootype hello första delen av gruppens namn.
5. Välj hello grupp och välj hello **tilldela åtkomst** knappen. Välj **Ja** när du ser hello bekräftelsemeddelande. Kapslade gruppmedlemskap stöds inte för gruppbaserad tilldelning tooapplications just nu.
6. Du kan också se vilka användare som är tilldelade toohello program, antingen direkt eller genom medlemskap i en grupp. toodo detta, ändra hello **visa listrutan från ”grupper”** för**”alla användare”**. hello listan visar användare i hello katalog och huruvida varje användare tilldelas toohello program. hello listan visas också om hello tilldelade användare har tilldelats toohello program direkt (Tilldelningstyp visas som ”Direct”) eller genom gruppmedlemskap (Tilldelningstyp visas som ärvs.)

> [!NOTE]
> Du kan se hello användare och grupper bara när du har aktiverat Azure AD Premium eller Azure AD Basic.
>
>

### <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Vad är Azure Active Directory?](active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
