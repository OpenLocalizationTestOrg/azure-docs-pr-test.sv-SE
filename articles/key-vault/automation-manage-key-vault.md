---
title: "aaaManage Azure Key Vault med hjälp av Azure Automation | Microsoft Docs"
description: "Läs mer om hur hello Azure Automation-tjänsten kan vara används toomanage Azure Key Vault."
services: Key-Vault, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 4e780762-19b6-4ca6-b894-ebb44c538f35
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte;csand
ms.openlocfilehash: 7f46ecc1206a96e8aeb1d086285461cb5b205472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a>Hantera Azure Key Vault med hjälp av Azure Automation
Den här guiden beskrivs toohello Azure Automation-tjänsten och hur det kan vara används toosimplify hanteringen av dina nycklar och hemligheter i Azure Key Vault.

## <a name="what-is-azure-automation"></a>Vad är Azure Automation?
[Azure Automation](../automation/automation-intro.md) är en Azure-tjänst som förenklar molnhantering via automatisering och önskad tillståndskonfiguration. Med Azure Automation kan manuell upprepade, tidskrävande och felbenägna aktiviteter vara automatisk tooincrease tillförlitlighet, effektivitet och tid toovalue för din organisation.

Azure Automation ger en Körningsmotor för arbetsflöden för mycket tillförlitliga, hög tillgänglighet som kan skalas toomeet dina behov. I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.

Minska operativ tillsyn och frigör IT och DevOps personal toofocus på arbete som lägger till affärsvärde genom att flytta dina molntjänster management uppgifter toobe körs automatiskt av Azure Automation.

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Hur Azure Automation kan hantera Azure Key Vault?
Key Vault kan hanteras i Azure Automation med hjälp av hello [AzureRM Key Vault-cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) och [Azure klassiska Key Vault-cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx). hello Azure-modulen för hantering av den klassiska Key Vault finns automatiskt i Azure Automation och du kan importera hello [AzureRM KeyVault modulen](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) i Azure Automation så att du kan utföra många av din Key Vault-hantering uppgifter i hello-tjänsten. Du kan också koppla dessa cmdlets i Azure Automation med hello-cmdlets för andra Azure-tjänster, tooautomate komplicerade uppgifter över Azure-tjänster och 3 part.

Du kan utföra dessa uppgifter, bland annat med hello Azure Key Vault-cmdlets: 

* Skapa och konfigurera ett nyckelvalv
* Skapa eller importera en nyckel
* Skapa eller uppdatera en hemlighet
* Uppdatera attribut för en nyckel
* Hämta en nyckel eller hemlighet.
* Ta bort en nyckel eller hemlighet.

Här följer några exempel på hur du använder PowerShell toomanage Key Vault:  

* [Azure Key Vault - steg-för-steg](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Installera och konfigurera en Azure Key Vault](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Azure Automation och hur det kan vara används toomanage Azure Key Vault kan du följa dessa länkar toolearn mer om Azure Automation.

* Se hello Azure Automation [komma igång-Självstudier](../automation/automation-first-runbook-graphical.md).
* Se hello [Azure Key Vault PowerShell-skript](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).

