---
title: "aaaAzure IoT Suite vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor och svar om IoT Suite"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: cb39e24af6d1ce2afea554285512d05b2d7c721e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a>Vanliga frågor och svar om IoT Suite

Se även hello anslutna factory specifika [vanliga frågor och svar](iot-suite-faq-cf.md).

### <a name="where-can-i-find-hello-source-code-for-hello-preconfigured-solutions"></a>Var hittar jag hello källkoden för hello förkonfigurerade lösningar?

hello källkoden lagras i följande GitHub-lagringsplatser hello:
* [Fjärrövervaknings förkonfigurerade lösningen][lnk-remote-monitoring-github]
* [Förutsägande Underhåll förkonfigurerade lösningen][lnk-predictive-maintenance-github]
* [Anslutna factory förkonfigurerade lösningen](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-toohello-latest-version-of-hello-remote-monitoring-preconfigured-solution-that-uses-hello-iot-hub-device-management-features"></a>Hur uppdaterar toohello senaste versionen av hello remote förkonfigurerade övervakningslösning som använder hello IoT-hubb funktioner för enhetshantering?

* Om du distribuerar en förkonfigurerade lösning från hello https://www.azureiotsuite.com/ plats distribuerar alltid en ny instans av hello senaste versionen av hello lösning.
* Om du distribuerar en förkonfigurerade lösning hello kommandoraden måste uppdatera du en befintlig distribution med ny kod. Se [Cloud deployment] [ lnk-cloud-deployment] i hello GitHub [databasen][lnk-remote-monitoring-github].

### <a name="how-can-i-add-support-for-a-new-device-method-toohello-remote-monitoring-preconfigured-solution"></a>Hur kan jag lägga till stöd för en ny enhet metoden toohello remote förkonfigurerade övervakningslösning?

Avsnittet hello [lägga till stöd för en ny metod toohello simulator] [ lnk-add-method] i hello [anpassa förkonfigurerade lösning] [ lnk-customize] artikel.

### <a name="hello-simulated-device-is-ignoring-my-desired-property-changes-why"></a>hello simulerade enheten ignorerar min önskade egenskapsändringar varför?
Hej fjärrövervaknings förkonfigurerade lösningen, hello simulerade enheten koden använder endast hello **Desired.Config.TemperatureMeanValue** och **Desired.Config.TelemetryInterval** önskade egenskaper tooupdate hello rapporterade egenskaper. Alla andra ändringsbegäranden för önskad egenskapen ignoreras.

### <a name="my-device-does-not-appear-in-hello-list-of-devices-in-hello-solution-dashboard-why"></a>Min enhet visas inte i hello lista över enheter i instrumentpanelen för hello lösning varför?

hello lista över enheter i instrumentpanelen för hello lösningen använder en fråga tooreturn hello lista över enheter. För närvarande kan inte en fråga returnera fler än 10K-enheter. Försök att göra hello sökvillkor för frågan mer restriktiva.

### <a name="whats-hello-difference-between-deleting-a-resource-group-in-hello-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Vad är hello skillnaden mellan att ta bort en resursgrupp i Azure portal och klicka på Ta bort på en förkonfigurerade lösning i azureiotsuite.com hello?

* Om du tar bort hello förkonfigurerade lösning i [azureiotsuite.com][lnk-azureiotsuite], du ta bort alla hello-resurser som etablerades när du skapade hello förkonfigurerade lösningen. Om du har lagt till fler resurser toohello resursgruppen raderas även dessa resurser. 
* Om du tar bort hello resursgrupp i hello [Azure-portalen][lnk-azure-portal], bara ta bort hello resurser i resursgruppen. Du måste också toodelete hello Azure Active Directory-program som är associerade med hello förkonfigurerade lösning i hello [klassiska Azure-portalen][lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Hur många IoT Hub-instanser kan etablera i en prenumeration?

Som standard kan du etablera [10 IoT-hubbar per prenumeration][link-azuresublimits]. Du kan skapa en [Azure supportärende] [ link-azuresupportticket] tooraise den här gränsen. Därför kan kan sedan varje förkonfigurerade lösningen etablerar en ny IoT-hubb endast etablera in too10 förkonfigurerade lösningar i en viss prenumeration. 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Hur många Azure DB som Cosmos-instanser kan etablera i en prenumeration?

Femtio. Du kan skapa en [Azure supportärende] [ link-azuresupportticket] tooraise detta begränsa, men som standard kan du bara etablera 50 Cosmos DB instanser per prenumeration. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hur många kostnadsfria Bing Maps-API:er kan jag etablera i en prenumeration?

Två. Du kan skapa två interna transaktioner nivå 1 Bing Maps för Enterprise planer i en Azure-prenumeration. hello remote övervakningslösning etableras som standard med hello interna transaktioner nivå 1-plan. Därför kan du bara etablera tootwo fjärråtkomst övervakning lösningar i en prenumeration utan ändringar.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Jag har en distribution av en fjärrövervakningslösning med en statisk karta. Hur lägger jag till en interaktiv Bing-karta?

1. Hämta Bing Maps API för Enterprise QueryKey från [Azure-portalen][lnk-azure-portal]: 
   
   1. Navigera toohello resursgruppen där dina Bing Maps API för företag är i hello [Azure-portalen][lnk-azure-portal].
   2. Klicka på **alla inställningar**, sedan **nyckelhantering**. 
   3. Du kan se två nycklar: **MasterKey** och **QueryKey**. Kopiera hello värde för **QueryKey**.
      
      > [!NOTE]
      > Har du inget Bing Maps-API för Enterprise-konto? Skapa en i hello [Azure-portalen] [ lnk-azure-portal] genom att klicka på + ny söker efter Bing Maps API för företag och följ uppmanar toocreate.
      > 
      > 
2. Hämtar hello senaste kod från hello [Azure IoT-fjärr-övervakning][lnk-remote-monitoring-github].
3. Kör en lokal eller cloud deployment följande hello kommandoradsverktyget distributionsanvisningar i hello /docs/ mappen i hello-databasen. 
4. När du har kört en lokal eller molnet distribution, titta på din rotmapp för hello *. user.config-filen som skapades under distributionen. Öppna den här filen i en textredigerare. 
5. Ändra hello följande rad tooinclude hello-värde som du kopierade från din **QueryKey**: 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Kan jag skapa en förkonfigurerad lösning om jag har Microsoft Azure för DreamSpark?

För närvarande kan du inte skapa en förkonfigurerade lösning med en [Microsoft Azure för DreamSpark] [ lnk-dreamspark] konto. Du kan dock skapa en [kostnadsfri utvärderingsversion konto för Azure] [ lnk-30daytrial] på ett par minuter som du kan skapa en förkonfigurerade lösning.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Kan jag skapa en förkonfigurerade lösning om jag har Cloud Solution Providers (CSP)-prenumeration?

För närvarande kan skapa du inte en förkonfigurerade lösning med en prenumeration Cloud Solution Providers (CSP). Du kan dock skapa en [kostnadsfri utvärderingsversion konto för Azure] [ lnk-30daytrial] på ett par minuter som du kan skapa en förkonfigurerade lösning.

### <a name="how-do-i-delete-an-aad-tenant"></a>Hur tar jag bort en AAD-klient?

Finns i blogginlägget Eric Golpe [genomgång av du tar bort en Azure AD-klient][lnk-delete-aad-tennant].

### <a name="next-steps"></a>Nästa steg

Du kan även utforska några av hello andra funktioner och förmågor i hello IoT Suite förkonfigurerade lösningar:

* [Förutsägande Underhåll förkonfigurerade lösning: översikt][lnk-predictive-overview]
* [Anslutna factory förkonfigurerade lösning: översikt](iot-suite-connected-factory-overview.md)
* [IoT-säkerhet från hello bakgrund][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
