---
title: aaaAzure Security Center och Windows-datorer i Azure | Microsoft Docs
description: "Läs mer om säkerheten för din Windows Azure-dator med Azure Security Center."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 238bf4e266a24a536d35dd647db6056ab39a1f1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a>Övervaka virtuella säkerhet med hjälp av Azure Security Center

Azure Security Center kan hjälpa dig få insyn i Azure-resurs säkerhetsåtgärder. Security Center ger övervakning av integrerad säkerhet. Det kan upptäcka hot som annars kan förbli oupptäckta. I kursen får du lära dig om Azure Security Center och hur du:
 
> [!div class="checklist"]
> * Konfigurera datainsamling
> * Ställa in säkerhetsprinciper
> * Visa och lösa konfigurationsproblem hälsa
> * Granska identifierade hot  

## <a name="security-center-overview"></a>Översikt över Security Center

Security Center identifierar potentiella konfigurationsproblem för virtuell dator (VM) och mål säkerhetshot. Dessa kan innehålla virtuella datorer som saknar nätverkssäkerhetsgrupper och okrypterade diskar brute force-attacker för Remote Desktop Protocol (RDP). hello information visas på instrumentpanelen som hello Security Center enkelt att läsa diagram.

tooaccess hello instrumentpanelen i Security Center, i hello Azure-portalen på hello-menyn och väljer **Security Center**. På instrumentpanelen hello du finns hello säkerhetshälsa i Azure-miljön, hitta en uppräkning av aktuella rekommendationer och visa hello aktuell status för hot aviseringar. Du kan expandera varje övergripande diagram toosee detalj.

![Instrumentpanelen Security Center](./media/tutorial-azure-security/asc-dash.png)

Security Center är mer omfattande än data identifiering tooprovide rekommendationer för problem som upptäcks. Om en virtuell dator har distribuerats utan ett anslutet nätverkssäkerhetsgrupp, visar Säkerhetscenter en rekommendation med steg du kan vidta. Du kan hämta automatiska reparationer utan att lämna hello kontexten för Security Center.  

![Rekommendationer](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Konfigurera datainsamling

Innan du kan få en överblick över VM säkerhetskonfigurationer, måste tooset in insamling av Security Center. Detta innebär att aktivera insamling av data och skapa ett Azure storage-konto toohold insamlade data. 

1. Klicka på instrumentpanelen för hello Security Center **säkerhetsprincip**, och sedan välja din prenumeration. 
2. För **datainsamling**väljer **på**.
3. Välj toocreate ett lagringskonto **väljer något lagringskonto**. Markera **OK**.
4. På hello **säkerhetsprincip** bladet väljer **spara**. 

hello Säkerhetscenter data collection agent installeras på alla virtuella datorer och datainsamlingen påbörjas. 

## <a name="set-up-a-security-policy"></a>Ställ in en säkerhetsprincip

Säkerhetsprinciper finns används toodefine hello objekt som Security Center samlar in data och ger rekommendationer. Du kan tillämpa olika principer toodifferent uppsättningar med Azure-resurser. Även om Azure-resurser som standard ska utvärderas mot alla principobjekt kan du inaktivera enskilda principobjekt för alla Azure-resurser eller för en resursgrupp. Detaljerad information om säkerhetsprinciper från security Center finns [ställa in säkerhetsprinciper i Azure Security Center](../../security-center/security-center-policies.md). 

tooset in en säkerhetsprincip för alla Azure-resurser:

1. Välj på instrumentpanelen för hello Security Center **säkerhetsprincip**, och sedan välja din prenumeration.
2. Välj **skyddsprincip**.
3. Aktivera eller inaktivera principobjekt som du vill tooapply tooall Azure-resurser.
4. När du är klar med att välja inställningarna väljer **OK**.
5. På hello **säkerhetsprincip** bladet väljer **spara**. 

tooset en princip för en viss resursgrupp:

1. Välj på instrumentpanelen för hello Security Center **säkerhetsprincip**, och välj sedan en resursgrupp.
2. Välj **skyddsprincip**.
3. Aktivera eller inaktivera principobjekt som du vill tooapply toohello resursgruppen.
4. Under **arv**väljer **unik**.
5. När du är klar med att välja inställningarna väljer **OK**.
6. På hello **säkerhetsprincip** bladet väljer **spara**.  

Du kan också stänga av insamling av data för en viss resursgrupp på den här sidan.

I följande exempel hello, en unik princip har skapats för en resursgrupp med namnet *myResoureGroup*. Disk kryptering och web application brandväggen rekommendationer är inaktiverade i den här principen.

![Unik princip](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Visa konfigurationshälsa för VM

När du har aktiverat datainsamling och ställa in en säkerhetsprincip, börjar Security Center tooprovide aviseringar och rekommendationer. Eftersom virtuella datorer distribueras är hello data collection agenten installerad. Security Center består av data för hello nya virtuella datorer. Detaljerad information om hälsa för VM-konfiguration, se [skydda dina virtuella datorer i Security Center](../../security-center/security-center-virtual-machine-recommendations.md). 

När data har samlats in sammanställs hello resurshälsa för varje virtuell dator och relaterade Azure-resurs. hello information visas i ett enkelt att läsa diagram. 

tooview resource health:

1.  På instrumentpanelen för hello Security Center under **resurssäkerhetshälsa**väljer **Compute**. 
2.  På hello **Compute** bladet väljer **virtuella datorer**. Den här vyn visar en sammanfattning av hello Konfigurationsstatus för din virtuella dator.

![Beräkna hälsa](./media/tutorial-azure-security/compute-health.png)

toosee alla rekommendationer för en virtuell dator, Välj hello VM. Rekommendationer och reparation beskrivs i detalj i hello nästa avsnitt i den här kursen.

## <a name="remediate-configuration-issues"></a>Åtgärda problem med konfiguration

När Security Center startar toopopulate med konfigurationsdata görs rekommendationer baserat på hello säkerhetsprincip som du konfigurerar. Till exempel om en virtuell dator har konfigurerats utan en nätverkssäkerhetsgrupp, görs en rekommendation toocreate en. 

toosee en lista över alla rekommendationer: 

1. Välj på instrumentpanelen för hello Security Center **rekommendationer**.
2. Välj en specifik rekommendation. En lista över alla resurser som gäller hello rekommendation visas.
3. tooapply en rekommendation, välja en specifik resurs. 
4. Följ instruktionerna för hello för steg. 

I många fall Security Center innehåller tillämplig steg du kan vidta tooaddress en rekommendation utan att lämna Security Center. I följande exempel hello, upptäcks Security Center en nätverkssäkerhetsgrupp som har en obegränsad inkommande regel. Hello rekommendation på sidan kan du välja hello **redigera regler för inkommande trafik** knappen. hello-användargränssnitt som är nödvändiga toomodify hello regel visas. 

![Rekommendationer](./media/tutorial-azure-security/remediation.png)

De är märkta som löst som rekommendationer har åtgärdats. 

## <a name="view-detected-threats"></a>Visa identifierade hot

Dessutom visar tooresource rekommendationer, Security Center hotidentifieringsaviseringar. hello aviseringar säkerhetsfunktion sammanställer data som samlas in från varje VM, Azure nätverk loggar och anslutna partner solutions toodetect säkerhetshot mot Azure-resurser. För detaljerad information om funktionerna i Security Center threat detection Se [identifieringsfunktionerna i Azure Security Center](../../security-center/security-center-detection-capabilities.md).

hello säkerhetsfunktion aviseringar kräver hello Security Center priser nivå toobe ökade från *lediga* för*Standard*. En 30-dagars **kostnadsfri utvärderingsversion** är tillgänglig när du flyttar toothis högre prisnivå. 

toochange hello prisnivån:  

1. Klicka på instrumentpanelen för hello Security Center **säkerhetsprincip**, och sedan välja din prenumeration.
2. Välj **prisnivå**.
3. Välj hello ny nivå och välj sedan **Välj**.
4. På hello **säkerhetsprincip** bladet väljer **spara**. 

När du har ändrat hello prisnivån börjar hello säkerhet aviseringar diagram toopopulate som säkerhetshot identifieras.

![Säkerhetsaviseringar](./media/tutorial-azure-security/security-alerts.png)

Välj en avisering tooview information. Du kan till exempel finns en beskrivning av hello hot, hello identifieringstiden alla hot försök och hello rekommenderade åtgärderna. I följande exempel hello, upptäcktes en RDP brute force-attacker med 294 RDP lösenordsförsök. En rekommenderad lösning på problemet.

![RDP-attack](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen, Ställ in Azure Security Center och granskas virtuella datorer i Security Center. Du har lärt dig att:

> [!div class="checklist"]
> * Konfigurera datainsamling
> * Ställa in säkerhetsprinciper
> * Visa och lösa konfigurationsproblem hälsa
> * Granska identifierade hot

Avancera toohello nästa självstudiekurs toolearn hur toocreate CI/CD-pipeline med Visual Studio Team Services och en Windows virtuell dator som kör IIS.

> [!div class="nextstepaction"]
> [Visual Studio Team Services CI/CD-pipeline](./tutorial-vsts-iis-cicd.md)
