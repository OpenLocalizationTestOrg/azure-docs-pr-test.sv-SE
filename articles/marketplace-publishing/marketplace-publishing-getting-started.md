---
title: "aaaOverview på hur toocreate och distribuera ett erbjudande toohello Marketplace | Microsoft Docs"
description: "Förstå hello steg krävs toobecome en godkänd Microsoft-utvecklare och skapa och distribuera en avbildning av virtuell dator, mall, datatjänst eller utvecklare tjänst i hello Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5343bd26-c6e4-4589-85b7-4a2c00bba8ab
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio
ms.openlocfilehash: ac5480b98b8b1021a595db951ed9c974f74415dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-and-manage-an-offer-in-hello-azure-marketplace"></a>Publicera och hantera ett erbjudande i hello Azure Marketplace
Den här artikeln anges toohelp utvecklare skapa, distribuera och hantera sina lösningar som anges i hello Azure Marketplace för andra Azure-kunder och partners toopurchase och användning.

## <a name="marketplace-publishing"></a>Marketplace-publicering
Som en Azure utgivare kan du distribuera och sälja innovativa lösning eller tooother tjänstutvecklare, ISV: er, och IT-proffs som hello Marketplace. Du kan nå via hello Marketplace, kunder som vill tooquickly utveckla sina molnbaserade program och mobila lösningar. Om din lösning riktar sig till användare i verksamheten, kanske du vill tooconsider hello [AppSource](http://appsource.microsoft.com) marketplace.


## <a name="supported-types-of-solutions"></a>Typer som stöds av lösningar
hello första som du vill använda toodo som en utgivare är toodefine vilken typ av lösning för ditt företag erbjuder. hello Marketplace stöder följande typer av erbjudanden hello:

|Typ av lösning|Virtuell dator|Lösningsmall|
|---|---|---|
|**Definition**|Förkonfigurerade avbildningar med ett fullständigt installerat operativsystem och en eller flera program. En avbildning av virtuell dator innehåller hello information behövs toocreate och distribuera virtuella datorer i hello virtuella datorer i Azure-tjänsten.|En datastruktur som kan referera till en eller flera distinkta Azure-tjänster, inklusive tjänster publicerade av andra säljare. Azure-prenumeranter kan använda den toodeploy ett eller flera erbjudanden på ett enda, samordnad sätt.|
|**Exempel**|Som en Azure utgivare du skapat och verifiera en virtuell dator med en innovativa database-tjänsten. Andra Azure-prenumeranter vill tooprocure och distribuera den här virtuella datorn i sina molnmiljöer för tjänsten.|Du har paketerade en uppsättning tjänster från i Azure som gör det snabbt toodeploy molntjänster med belastningsutjämning, förbättrad säkerhet och hög tillgänglighet som en Azure utgivare. Andra Azure-prenumeranter kan spara tid genom upphandling hello lösningsmall som uppfyller sina mål. De inte har toomanually hitta, köpa, distribuera och konfigurera hello samma eller liknande Azure-tjänster.|

> [!NOTE]
> Vissa steg delas mellan hello olika typer av lösningar och andra är distinkt toohello respektive typ av lösning. Den här artikeln innehåller en kort översikt av hello steg behöver du toocomplete för någon typ av lösningen.

## <a name="publish-a-solution"></a>Publicera en lösning
![Utse, registrera, publicera](media/marketplace-publishing-getting-started/img01.png)

### <a name="nominate-your-solution-for-pre-approval"></a>Ange din lösning för före godkännande
toopublish en virtuell dator [lösning](https://createopportunity.azurewebsites.net) toohello Marketplace, fullständig hello Azure för Microsoft Certified **lösning kandidat formuläret**.

>[!NOTE]
> Om du arbetar med en Partner Kontoansvariga eller en DX Partner Manager be dem toonominate din lösning för hello Azure certifierade program. Du kan även gå toohello [Microsoft Azure-certifierad](http://createopportunity.azurewebsites.net) webbsidan och Fyll ut hello formulär. Ange hello e-postadress till ditt Partner-kontoansvarige eller DX Partner Manager i hello **Microsoft Sponsor Kontakta** rutan.

Om du uppfyller kriterierna för hello i hello [Azure Marketplace deltagande principer](http://go.microsoft.com/fwlink/?LinkID=526833) och din ansökan har godkänts, vi börja jobba med du tooonboard din lösning toohello Marketplace.

### <a name="register-your-account-as-a-microsoft-seller"></a>Registrera ditt konto som ett Microsoft-säljare
Registrera ditt Microsoft-konto som en [Microsoft Developer konto](marketplace-publishing-accounts-creation-registration.md).

### <a name="publish-your-solution"></a>Publicera din lösning
toopublish en lösning toohello Marketplace, Följ dessa steg:
1. Uppfylla hello sökassistent krav.

    a. Uppfyll hello [sökassistent krav](marketplace-publishing-pre-requisites.md).

    b. Uppfyll hello [VM tekniska krav](marketplace-publishing-vm-image-creation-prerequisites.md).

    c. Uppfyll hello [mall tekniska krav för lösningen](marketplace-publishing-solution-template-creation-prerequisites.md).

2. Skapa erbjudandet.

    a. Skapa en [virtuella](marketplace-publishing-vm-image-creation.md) erbjuder.

    b. Skapa en [lösningsmall](marketplace-publishing-solution-template-creation.md) erbjuder.

3. Skapa erbjudandet [marknadsföring innehåll](marketplace-publishing-push-to-staging.md).

4. Testa erbjudandet i Förproduktion.

    a. Testa VM erbjudandet i [mellanlagring](marketplace-publishing-vm-image-test-in-staging.md).

    b. Testa din lösning mallen erbjudandet i [mellanlagring](marketplace-publishing-solution-template-test-in-staging.md).

5. Distribuera din erbjudande toohello [Marketplace](marketplace-publishing-push-to-production.md).


### <a name="create-and-manage-a-virtual-machine-image"></a>Skapa och hantera en avbildning av virtuell dator
Skapa och hantera en VM-avbildning med hjälp av följande resurser:
* Skapa en VM-avbildning [lokalt](marketplace-publishing-vm-image-creation-on-premise.md).
* Skapa en virtuell dator som kör [Windows i hello Azure-portalen](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Skapa en virtuell dator som kör [Linux i hello Azure-portalen](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Felsöka vanliga problem som påträffas under [VHD skapa](marketplace-publishing-vm-image-creation-troubleshooting.md).

## <a name="manage-your-solution"></a>Hantera din lösning
Hantera din lösning med hjälp från hello följande resurser:
* [Läs hello släppts guiden för den virtuella datorn är](marketplace-publishing-vm-image-post-publishing.md)
* [Uppdatera hello sökassistent information om ett erbjudande eller en SKU](marketplace-publishing-vm-image-post-publishing.md#update-the-nontechnical-details-of-an-offer-or-a-sku)
* [Uppdatera hello teknisk information om ett erbjudande eller en SKU](marketplace-publishing-vm-image-post-publishing.md#update-the-technical-details-of-a-sku)
* [Lägg till en ny SKU under listan erbjudande](marketplace-publishing-vm-image-post-publishing.md#add-a-new-sku-under-a-listed-offer)
* [Ändra hello datadiskar listade SKU](marketplace-publishing-vm-image-post-publishing.md#change-the-data-disk-count-for-a-listed-sku)
* [Ta bort ett listade erbjudande från hello Marketplace](marketplace-publishing-vm-image-post-publishing.md)
* [Ta bort en listade SKU från hello Marketplace](marketplace-publishing-vm-image-post-publishing.md#delete-a-listed-sku-from-the-marketplace)
* [Ta bort hello nuvarande version av listan SKU från hello Marketplace](marketplace-publishing-vm-image-post-publishing.md#delete-the-current-version-of-a-listed-sku-from-the-marketplace)
* [Återställa hello visar pris tooproduction värden](marketplace-publishing-vm-image-post-publishing.md#revert-the-listing-price-to-production-values)
* [Återställa hello fakturering modellen tooproduction värden](marketplace-publishing-vm-image-post-publishing.md#revert-the-billing-model-to-production-values)
* [Återställa hello inställningar för listan SKU toohello produktionsvärdet](marketplace-publishing-vm-image-post-publishing.md#revert-the-visibility-setting-of-a-listed-sku-to-the-production-value)
* [Ändra din leverantör av Molnlösningar återförsäljare morot](marketplace-publishing-csp-incentive.md)
* [Förstå din beloppet reporting](marketplace-publishing-report-payout.md)
* [Få support som en utgivare](marketplace-publishing-get-publisher-support.md)

## <a name="additional-resources"></a>Ytterligare resurser
[Konfigurera Azure PowerShell](marketplace-publishing-powershell-setup.md)
