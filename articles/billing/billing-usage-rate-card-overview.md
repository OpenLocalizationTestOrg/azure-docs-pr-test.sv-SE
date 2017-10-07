---
title: 'aaaAzure fakturering API: er | Microsoft Docs'
description: "Läs mer om Azure Billing användnings- och RateCard APIs som används tooprovide insikter om Azure resursförbrukning och trender."
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/18/2017
ms.author: mobandyo;bryanla
ms.openlocfilehash: b3214996cc3279f76fdc7f0dbd2059c3ae7bb15c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-billing-apis-tooprogrammatically-get-insight-into-your-azure-usage"></a>Använd Azure Billing API: erna tooprogrammatically få bättre överblick på din användning av Azure
Använda Azure Billing API: erna toopull användnings- och data till din önskade verktyg för dataanalys. hello Azure Resursanvändning och RateCard APIs kan hjälpa dig korrekt förutsäga och hantera dina kostnader. hello API: er implementeras som en Resource Provider och en del av hello API: er som exponeras av hello Azure Resource Manager.  

## <a name="azure-invoice-download-api-preview"></a>Azure faktura Download API (förhandsgranskning)
En gång hello [Anmäl har slutförts](billing-manage-access.md#opt-in), ladda ned fakturor med hello förhandsversionen av [faktura API](/rest/api/billing). hello-funktioner inkluderar:

* **Azure rollbaserad åtkomstkontroll** -Konfigurera åtkomst principer på hello [Azure-portalen](https://portal.azure.com) eller via [Azure PowerShell-cmdlets](/powershell/azure/overview) toospecify vilka användare eller program kan få åtkomst toohello prenumeration användningsdata. Anropare måste använda standard Azure Active Directory-token för autentisering. Lägg till hello anroparen tooeither hello fakturering Reader, Reader, ägare eller deltagare rollen tooget åtkomst toohello användningsdata för en viss Azure-prenumeration.
* **Datum filtrering** -Använd hello `$filter` parametern tooget alla hello fakturor i omvänd kronologisk ordning av hello faktura slutdatum. 

> [!NOTE]
> Den här funktionen är i första versionen av förhandsgranskning och kan vara ämne toobackward inkompatibel ändringar. För närvarande, är det inte tillgängliga för vissa prenumerationserbjudanden (EA, CSP, AIO stöds inte) och Azure Tyskland.

## <a name="azure-resource-usage-api-preview"></a>Azure Resursanvändning API (förhandsgranskning)
Använd hello Azure [resurs användning API](https://msdn.microsoft.com/library/azure/mt219003) tooget uppskattade Azure förbrukningsdata. hello API innehåller:

* **Azure rollbaserad åtkomstkontroll** -Konfigurera åtkomst principer på hello [Azure-portalen](https://portal.azure.com) eller via [Azure PowerShell-cmdlets](/powershell/azure/overview) toospecify vilka användare eller program kan få åtkomst toohello prenumeration användningsdata. Anropare måste använda standard Azure Active Directory-token för autentisering. Lägg till hello anroparen tooeither hello fakturering Reader, Reader, ägare eller deltagare rollen tooget åtkomst toohello användningsdata för en viss Azure-prenumeration.
* **Varje timme och dag** – anropare kan ange om de vill ha sina Azure användningsdata i varje timme tidsgrupper eller dagligen tidsgrupper. hello standard är varje dag.
* **Instansen metadata (inklusive resurstaggar)** – hämta instansen detaljnivåer som hello fullständiga resurs-uri (/subscriptions/ {prenumerations-id} /..), hello resursinformation för gruppen och resurstaggar. Dessa metadata hjälper dig att deterministiskt och allokera programmässigt användning av hello taggar för användningsområden som mellan debitering.
* **Resursmetadata** -resursinformation som hello mätaren namn, mätaren kategori, mätaren underkategori, enhet och region ge hello anroparen en bättre förståelse för vad förbrukades. Vi arbetar också tooalign resurs metadata terminologi över hello Azure-portalen, Azure användning CSV, EA CSV, fakturering och andra offentliga upplevelser, toolet du korrelera data över upplevelser.
* **Användning för alla erbjuder typer** – användningsdata är tillgängliga för alla erbjuder typer som betala per användning, MSDN, Summa, kredit och EA.

## <a name="azure-resource-ratecard-api-preview"></a>Azure-resurs RateCard API (förhandsgranskning)
Använd hello [Azure Resource RateCard API](https://msdn.microsoft.com/library/azure/mt219005) tooget hello lista över tillgängliga Azure-resurser och uppskattade prisinformation för varje. hello API innehåller:

* **Azure rollbaserad åtkomstkontroll** -konfigurera principer för åtkomst på hello [Azure-portalen](https://portal.azure.com) eller via [Azure PowerShell-cmdlets](/powershell/azure/overview) toospecify vilka användare eller program kan få åtkomst toohello RateCard data. Anropare måste använda standard Azure Active Directory-token för autentisering. Lägg till hello anroparen tooeither hello Reader, ägare eller deltagare rollen tooget åtkomst toohello användningsdata för en viss Azure-prenumeration.
* **Stöd för betala per användning, MSDN, Summa och kredit erbjuder (EA stöds inte)** -detta API ger Azure erbjudande nivå hastighet information.  hello anroparen av denna API måste klara i hello erbjudande information tooget resursinformation och priser. Vi kan för närvarande inte tooprovide EA priser eftersom EA erbjudanden har anpassat kostnader per registrering. 

## <a name="scenarios"></a>Scenarier
Här följer några hello-scenarier som är möjligt med hello kombination av hello användning och hello RateCard APIs:

* **Azure tillbringar under hello månad** -Använd hello kombination av hello användnings- och RateCard APIs tooget bättre insikter om ditt moln tillbringar under hello månad. Du kan analysera hello varje timme och dag buckets för användnings- och kostnad beräkningarna.
* **Ställa in aviseringar** – använda hello användning och hello RateCard APIs tooget uppskattade molnet förbrukning och kostnader och konfigurera resurs- eller monetära baserade aviseringar.
* **Förutsäga faktura** – Get uppskattade användnings- och molnet tillbringar och tillämpa maskininlärning algoritmer toopredict vilka hello faktura skulle vara hello slutet av hello fakturering cykel.
* **Före förbrukning kostnad analysis** – Använd hello RateCard API toopredict hur mycket din faktura är för din förväntade användning när du flyttar dina arbetsbelastningar tooAzure. Om du har befintliga arbetsbelastningar i andra moln eller privata moln, kan du mappa förbrukningen med hello Azure priser tooget ägna en bättre uppfattning på Azure. Denna uppskattning ger du hello möjlighet toopivot på erbjudande och jämföra och kontrast mellan hello olika erbjudandetyper utöver betala per användning, t.ex. summa och kredit. hello API ger dig även hello möjlighet toosee kostnaden skillnader efter region och låter dig toodo en vad händer om kostnaden analysis toohelp du fattar distributionsbeslut.
* **Konsekvensanalys** -
  
  * Du kan fastställa om det är mer kostnadseffektivt toorun arbetsbelastningar i en annan region, eller på en annan konfiguration av hello Azure-resurs. Azure-resurs kostnaderna kan skilja sig utifrån hello Azure-region som du använder.
  * Du kan också bestämma om en annan typ av Azure-erbjudande ger en bättre hastighet på Azure-resurs.
  
## <a name="partner-solutions"></a>Partnerlösningar
[Microsoft Azure-användning och RateCard API: er aktivera Cloudyn tooProvide ITFM för kunder](billing-usage-rate-card-partner-solution-cloudyn.md) beskriver hello integration upplevelse som erbjuds av Azure API för fakturering partner [Cloudyn](https://www.cloudyn.com/microsoft-azure/). Den här artikeln handlar om deras upplevelser och innehåller en video som visar hur du kan använda Cloudyn och hello Azure Billing API: erna tooget insikter från Azure-förbrukningsdata.

[Molnet Cruiser och Microsoft Azure Billing API-integrering](billing-usage-rate-card-partner-solution-cloudcruiser.md) beskriver hur [moln Cruiser Express för Azure-paket](http://www.cloudcruiser.com/partners/microsoft/) fungerar direkt från hello Windows Azure Pack (WAP)-portalen. Du kan hantera både hello operationella och finansiella aspekter av hello Microsoft Azure privat eller värdbaserade offentligt moln sömlöst från ett enda användargränssnitt.   

## <a name="next-steps"></a>Nästa steg
* Kolla hello kodexempel på GitHub:
  * [Fakturan API-kodexempel](https://go.microsoft.com/fwlink/?linkid=845124)

  * [Användnings-API-kodexempel](https://github.com/Azure-Samples/billing-dotnet-usage-api)

  * [RateCard API-kodexempel](https://github.com/Azure-Samples/billing-dotnet-ratecard-api)

* toolearn mer om hello Azure Resource Manager finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 

* Mer information om hello uppsättning med verktyg nödvändiga toohelp du lär dig om molnet tillbringar finns hello Gartner artikel [marknaden Guide för IT finansiella Management (ITFM) verktyg](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

