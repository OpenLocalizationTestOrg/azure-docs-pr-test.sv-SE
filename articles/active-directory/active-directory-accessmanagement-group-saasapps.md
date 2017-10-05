---
title: "Hantera åtkomst till SaaS-program med hjälp av en grupp | Microsoft Docs"
description: "Hur du använder grupper i Azure Active Directory Premium eller Basic du tilldelar åtkomst till SaaS-program som är integrerade med Azure Active Directory."
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
ms.openlocfilehash: d350011ee9fc5ced9ddb16993f68d3c840a645a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Använd en grupp för att hantera åtkomst till SaaS-program
Använder Azure Active Directory (AD Azure) med en Azure AD Premium eller Azure AD Basic-licens kan använda du grupper för att tilldela åtkomst till ett SaaS-program som är integrerad med Azure AD. Till exempel om du vill tilldela åtkomst för marknadsföringsavdelningen att använda fem olika SaaS-program kan du skapa en grupp som innehåller användarna i marknadsföringsavdelningen och sedan tilldela den gruppen för dessa fem SaaS-program som krävs av marknadsföringsavdelningen. Det här sättet kan du spara tid genom att hantera medlemskap i marknadsföringsavdelningen på ett ställe. Användare sedan tilldelas till programmet när de läggs till som medlemmar i gruppen marknadsföring och deras tilldelningar ta bort från programmet när de tas bort från gruppen marknadsföring.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln. 

Den här funktionen kan användas med hundratals program som du kan lägga till i Azure AD Application Gallery.

**Du tilldelar åtkomst för en grupp till ett SaaS-program**

1. I den [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory** på navigeringsfältet till vänster.
2. Välj den **Directory** och öppna katalogen där du vill ge behörighet för en grupp till ett SaaS-program.
3. Välj den **program** fliken. Välj ett program som du har lagt till från galleriet program och klicka sedan på den **användare och grupper** fliken.
4. På den **användare och grupper** fliken den **från och med** , ange namnet på gruppen som du vill bevilja åtkomst och välj sedan kryssmarkeringen i det övre högra hörnet. Du behöver bara ange den första delen av gruppens namn.
5. Markera gruppen och välj sedan den **tilldela åtkomst** knappen. Välj **Ja** när du ser i bekräftelsemeddelandet. Kapslade gruppmedlemskap stöds inte för gruppbaserad tilldelning till program just nu.
6. Du kan också se vilka användare som har tilldelats programmet, antingen direkt eller genom medlemskap i en grupp. Det gör du genom att ändra den **visa listrutan från ”grupper”** till **”alla användare”**. I listan visas användare i katalogen och huruvida varje användare tilldelas till programmet. I listan visas också om tilldelade användare som är kopplade till programmet direkt (Tilldelningstyp visas som ”Direct”) eller genom gruppmedlemskap (Tilldelningstyp visas som ärvs.)

> [!NOTE]
> Du kan se fliken användare och grupper bara när du har aktiverat Azure AD Premium eller Azure AD Basic.
>
>

### <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst till resurser med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Vad är Azure Active Directory?](active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
