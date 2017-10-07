---
title: aaaService kartan integrering med System Center Operations Manager | Microsoft Docs
description: "Tjänstkarta är en Operations Management Suite som automatiskt identifierar programkomponenter i Windows och Linux-system och kartor hello kommunikation mellan tjänster. Den här artikeln beskrivs med hjälp av Tjänstkarta tooautomatically skapa diagram för distribuerade program i Operations Manager."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: e8614a5a-9cf8-4c81-8931-896d358ad2cb
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: bwren;dairwin
ms.openlocfilehash: cff9cce2559448ec3a5fd14087b867f314716560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-map-integration-with-system-center-operations-manager"></a>Tjänstkarta integrering med System Center Operations Manager
  > [!NOTE]
  > Den här funktionen är tillgänglig som förhandsversion.
  > 
  
Operations Management Suite Tjänstkarta automatiskt identifierar programkomponenter på Windows- och Linux-system och mappar hello kommunikation mellan tjänster. Tjänstkarta kan tooview servrar hello väg du betrakta dem som sammanlänkade system som levererar kritiska tjänster. Tjänstkarta visar anslutningar mellan servrar, processer och portar i alla TCP-anslutna arkitektur med ingen konfiguration krävs förutom hello installation av en agent. Mer information finns i hello [Tjänstkarta dokumentationen](operations-management-suite-service-map.md).

Med den här integreringen mellan Tjänstkarta och System Center Operations Manager kan automatiskt skapa diagram för distribuerade program i Operations Manager som är baserade på hello dynamiskt beroende maps i Tjänstkartan.

## <a name="prerequisites"></a>Krav
* En Operations Manager-hanteringsgrupp som hanterar en uppsättning servrar.
* En Operations Management Suite-arbetsyta med hello Tjänstkarta lösning som är aktiverad.
* En uppsättning servrar (minst) som hanteras av Operations Manager och skicka data tooService kartan. Windows- och Linux-servrar stöds.
* Ett huvudnamn för tjänsten med åtkomst toohello Azure-prenumeration som är associerad med hello Operations Management Suite-arbetsyta. Mer information finns för[skapa ett huvudnamn för tjänsten](#creating-a-service-principal).

## <a name="install-hello-service-map-management-pack"></a>Installera hello Tjänstkarta management pack
Du kan aktivera hello integrering mellan Operations Manager och Tjänstkarta genom att importera hello Microsoft.SystemCenter.ServiceMap management pack-samling (Microsoft.SystemCenter.ServiceMap.mpb). hello paket innehåller hello följande hanteringspaket:
* Microsoft Service kartvy program
* Microsoft System Center Service Map internt
* Microsoft System Center Service Map åsidosättningar
* Microsoft System Center-Tjänstkarta

## <a name="configure-hello-service-map-integration"></a>Konfigurera hello Tjänstkarta-integrering
När du har installerat hello Tjänstkarta hanteringspaket, en ny nod **Tjänstkarta**, visas under **Operations Management Suite** i hello **Administration** fönstret. 

tooconfigure Tjänstkarta integration hello följande:

1. tooopen hello-konfigurationsguiden i hello **översikt över tjänsten** rutan klickar du på **lägger du till arbetsytan**.  

    ![Översikt över tjänsten fönstret](media/oms-service-map/scom-configuration.png)

2. I hello **anslutningskonfiguration** , ange hello innehavarens namn eller ID, program-ID (även kallat hello användarnamn eller clientID) och lösenord för hello tjänstens huvudnamn och klicka sedan på **nästa**. Mer information finns för[skapa ett huvudnamn för tjänsten](#creating-a-service-principal).

    ![hello anslutningskonfiguration fönster](media/oms-service-map/scom-config-spn.png)

3. I hello **prenumeration markeringen** fönstret Välj hello Azure-prenumeration, Azure-resursgrupp (hello en som innehåller hello Operations Management Suite-arbetsyta) och Operations Management Suite-arbetsyta och klicka sedan på **Nästa**.

    ![hello Operations Manager Configuration-arbetsyta](media/oms-service-map/scom-config-workspace.png)

4. I hello **val av datorn** fönstret kan du välja vilka Service Map datorgrupper du vill toosync tooOperations Manager. Klicka på **Lägg till/ta bort datorgrupper**, Välj grupper hello listan över **tillgängliga datorgrupper**, och klicka på **Lägg till**.  När du har valt grupper klickar du på **Ok** toofinish.
    
    ![hello datorgrupper för Operations Manager konfiguration](media/oms-service-map/scom-config-machine-groups.png)
    
5. I hello **Serverval** fönstret du konfigurerar hello tjänstgruppen kartan servrar med hello-servrar som du vill toosync mellan Operations Manager och Tjänstkartan. Klicka på **Lägg till/ta bort servrar**.   
    
    Hello-server måste vara för hello integration toobuild ett distribuerat program diagram för en server:

    * Hanteras av Operations Manager
    * Hanteras av en Tjänstkarta
    * Som anges i hello tjänstgruppen kartan servrar

    ![hello konfigurationsgruppen för Operations Manager](media/oms-service-map/scom-config-group.png)

6. Valfritt: Välj hello Management Server resource pool toocommunicate med Operations Management Suite och klicka sedan på **lägger du till arbetsytan**.

    ![hello resurspool för Operations Manager-konfiguration](media/oms-service-map/scom-config-pool.png)

    Det kan ta en minut tooconfigure och registrera hello Operations Management Suite-arbetsyta. När den är konfigurerad initieras Operations Manager hello första Tjänstkarta synkronisering från Operations Management Suite.

    ![hello resurspool för Operations Manager-konfiguration](media/oms-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a>Övervakare för Tjänstmappning
Efter hello Operations Management Suite-arbetsyta är ansluten i hello visas en ny mapp Tjänstkartan **övervakning** rutan hello Operations Manager-konsolen.

![hello övervakning för Operations Manager-fönstret](media/oms-service-map/scom-monitoring.png)

Hej Tjänstkarta mapp har fyra noder:
* **Aktiva aviseringar**: Visar en lista över alla hello aktiva aviseringar om hello kommunikation mellan Operations Manager och Tjänstkartan.  Observera att dessa varningar inte är Operations Management Suite-aviseringar som synkroniserade tooOperations Manager. 

* **Servrar**: Visar hello övervakade servrar som är konfigurerade toosync från Tjänstkarta.

    ![hello övervakning servrar för Operations Manager-fönstret](media/oms-service-map/scom-monitoring-servers.png)

* **Datorn beroende Gruppvyer**: Visar en lista över alla datorgrupper som synkroniseras från Tjänstkartan. Du kan klicka på någon grupp tooview diagram dess distribuerade program.

    ![diagram över hello Operations Manager-distribuerade program](media/oms-service-map/scom-group-dad.png)

* **Servern beroende vyer**: Visar en lista över alla servrar som synkroniseras från Tjänstkartan. Du kan klicka på varje server tooview diagram dess distribuerade program.

    ![diagram över hello Operations Manager-distribuerade program](media/oms-service-map/scom-dad.png)

## <a name="edit-or-delete-hello-workspace"></a>Redigera eller ta bort hello-arbetsytan
Du kan redigera eller ta bort hello konfigurerats arbetsytan via hello **översikt över tjänsten** fönstret (**Administration** fönstret > **Operations Management Suite**  >  **Service Map**). Du kan konfigurera en enda Operations Management Suite-arbetsyta för tillfället.

![hello Operations Manager redigera arbetsyta](media/oms-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Konfigurera regler och åsidosättningar
En regel _Microsoft.SystemCenter.ServiceMapImport.Rule_, skapas tooperiodically hämtning av information från Tjänstkartan. toochange sync tidsinställningar som du kan konfigurera åsidosättningar av hello regel (**redigering** fönstret > **regler** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).

![egenskapsfönstret om hello åsidosättningar i Operations Manager](media/oms-service-map/scom-overrides.png)

* **Aktiverad**: aktivera eller inaktivera automatiska uppdateringar. 
* **IntervalMinutes**: återställa hello tiden mellan uppdateringar. hello Standardintervallet är en timme. Om du vill toosync server maps oftare, kan du ändra hello-värdet.
* **TimeoutSeconds**: återställa hello lång tid innan tidsgränsen uppnås för hello-begäran. 
* **TimeWindowMinutes**: Återställ hello tidsfönstret för datafrågor. Standardvärdet är 60 minuter-fönstret. hello största tillåtna värdet av Tjänstkarta är 60 minuter.

## <a name="known-issues-and-limitations"></a>Kända problem och begränsningar

hello aktuella designen visar hello följande problem och begränsningar:
* Du kan bara ansluta tooa enstaka Operations Management Suite-arbetsyta.
* Du kan lägga till servrar toohello tjänstgruppen kartan servrar manuellt via hello **redigering** rutan hello kartor för servrarna synkroniseras inte omedelbart.  De kommer att synkroniseras från Tjänstkarta hello nästa synkronisering cykel.
* Om du gör några ändringar toohello distribuerade program diagram som har skapats av hello hanteringspaket ändringarna sannolikt att skrivas över på hello nästa synkronisering med Tjänstkartan.

## <a name="create-a-service-principal"></a>Skapa ett huvudnamn för tjänsten
Officiell Azure-dokumentation om hur du skapar en tjänst huvudnamn finns:
* [Skapa ett huvudnamn för tjänsten med hjälp av PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Skapa ett huvudnamn för tjänsten med hjälp av Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Skapa ett huvudnamn för tjänsten med hjälp av hello Azure-portalen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Feedback
Du har feedback för oss om Tjänstkarta eller den här dokumentationen? Besök vår [User Voice sidan](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), där du kan föreslår funktioner eller rösta på förslag befintliga.
