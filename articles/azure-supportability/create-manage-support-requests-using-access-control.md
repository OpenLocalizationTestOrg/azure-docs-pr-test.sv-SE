---
title: "aaaAzure rollbaserad åtkomstkontroll (RBAC) toocontrol åt rättigheter toocreate och hantera supportärenden | Microsoft Docs"
description: "Azure rollbaserad åtkomstkontroll (RBAC) toocontrol åt rättigheter toocreate och hantera supportärenden"
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: c68a699ac870fa6bf371deb8ed0424848f39acf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-role-based-access-control-rbac-toocontrol-access-rights-toocreate-and-manage-support-requests"></a>Azure rollbaserad åtkomstkontroll (RBAC) toocontrol åt rättigheter toocreate och hantera supportärenden

[Rollbaserad åtkomstkontroll (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) Aktivera detaljerad åtkomsthantering för Azure.
Stöd för begäran skapas i hello Azure-portalen [portal.azure.com](https://portal.azure.com), använder Azure RBAC-modellen toodefine som kan skapa och hantera supportärenden.
Komma åt genom att tilldela hello lämpliga RBAC rollen toousers, grupper och program för ett visst område som kan vara en prenumeration, resursgrupp eller resurs.

Låt oss ta ett exempel: du kan hantera alla hello resurser under hello resursgrupp som webbplatser, virtuella datorer och undernät som en resurs gruppägare med läsbehörighet hello prenumeration definitionsområdet.
Men när du försöker toocreate en supportbegäran mot hello virtuell datorresurs, får du hello följande fel

![Prenumerationen fel](./media/create-manage-support-requests-using-access-control/subscription-error.png)

Stöd för hantering av begäran, behöver du skrivbehörighet eller i en roll som har hello stöd för åtgärden Microsoft.Support/* på hello prenumeration scope toobe kan toocreate och hantera supportärenden.

hello följande artikel förklarar hur du kan använda Azures anpassade rollbaserad åtkomstkontroll (RBAC) toocreate och hantera supportärenden i hello Azure-portalen.

## <a name="getting-started"></a>Komma igång

Med hello-exemplet ovan kan kommer du att kunna toocreate en supportbegäran för din resurs om du har tilldelat en anpassad roll RBAC på hello prenumeration av hello prenumerationsägaren.
[Anpassade RBAC-roller](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) kan skapas med hjälp av Azure PowerShell, Azure-kommandoradsgränssnittet (CLI) och hello REST API.

hello åtgärder egenskapen för en anpassad roll anger hello Azure operations toowhich hello rollen ger åtkomst.
toocreate en anpassad roll för stöd för hantering av begäran hello roll måste ha hello åtgärden Microsoft.Support/*

Här är ett exempel på en anpassad roll kan du använda toocreate och hantera stöder begäranden.
Vi har med namnet ”Support begär bidragsgivare” för den här rollen och hur vi refererar toohello anpassad roll i den här artikeln.

``` Json
{
    "Name":  "Support Request Contributor",
    "Id":  "1f2aad59-39b0-41da-b052-2fb070bd7942",
    "IsCustom":  true,
    "Description":  "Lets you create and manage support tickets.",
    "Actions":  [
                    "Microsoft.Support/*"
                ],
    "NotActions":  [
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Följ stegen i hello [den här videon](https://www.youtube.com/watch?v=-PaBaDmfwKI) toolearn hur toocreate en anpassad roll för din prenumeration.

## <a name="create-and-manage-support-requests-in-hello-azure-portal"></a>Skapa och hantera supportärenden i hello Azure-portalen

Låt oss ta ett exempel – du är hello ägaren av prenumerationen ”Visual Studio MSDN-prenumeration”.
Joe är din peer som är en resurs ägare toosome hello resursgrupper i den här prenumerationen och har skrivskyddad behörighet toohello prenumeration.
Du vill toogive åtkomst tooyour peer, Johan, hello möjlighet toocreate och hantera supportärenden för hello resurser under den här prenumerationen.

1. hello första steget är toogo toohello prenumeration under ”inställningar” visas en lista med användare. Klicka på hello användare Joe som har reader åtkomst på hello prenumeration och vi tilldela en ny anpassad roll toohim.

    ![Lägg till roll](./media/create-manage-support-requests-using-access-control/add-role.png)

2. Klicka på ”Lägg till” under hello ”användare” bladet. Välj hello anpassad roll ”Support begär bidragsgivare” hello listan över roller

    ![Lägga till stöd för deltagarrollen](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. När du har valt hello rollnamn, klicka på ”Lägg till användare” och ange hello Joe e-autentiseringsuppgifter. Klicka på ”Välj”

    ![Lägga till användare](./media/create-manage-support-requests-using-access-control/add-users.png)

4. Klicka på ”Ok” tooproceed

    ![Lägg till åtkomst](./media/create-manage-support-requests-using-access-control/add-access.png)

5. Nu ska du se hello användare med hello nytillagda anpassad roll ”Support begär bidragsgivare” under hello prenumeration som du är hello ägare

    ![Användare som läggs till](./media/create-manage-support-requests-using-access-control/user-added.png)

    När Joe loggar in hello portal, ser han hello prenumeration toowhich han har lagts till.

7. Joe klickar du på ”ny supportbegäran” hello ”hjälp och Support” bladet och kan skapa supportärenden för ”Visual Studio Ultimate med MSDN”

    ![Ny supportbegäran](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. Klicka på ”alla stöder begäranden” Joe visas hello lista över supportärenden som skapats för den här prenumerationen ![fallet detaljvy](./media/create-manage-support-requests-using-access-control/case-details-view.png)

## <a name="remove-support-request-access-in-hello-azure-portal"></a>Ta bort support begär åtkomst i hello Azure-portalen

Precis som det är möjligt toogrant åtkomst tooa användaren toocreate och hantera supportärenden är möjliga tooremove åtkomst för samt hello-användare.
tooremove hello möjlighet toocreate och hantera supportärenden, gå toohello prenumeration, klicka på ”inställningar” och klicka hello användare (i det här fallet Joe).
Högerklicka på hello rollnamn, ”Support begär bidragsgivare” och klicka på ”Ta bort”

![Ta bort stöd för begäran om åtkomst](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

När Joe loggar i toohello portal och försöker toocreate en supportbegäran, påträffar han hello följande fel

![Fel-2-prenumeration](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

Johan kan inte se någon stöder begäranden när han klickar ”alla stöder begäranden”

![Case information vy-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
