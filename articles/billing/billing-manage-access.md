---
title: "aaaManage åtkomst tooAzure fakturering med roller | Microsoft Docs"
description: 
services: 
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: vikdesai
ms.openlocfilehash: 5937fac5ffa5ca204eb03a1dcbc5e800b3d5eb74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-access-toobilling-information-for-azure-using-role-based-access-control"></a>Hantera åtkomst toobilling information för Azure med hjälp av rollbaserad åtkomstkontroll

Du kan ge åtkomst för Azure fakturering information toomembers gruppmedlemmarna genom att tilldela en hello efter användarens roller tooyour prenumeration: kontoadministratör, administratör, medadministratör, ägare, deltagare, läsare och fakturering läsare. De skulle ha åtkomst till toobilling information i hello [Azure-portalen](https://portal.azure.com/), och de kan använda hello [fakturering API: er](billing-usage-rate-card-overview.md) tooprogrammatically hämta fakturor (en gång deltar i) och användningsinformation. Mer information om vem som kan ge roller, och vilka roller kan göra vad, se [roller i Azure RBAC](../active-directory/role-based-access-built-in-roles.md).

## <a name="opt-in"></a>Att tillåta ytterligare användare tooaccess fakturor

hello kontoadministratör måste välja med hello [Azure-portalen](https://portal.azure.com/) Tillåt åtkomst tooinvoices för andra användare och via API.

1. Eftersom hello kontoadministratör, väljer din prenumeration från hello [prenumerationer bladet](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) i Azure-portalen.

1. Välj **fakturor** och sedan **åtkomst till tooinvoices**.

    ![Skärmbild som visar hur toodelegate åt tooinvoices](./media/billing-manage-access/AA-optin.png)

1. Aktivera **på** hello åtkomst följt av hello ändringar, tooallow användare i prenumerationen begränsade roller toodownload faktura.

    ![Skärmbild som visar-off toodelegate åtkomst tooinvoice](./media/billing-manage-access/AA-optinAllow.png)

Börjar tillåter administratör, medadministratör, ägare, deltagare, läsare och fakturering Reader på hello prenumeration toodownload PDF fakturor i hello Azure-portalen. Fakturor som är äldre än December 2016 är dock tillgängligt endast toohello kontoadministratör för tillfället.

hello kontoadministratör kan också konfigurera toohave fakturor skickas via e-post. Det finns fler toolearn [hämta fakturan i e-post](billing-download-azure-invoice-daily-usage-date.md).

## <a name="adding-users-toohello-billing-reader-role"></a>Att lägga till rollen för användare toohello fakturering läsare

rollen för hello fakturering läsare har skrivskyddad åtkomst toosubscription faktureringsinformation i Azure-portalen och ingen åtkomst tooservices, till exempel virtuella datorer och storage-konton. Tilldela hello fakturering Reader rollen toosomeone som behöver komma åt toohello faktureringsinformation för prenumerationen men inte hello möjlighet toomanage Azure services. Den här rollen är lämplig för användare i en organisation som endast utför ekonomi- och hantering för Azure-prenumerationer.

1. Välj din prenumeration från hello [prenumerationer bladet](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) i Azure-portalen.

1. Välj **åtkomstkontroll (IAM)** och klicka sedan på **Lägg till**.

    ![Skärmbild som visar IAM i hello prenumerationsbladet](./media/billing-manage-access/select-iam.PNG)

1. Välj **fakturering Reader** i hello **Välj en roll** sidan.

    ![Skärmbild som visar fakturering läsare i hello popup-vy](./media/billing-manage-access/select-roles.PNG)

1. Skriv hello e-post för hello användare du vill tooinvite och klickar sedan **OK** toosend hello inbjudan.

    ![Skärmbild som visar tooenter e-tooinvite någon](./media/billing-manage-access/add-user.PNG)

1. Följ anvisningarna i hello inbjudan e-toolog i som en läsare för fakturering.

    ![Skärmbild som visar vad hello fakturering läsaren kan se i Azure-portalen](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> hello fakturering Reader funktionen är i förhandsvisning och ännu stöder inte företagsprenumerationer (EA) eller icke-globala moln.

## <a name="adding-users-tooother-roles"></a>Lägga till användare tooother roller

Användare i andra roller, till exempel ägare eller deltagare, kan komma åt inte bara faktureringsinformation, men Azure-tjänster. dessa roller finns i toomanage [lägga till eller ändra Azure-administratörsroller som hanterar hello prenumeration eller tjänster](billing-add-change-azure-subscription-administrator.md).

## <a name="who-can-access-hello-account-centerhttpsaccountwindowsazurecom"></a>Vem som kan komma åt hello [Kontocenter](https://account.windowsazure.com)?

Endast hello kontoadministratör kan logga in toohello kontocenter. hello kontoadministratör äger hello juridiska hello prenumeration. Hello person som registrerade sig för eller köpt hello Azure-prenumeration är som standard hello kontoadministratör, såvida inte hello [prenumeration ägarskap överfördes](billing-subscription-transfer.md) toosomebody annan. hello kontoadministratör kan skapa prenumerationer, avbryta prenumerationer, ändra hello faktureringsadress för en prenumeration och hantera åtkomstregler för hello prenumeration.

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.

Om du fortfarande har fler frågor, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.
