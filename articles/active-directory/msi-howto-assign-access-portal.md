---
title: "Tilldela en MSI-åtkomst till en Azure-resurs som använder Azure portal"
description: "Stegvisa instruktioner för att tilldela en MSI på en resursåtkomst till en annan resurs med hjälp av Azure portal."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.openlocfilehash: 88abc2a9836633e5d88a91e59f7078a388b26068
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Tilldela en hanterade tjänstidentiteten åtkomst till en resurs med hjälp av Azure portal

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

När du har konfigurerat en Azure-resurs med en hanterad tjänst identitet (MSI), kan du ge MSI-åtkomst till en annan resurs, precis som alla säkerhetsobjekt. Den här artikeln visar hur du ge en Azure-dator MSI åtkomst till ett Azure storage-konto med hjälp av Azure portal.

## <a name="prerequisites"></a>Krav

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Använda RBAC tilldela MSI-åtkomst till en annan resurs

När du har aktiverat MSI på en Azure-resurs [, till exempel en Azure VM](msi-qs-configure-portal-windows-vm.md):

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är associerade med Azure-prenumeration som du har konfigurerat MSI.

2. Navigera till en resurs som du vill ändra åtkomstkontroll. I det här exemplet ger vi en virtuell dator i Azure-åtkomst till ett lagringskonto, så vi gå till lagringskontot.

3. Välj den **åtkomstkontroll (IAM)** i resursen och välj **+ Lägg till**. Ange den **rollen**, **tilldelar åtkomst till virtuella**, och ange motsvarande **prenumeration** och **resursgruppen** där resursen finns. Du bör se resursen under området för sökvillkor. Markera resursen och välj **spara**. 

   ![Skärmbild av Access control (IAM)](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  

4. Du kommer tillbaka till huvudsakliga **åtkomstkontroll (IAM)** sidan där du ser en ny post för den resursen MSI. I det här exemplet ”SimpleWinVM” virtuell dator från resursgruppen Demo har **deltagare** åtkomst till lagringskontot.

   ![Skärmbild av Access control (IAM)](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

## <a name="troubleshooting"></a>Felsökning

Om MSI-filerna för resursen inte visas i listan över tillgängliga identiteter, kontrollerar du att MSI har aktiverats. I vårt fall kan vi gå tillbaka till den virtuella Azure-datorn och kontrollera följande:

- Titta på den **Configuration** sidan och kontrollera att värdet för **MSI aktiverat** är **Ja**.
- Titta på den **tillägg** sidan och kontrollera att MSI-tillägget har distribuerats.

Om något är fel kan du behöva distribuera om MSI-filerna på din resurs eller felsöka distributionen misslyckades.

## <a name="related-content"></a>Relaterat innehåll

- En översikt över MSI finns [hanterade tjänstidentiteten översikt](msi-overview.md).
- För att aktivera MSI på en Azure VM, se [konfigurera en Azure VM hanterade tjänsten identitet (MSI) med hjälp av Azure portal](msi-qs-configure-portal-windows-vm.md).


