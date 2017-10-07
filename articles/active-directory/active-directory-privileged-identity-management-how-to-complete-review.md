---
title: "aaaHow toocomplete en åtkomst-granskning | Microsoft Docs"
description: "När du har startat en åtkomst-granskning i Azure AD Privileged Identity Management lär du dig hur toocomplete det och visa hello resultat"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: abc2d3dd-afd5-42cf-8a17-6c11f5674c35
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: f99ddf3ebcf80b51110326064d584f33d8e1679a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocomplete-an-access-review-in-azure-ad-privileged-identity-management"></a>Hur toocomplete åtkomst finns i Azure AD Privileged Identity Management
Privilegierade rollen administratörer kan granska privilegierad åtkomst när en [säkerhetsgranskning har startats](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure AD Privileged Identity Management (PIM) skickar automatiskt ett e-postmeddelande som uppmanar användare tooreview deras åtkomst. Om en användare inte fick ett e-postmeddelande, kan du skicka dem hello instruktioner [hur tooperform en säkerhets-och granska](active-directory-privileged-identity-management-how-to-perform-security-review.md).

När hello säkerhet granska perioden är över, eller alla hello användare är klar med sin egen granska, följ hello stegen i den här artikeln toomanage hello granska och se hello resultat.

## <a name="manage-security-reviews"></a>Hantera säkerhet granskningar
1. Gå toohello [Azure-portalen](https://portal.azure.com/) och välj hello **Azure AD Privileged Identity Management** på instrumentpanelen.
2. Välj hello **åtkomst till granskningar** avsnittet hello instrumentpanelen.
3. Välj hello åtkomst granska som du vill toomanage.

På hello åtkomst granska detalj bladet finns ett antal alternativ för att hantera granskningen.

![PIM-knappar för granskning av åtkomst – skärmbild][1]

### <a name="remind"></a>Påminn
Om en åtkomst-granskning har konfigurerats så att användare hello granska själva hello **Påminn** knappen skickas ett meddelande. 

### <a name="stop"></a>Stoppa
Alla åtkomst omdömen har ett slutdatum, men du kan använda hello **stoppa** knappen toofinish den tidigt. Om användare inte har granskats vid den tidpunkten, kan de inte att kunna tooafter du stoppar hello, granska. Du kan inte starta om ett omdöme när den stoppas.

### <a name="apply"></a>Ansök
När en åtkomst-granskning har slutförts, antingen eftersom du har nått hello slutdatum eller hindrade den manuellt, hello **tillämpa** knappen implementerar hello resultatet av hello, granska. Om en användares åtkomst nekades i hello, granska är hello steg som tar bort sin rolltilldelning.  

### <a name="export"></a>Exportera
Om du vill tooapply hello resultaten av hello säkerhetsgranskning manuellt, kan du exportera hello, granska. Hej **exportera** knappen startar hämtningen av en CSV-fil. Du kan hantera hello resultat i Excel eller andra program som öppnar CSV-filer.

### <a name="delete"></a>Ta bort
Om du inte är intresserad av hello granska någon ytterligare kan du ta bort den. Hej **ta bort** knappen tar bort hello, granska från hello PIM-program.

> [!IMPORTANT]
> Du kommer inte får en varning innan borttagningen sker, så som du vill toodelete finns. 

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
