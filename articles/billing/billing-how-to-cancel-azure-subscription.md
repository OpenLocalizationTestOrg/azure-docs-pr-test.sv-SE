---
title: aaaCancel azureprenumerationen | Microsoft Docs
description: "Beskriver hur toocancel din Azure-prenumeration, som hello kostnadsfri utvärderingsversion"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 3051d6b0-179f-4e3a-bda4-3fee7135eac5
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: genli
ms.openlocfilehash: 198d6ab3de143c071a572db899037f8de85a8cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cancel-your-subscription-for-azure"></a>Avbryta din prenumeration på Azure

Du kan avbryta din Azure-prenumeration som hello [kontoadministratör](billing-subscription-transfer.md#whoisaa). Efter att du avbryter prenumerationen hello slutar din tooAzure tjänster och resurser.

Innan du avbryter prenumerationen:

* Säkerhetskopiera dina data. Om du lagrar data i Azure storage eller SQL, hämta, till exempel en kopia. Om du har en virtuell dator kan du spara en avbildning av det lokalt.
* Stäng av dina tjänster. Gå toohello [resurser sidan i hanteringsportalen för hello](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), och **stoppa** någon med virtuella datorer, program eller andra tjänster.
* Bör du migrera dina data. Se [flytta resurser toonew resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md).

Om du avbryter en betald [Azure-supportplan](https://azure.microsoft.com/support/plans/), fortfarande debiteras du varje månad för hello rest hello 6 månader har löpt ut.

## <a name="cancel-subscription-using-hello-azure-portal"></a>Avbryt prenumeration med hjälp av hello Azure-portalen

1. Välj din prenumeration från hello [prenumerationssidan](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

1. Välj hello prenumeration som du vill använda toocancel och klicka på **avbryta prenumerationen**.

    ![Skärmbild som visar hello Avbryt-knapp](./media/billing-how-to-cancel-azure-subscription/cancel_ibiza.png)

1. Följ anvisningarna och slutför annullering.

## <a name="cancel-subscription-using-hello-azure-account-center"></a>Avbryt prenumeration med hjälp av hello Azure Account Center

1. Logga in toohello [Azure Kontocenter](https://account.windowsazure.com/subscriptions) som hello kontoadministratör.

1. Under **klickar du på en prenumeration tooview information och användning**, Välj hello prenumeration som du vill toocancel.

    ![Skärmbild som visar en exempel-prenumeration som har valts](./media/billing-how-to-cancel-azure-subscription/Selectsub.png)

1. Välj hello höger på sidan hello **Avbryt prenumeration**.

    ![Skärmbild som visar hello Avbryt prenumeration](./media/billing-how-to-cancel-azure-subscription/cancelsub.png)

1. Välj **Ja, avsluta min prenumeration**.

    ![Skärmbild som visar hello Avbryt dialogrutan](./media/billing-how-to-cancel-azure-subscription/cancelbox.png)

1. Klicka på ![Markera knappen symbol](./media/billing-how-to-cancel-azure-subscription/checkbutton.png) tooclose hello dialogrutan fönster och returnera tooyour prenumerationssidan.

## <a name="what-happens-after-i-cancel-my-subscription"></a>Vad händer när jag avsluta min prenumeration?

När du avbryter stoppas fakturering omedelbart. Dock kan det ta upp too10 minuter för hello annullering visar hello-portalen.

Sedan kan har dina tjänster inaktiverats. Det innebär att virtuella datorer har frigjorts, frigörs tillfälliga IP-adresser och lagring är skrivskyddad.

Om du inte är på en kostnadsfri utvärderingsversion eller har krediter som är tillgängliga, är du debiteras för alla utestående användningskostnader genereras mellan din senaste faktureringsperioden och hello annulleringsdatum. Du får den senaste fakturan hello slutet av hello fakturering cykel.

När du avbryter din prenumeration, vänta vi 90 dagar före permanent ta bort data om du behöver tooaccess den eller om du ändrar dig. Vi debitera inte dig för att behålla hello data. Det finns fler toolearn [Microsoft Trust Center - hur vi kan hantera dina data](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="reactivate-subscription"></a>Återaktivera prenumeration

Om du avbryter prenumerationen betalning per användning av misstag kan du [återaktivera i hello Accounts Center](billing-subscription-become-disable.md).

Om din prenumeration inte är betala per användning, kontaktar du support inom 90 dagar efter annullering tooreactivate din prenumeration.

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.

Om du fortfarande har frågor, [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.
