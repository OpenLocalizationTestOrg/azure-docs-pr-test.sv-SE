---
title: "aaaSet upp PowerShell toocreate en virtuell dator för hello Marketplace | Microsoft Docs"
description: "Instruktioner för att installera Azure PowerShell och använda det som en valfri process flow toocreate VM-avbildningar toodeploy till, och sälja på, hello Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e19d6cda-76df-4e42-be84-c9fe47a636db
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2016
ms.author: hascipio
ms.openlocfilehash: cd2ebad7472248b8f921706e1a8c82d41f33b9cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-azure-powershell-toocreate-an-offer-for-hello-azure-marketplace"></a>Konfigurera Azure PowerShell toocreate ett erbjudande om hello Azure Marketplace
Detaljerad information om hur tooset upp PowerShell i Azure, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). En enkel metod är toouse hello certifikat-metoden, som hämtar och importerar ett certifikat som krävs för autentisering. tooobtain hello behövs certifikat använder hello **Get-AzurePublishSettingsFile** cmdlet. Spara hello-filen när du uppmanas. tooimport hello certifikatet till en PowerShell-session, Använd hello **importera AzurePublishSettingsFile** cmdlet.

tooconfigure och lagra hello vanliga Microsoft Azure-prenumerationsinställningar för hello PowerShell-sessionen använder hello **Set AzureSubscription** och **Välj AzureSubscription** cmdlets:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

hello första kommandot associerar standardkontot för lagring med hello prenumeration (behövs för vissa etablering VM-åtgärder).  hello gör andra hello prenumeration hello aktuella en (som identifieras av andra cmdlet: ar).

## <a name="see-also"></a>Se även
* [Komma igång: hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md)
* [Skapa en avbildning av virtuell dator för hello Marketplace](marketplace-publishing-vm-image-creation.md)

