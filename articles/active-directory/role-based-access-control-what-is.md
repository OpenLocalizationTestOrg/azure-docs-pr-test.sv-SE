---
title: "aaaManage åtkomst och behörighet med RBAC - Azure RBAC | Microsoft Docs"
description: "Kom igång med åtkomsthantering med rollbaserad åtkomstkontroll i Azure i hello Azure-portalen. Använd rollbehörigheter tilldelningar tooassign i din katalog."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 1c133b2b57b49d85f0e12a318c7997478e095fb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-role-based-access-control-in-hello-azure-portal"></a>Kom igång med rollbaserad åtkomstkontroll i hello Azure-portalen
Säkerhet-inriktade företag bör fokusera på och ger anställda hello exakt behörigheter de behöver. För många behörigheter kan exponera en konto tooattackers. För få behörigheter innebär att anställda kan få arbetet gjort effektivt. Azure rollbaserad åtkomstkontroll (RBAC) kan du lösa det här problemet genom att erbjuda detaljerad åtkomsthantering för Azure.

Med RBAC kan du särskilja uppgifter i din grupp och ge endast hello mängd åtkomst toousers de behöver tooperform sitt arbete. Obegränsad istället för att ge alla behörigheter i din Azure-prenumeration eller resurser, kan du tillåta endast vissa åtgärder. Till exempel använda RBAC toolet en medarbetare hantera virtuella datorer i en prenumeration, medan en annan kan hantera SQL-databaser inom hello samma prenumeration.

## <a name="basics-of-access-management-in-azure"></a>Grunderna i åtkomsthantering i Azure
Varje Azure-prenumeration är associerad med en katalog i Azure Active Directory (AD). Användare, grupper och program från katalogen kan hantera resurser i hello Azure-prenumeration. Tilldela behörigheter med hello Azure-portalen, Azure kommandoradsverktyg och Azure Management-API: er.

Bevilja åtkomst genom att tilldela hello lämpliga RBAC rollen toousers, grupper och program för ett visst område. hello omfånget för en rolltilldelning kan vara en prenumeration, resursgrupp eller en enskild resurs. En roll som tilldelats en överordnad omfattning ger också åtkomst toohello underordnade som finns i den. En användare med åtkomst tooa resursgruppen kan exempelvis hantera alla hello-resurser som den innehåller som webbplatser, virtuella datorer och undernät.

![Relationen mellan element i Azure Active Directory - diagram](./media/role-based-access-control-what-is/rbac_aad.png)

Hej RBAC roll som du tilldelar avgör vilka resurser hello användare, grupp eller programmet kan hantera i omfattningen.

## <a name="built-in-roles"></a>Inbyggda roller
Azure RBAC har tre grundläggande roller som gäller tooall resurstyper:

* **Ägare** har fullständig åtkomst tooall resurser inklusive hello rätt toodelegate åtkomst tooothers.
* **Deltagare** kan skapa och hantera alla typer av Azure-resurser, men det går inte att bevilja åtkomst tooothers.
* **Läsaren** kan visa befintliga Azure-resurser.

hello resten av hello RBAC-roller i Azure kan hanteringen av specifika Azure-resurser. Hello virtuella deltagarrollen tillåter hello användaren toocreate och hantera virtuella datorer. Det ger dem åtkomst toohello virtuellt nätverk eller hello undernät som hello virtuella datorn ansluter till. 

[Inbyggda RBAC-roller](role-based-access-built-in-roles.md) visar hello roller som är tillgängliga i Azure. Det anger hello åtgärder och att varje inbyggda rollen ger toousers omfång. Om du tittar toodefine egna roller för ännu mer kontroll, se hur toobuild [anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Resursen hierarki- och arv
* Varje **prenumeration** tillhör tooonly en katalog i Azure. (Men varje katalog kan ha mer än en prenumeration.)
* Varje **resursgruppen** tillhör tooonly en prenumeration.
* Varje **resurs** tillhör tooonly en resursgrupp.

Åtkomst som du beviljar i överordnade omfattningar ärvs på underordnade omfattningar. Exempel:

* Du tilldelar hello Reader rollen tooan Azure AD-grupp hello prenumeration definitionsområdet. hello medlemmar i gruppen kan visa varje resursgrupp och resursen i hello prenumerationen.
* Du tilldelar hello deltagare rollen tooan program på hello resurs Gruppomfång. Den kan hantera resurser av alla typer i resursgruppen, men inte andra resursgrupper i hello prenumerationen.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC kontra klassiska prenumerationsadministratörer
Och medadministratörer för klassiska prenumerationsadministratörer har fullständig åtkomst toohello Azure-prenumeration. De kan hantera resurser med hjälp av hello [Azure-portalen](https://portal.azure.com) med Azure Resource Manager API: erna eller hello [klassiska Azure-portalen](https://manage.windowsazure.com) och Azure klassiska distributionsmodellen. I hello RBAC-modellen tilldelas klassiska administratörer hello ägarrollen hello prenumeration definitionsområdet.

Endast hello Azure-portalen och hello nya Azure Resource Manager API: erna stöd Azure RBAC. Användare och program som tilldelats RBAC-roller kan inte använda hello klassiska hanteringsportalen och hello Azure klassiska distributionsmodellen.

## <a name="authorization-for-management-vs-data-operations"></a>Tillstånd för hantering jämfört med dataåtgärder
Azure RBAC endast stöder hanteringsåtgärder för hello Azure-resurser i hello Azure-portalen och Azure Resource Manager API: er. Det går inte att tillåta alla data på åtgärder för Azure-resurser. Du kan till exempel tillåta någon toomanage Storage-konton, men inte toohello blobbar eller tabeller i ett Lagringskonto. Kan hanteras på samma sätt kan en SQL-databas, men hello inte tabeller i den.

## <a name="next-steps"></a>Nästa steg
* Kom igång med [rollbaserad åtkomstkontroll i hello Azure-portalen](role-based-access-control-configure.md).
* Se hello [inbyggda RBAC-roller](role-based-access-built-in-roles.md)
* Definiera egna [anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md)
