---
title: aaaUpgrade ett Azure Service Fabric-kluster | Microsoft Docs
description: "Uppgradera hello Service Fabric-kod och/eller konfiguration som kör ett Service Fabric-kluster, inklusive inställning klustret uppdateringsläge, uppgradera certifikat, lägga till programmet portar, göra operativsystemskorrigeringar, och så vidare. Vad kan du förvänta dig när hello uppgraderingar utförs?"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/10/2017
ms.author: chackdan
ms.openlocfilehash: 94ac3833ec0810f79de06ecb50f254028fa90408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a>Uppgradera en Azure Service Fabric-kluster
> [!div class="op_single_selector"]
> * [Azure-kluster](service-fabric-cluster-upgrade.md)
> * [Fristående kluster](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

För alla moderna system är designa för möjligheterna viktiga tooachieving långsiktig framgång för en produkt. Ett Azure Service Fabric-kluster är en resurs som du äger, men delvis hanteras av Microsoft. Den här artikeln beskriver vad hanteras automatiskt och vad du kan konfigurera själv.

## <a name="controlling-hello-fabric-version-that-runs-on-your-cluster"></a>Kontrollera hello fabric-versionen som körs på klustret
Du kan ange ditt kluster tooreceive automatisk fabric uppgraderingar som Microsoft släpper eller så kan du välja en stöds fabric-versionen som du vill använda ditt kluster toobe på.

Det gör du genom inställningen hello ”upgradeMode” klusterkonfigurationen på hello-portalen eller med hjälp av Resource Manager för närvarande hello skapas eller senare på ett aktivt kluster 

> [!NOTE]
> Se till att tookeep ditt kluster som kör en version stöds fabric alltid. Och när vi meddela hello-versionen av en ny version av service fabric, har hello tidigare version markerats för slutet av stödet efter minst 60 dagar från det datumet. hello hello nya versioner tillkännages [på hello service fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric/). hello nya versionen är sedan tillgängliga toochoose. 
> 
> 

14 dagar tidigare toohello utgången av hello versionen klustret körs, skapas en hälsohändelse som placerar klustret i ett varningshälsotillstånd. hello klustret förblir i ett varningstillstånd förrän du har uppgraderat tooa stöds fabric-versionen.

### <a name="setting-hello-upgrade-mode-via-portal"></a>Inställningen hello Uppgraderingsläge via portalen
Du kan ange hello klustret tooautomatic eller manuellt när du skapar hello klustret.

![Create_Manualmode][Create_Manualmode]

Du kan ange hello klustret tooautomatic eller manuell på ett aktivt kluster, med hjälp av hello hantera upplevelse. 

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-portal"></a>Uppgradera tooa nya versionen på ett kluster som har angetts tooManual läge via portalen.
tooupgrade tooa ny version, allt du behöver toodo är Välj hello tillgängliga versionen hello listrutan och spara. hello Fabric-uppgraderingen hämtar inletts automatiskt. hello klustret hälsoprinciper (en kombination av noden hälso- och hello alla hello program som körs i klustret hello) följs tooduring hello uppgraderingen.

Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda. Rulla ned det här dokumentet tooread mer om hur tooset dessa anpassade hälsoprinciper. 

När du har åtgärdat hello problem som resulterade i hello återställning måste tooinitiate hello uppgraderingen igen, med följande hello samma steg som tidigare.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-hello-upgrade-mode-via-a-resource-manager-template"></a>Inställningen hello Uppgraderingsläge via en Resource Manager-mall
Lägg till hello ”upgradeMode” configuration toohello Microsoft.ServiceFabric/clusters resursdefinitionen och ange hello ”clusterCodeVersion” tooone av hello fabric-versioner som stöds som visas nedan och sedan distribuera hello mallen. hello giltiga värden för ”upgradeMode” är ”manuell” eller ”automatisk”

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-a-resource-manager-template"></a>Uppgradera tooa nya versionen på ett kluster som har angetts tooManual läge via en Resource Manager-mall.
När hello klustret är i manuellt läge måste tooupgrade tooa ny version, ändra hello ”clusterCodeVersion” tooa-version som stöds och distribuerar den. hello distribution av hello mallen, sparkar av hello Fabric-uppgraderingen hämtar inletts automatiskt. hello klustret hälsoprinciper (en kombination av noden hälso- och hello alla hello program som körs i klustret hello) följs tooduring hello uppgraderingen.

Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda. Rulla ned det här dokumentet tooread mer om hur tooset dessa anpassade hälsoprinciper. 

När du har åtgärdat hello problem som resulterade i hello återställning måste tooinitiate hello uppgraderingen igen, med följande hello samma steg som tidigare.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Hämta en lista över alla tillgängliga versionen för alla miljöer för en viss prenumeration
Kör hello följande kommando och du bör få en liknande toothis utdata.

”supportExpiryUtc” talar om när en viss version upphör att gälla eller har upphört att gälla. hello senaste versionen har inte ett giltigt datum - har ett värde på ”9999-12-31T23:59:59.9999999”, vilket innebär bara att hello förfallodatum inte ännu har angetts.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2016-09-01

Example: https://management.azure.com/subscriptions/1857f442-3bce-4b96-ad95-627f76437a67/providers/Microsoft.ServiceFabric/locations/eastus/clusterVersions?api-version=2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-hello-cluster-upgrade-mode-is-automatic"></a>Fabric-uppgraderingar när hello klustret uppgradera läge är automatisk
Microsoft har hello fabric koden och konfigurationen som körs i ett Azure-kluster. Vi utföra automatiska övervakade uppgraderingar toohello programvara som det behövs. Uppgraderingarna kan vara koden, konfiguration, eller båda. toomake till att ditt program lider ingen påverkan eller minimal påverkan på grund av toothese uppgraderingar, vi utföra hello uppgraderingar i hello följande faser:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Fas 1: En uppgradering utförs med hjälp av alla hälsoprinciper för kluster
Under den här fasen hello uppgraderingar fortsätter en domän i taget och hello-program som kördes i hello kluster fortsätter toorun utan driftavbrott. hello klustret hälsoprinciper (en kombination av noden hälso- och hello alla hello program som körs i klustret hello) följs tooduring hello uppgraderingen.

Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda. Ett e-postmeddelande skickas sedan toohello ägare hello prenumeration. hello e-post innehåller hello följande information:

* Meddelande om att vi hade tooroll tillbaka en uppgradering av klustret.
* Föreslagna vidtar åtgärder, om sådana finns.
* hello antal dagar (n) förrän vi köra fas 2.

Vi försök tooexecute hello samma uppgradera några gånger om eventuella uppgraderingar misslyckades av orsaker som infrastruktur. Efter hello n dagar från hello datum hello e-postmeddelande har skickats, vi fortsätta tooPhase 2.

Om hello klustret hälsoprinciper uppfylls som genomförd hello uppgradering och markerat som slutförda. Detta kan inträffa under hello inledande uppgradering eller någon av hello uppgradera repriser i det här steget. Det finns ingen e-bekräftelse av en lyckad körning. Detta är tooavoid skickar du för många e-postmeddelanden--en e-postmeddelanden ska ses som ett undantag toonormal. De flesta av hello klustret uppgraderingar toosucceed planerar vi för utan att påverka programmets tillgänglighet.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Fas 2: En uppgradering utförs med hjälp av standard hälsoprinciper endast
hello hälsoprinciper i det här steget är inställda så att hello antalet program som har felfri hello början av hello uppgraderingen förblir hello samma för hello varaktighet hello uppgraderingsprocessen. Som i fas 1 hello fas 2 uppgraderingar fortsätter en domän i taget och hello program som kördes i hello kluster fortsätter toorun utan driftavbrott. hello klustret hälsoprinciper (en kombination av noden hälso- och hello alla hello program som körs i klustret hello) är följt toofor hello varaktighet hello uppgraderingen.

Hello uppgraderingen återställs om hello klustret hälsoprinciper gäller inte uppfylls. Ett e-postmeddelande skickas sedan toohello ägare hello prenumeration. hello e-post innehåller hello följande information:

* Meddelande om att vi hade tooroll tillbaka en uppgradering av klustret.
* Föreslagna vidtar åtgärder, om sådana finns.
* hello antal dagar (n) förrän vi köra fas 3.

Vi försök tooexecute hello samma uppgradera några gånger om eventuella uppgraderingar misslyckades av orsaker som infrastruktur. En påminnelse om e-postmeddelande skickas ett par dagar innan n dagar är igång. Efter hello n dagar från hello datum hello e-postmeddelande har skickats, vi fortsätta tooPhase 3. hello e-postmeddelanden som vi skickar dig i fas 2 måste du vidta allvarligt och vidtar åtgärder vidtas.

Om hello klustret hälsoprinciper uppfylls som genomförd hello uppgradering och markerat som slutförda. Detta kan inträffa under hello inledande uppgradering eller någon av hello uppgradera repriser i det här steget. Det finns ingen e-bekräftelse av en lyckad körning.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Fas 3: En uppgradering utförs med hjälp av aggressivt hälsoprinciper
Dessa hälsoprinciper i det här steget är inriktad på hello uppgraderingen har slutförts i stället för hello hälsotillståndet hos hello-program. Mycket få klusteruppgradering hamna i det här steget. Om klustret hämtar toothis fasen, är det troligt att programmet blir ohälsosamt och förlora tillgänglighet.

Liknande toohello andra två faser, fas 3 uppgraderingar fortsätta en domän i taget.

Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda. Vi försök tooexecute hello samma uppgradera några gånger om eventuella uppgraderingar misslyckades av orsaker som infrastruktur. Efter det fästs hello klustret, så att den får inte längre stöd och/eller uppgraderingar.

Ett e-postmeddelande med informationen skickas toohello prenumerationsägaren tillsammans med hello vidtar åtgärder. Vi förväntar sig inte några kluster tooget i ett tillstånd där fas 3 har misslyckats.

Om hello klustret hälsoprinciper uppfylls som genomförd hello uppgradering och markerat som slutförda. Detta kan inträffa under hello inledande uppgradering eller någon av hello uppgradera repriser i det här steget. Det finns ingen e-bekräftelse av en lyckad körning.

## <a name="cluster-configurations-that-you-control"></a>Klusterkonfigurationer som du hanterar
Dessutom toohello möjlighet tooset hello klustret Uppgraderingsläge, här hello-konfigurationer som du kan ändra på ett aktivt kluster.

### <a name="certificates"></a>Certifikat
Du kan lägga till nya eller ta bort certifikat för hello kluster och klienten via hello portal enkelt. Se för[det här dokumentet för detaljerade anvisningar](service-fabric-cluster-security-update-certs-azure.md)

![Skärmbild som visar certifikattumavtryck i hello Azure-portalen.][CertificateUpgrade]

### <a name="application-ports"></a>Programmet portar
Du kan ändra program portar genom att ändra hello belastningsutjämnaren resursegenskaper som är associerade med hello nodtypen. Du kan använda hello portalen eller använda Resource Manager PowerShell direkt.

tooopen en ny port på alla virtuella datorer i en nodtyp hello följande:

1. Lägg till en ny avsökning toohello lämplig belastningsutjämnare.
   
    Om du har distribuerat ditt kluster med hjälp av hello portal hello belastningsutjämnare namnges ”LB-namnet på hello Resource group-NodeTypename”, en för varje nodtyp. Eftersom hello belastningen belastningsutjämnaren namnen är unika endast inom en resursgrupp, är det bäst om du söker efter dem under en viss resursgrupp.
   
    ![Skärmbild som visar hur du lägger till en avsökning tooa belastningsutjämnare i hello-portalen.][AddingProbes]
2. Lägga till en ny regel toohello belastningsutjämnare.
   
    Lägg till en ny regel toohello samma belastningsutjämning med hjälp av hello-avsökningen som du skapade i föregående steg i hello.
   
    ![Lägger till en ny regel tooa belastningsutjämnare i hello-portalen.][AddingLBRules]

### <a name="placement-properties"></a>Placeringsegenskaper
För varje hello nodtyper, kan du lägga till anpassade placeringsegenskaper som du vill toouse i dina program. NodeType är en standardegenskap som du kan använda utan att lägga till det explicit.

> [!NOTE]
> Mer information om hello användning av placeringen nodegenskaper, och hur toodefine dem, finns toohello ”begränsningar och noden placeringsegenskaper” i avsnittet hello resurshanteraren för Service Fabric-klustret dokumentet på [som beskriver ditt kluster ](service-fabric-cluster-resource-manager-cluster-description.md).
> 
> 

### <a name="capacity-metrics"></a>Kapacitet mått
Du kan lägga till anpassade kapacitetsmått som du vill toouse i ditt program tooreport belastning för varje hello nodtyper. Mer information om hello användning av kapacitet mått tooreport finns toohello resurshanteraren för Service Fabric-klustret dokument på [som beskriver ditt kluster](service-fabric-cluster-resource-manager-cluster-description.md) och [mått och Läs in](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Inställningar för fabric - hälsoprinciper
Du kan ange anpassade hälsoprinciper för fabric-uppgraderingen. Om du har angett ditt kluster tooAutomatic fabric uppgraderingar, hämta tillämpade toohello fas 1 hello automatisk fabric uppgraderas med dessa principer.
Om du har angett ditt kluster för manuell fabric uppgraderingar, och dessa principer tillämpas varje gång väljer du en ny version utlösa hello system tookick av hello fabric-uppgraderingen i klustret. Om du inte åsidosätter hello principer används hello standardinställningar.

Du kan ange hello anpassade hälsoprinciper eller granska hello aktuella inställningar under hello ”fabric-uppgraderingen” bladet genom att välja hello avancerade inställningar för uppgradering. Granska hello följande bild på hur du. 

![Hantera anpassade hälsoprinciper][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Anpassa infrastrukturinställningarna för klustret
Se för[service fabric-kluster infrastrukturinställningarna](service-fabric-cluster-fabric-settings.md) på vad och hur du kan anpassa dem.

### <a name="os-patches-on-hello-vms-that-make-up-hello-cluster"></a>Operativsystemskorrigeringar på hello virtuella datorer som utgör hello kluster
Se för[korrigering Orchestration programmet](service-fabric-patch-orchestration-application.md) som kan distribueras på ditt kluster tooinstall korrigeringsfiler från Windows Update på ett framförhållning sätt att hålla hello-tjänster som är tillgängliga alla hello tid. 

### <a name="os-upgrades-on-hello-vms-that-make-up-hello-cluster"></a>OS-uppgraderingar på hello virtuella datorer som utgör hello kluster
Om du måste uppgradera hello OS-avbildningen på hello virtuella datorer i hello kluster, måste du göra det en virtuell dator i taget. Du ansvarar för den här uppgraderingen – det finns för närvarande ingen automatisering för det här.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur toocustomize vissa hello [tjänstinställningar fabric-kluster fabric](service-fabric-cluster-fabric-settings.md)
* Lär dig hur för[skala ditt kluster in och ut](service-fabric-cluster-scale-up-down.md)
* Lär dig mer om [programuppgraderingar](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
