---
title: "hantering och aaaConfiguration problem för Microsoft Azure Cloud Services FAQ | Microsoft Docs"
description: "Den här artikeln innehåller vanliga frågor om konfiguration och hantering av Microsoft Azure Cloud Services hello."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 62ece142ac0ef5d45081cab333375b1a0a8f0ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Konfiguration och hantering av problem för Azure Cloud Services: vanliga frågor (FAQ)

Den här artikeln innehåller vanliga frågor och svar om konfiguration och hantering av problem för [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Du kan också läsa hello [Cloud Services VM-storlek sidan](cloud-services-sizes-specs.md) storlek information.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="how-do-i-add-nosniff-toomy-website"></a>Hur lägger jag till ”nosniff” toomy webbplats?
tooprevent klienter från identifiering hello MIME-typer, lägga till en inställning i din *web.config* fil.

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

Du kan också lägga till detta som en inställning i IIS. Använd hello följande kommando med hello [vanliga uppgifter för Start](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

## <a name="how-do-i-customize-iis-for-a-web-role"></a>Hur anpassar IIS för en webbroll?
Använd hello IIS startskript från hello [vanliga uppgifter för Start](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.

## <a name="i-cannot-scale-beyond-x-instances"></a>Det går inte att skala upp X instanser
Azure-prenumerationen har en gräns på hello antal kärnor som du kan använda. Skalning fungerar inte om du har använt alla hello kärnor som är tillgängliga. Till exempel om du har en gräns på 100 kärnor, innebär det du kan ha 100 A1 storlek virtuella instanser för din molntjänst eller 50 A2 storlek instanser för virtuella datorer.

## <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Hur kan jag Implementera rollbaserad åtkomst för molntjänster?
Cloud Services stöder inte hello rollbaserad åtkomstkontroll (RBAC) modellen eftersom den inte är en Azure Resource Manager-baserad tjänst.

Se [Azure RBAC kontra klassiska prenumerationsadministratörer](../active-directory/role-based-access-control-what-is.md#azure-rbac-vs-classic-subscription-administrators).

## <a name="how-do-i-set-hello-idle-timeout-for-azure-load-balancer"></a>Hur ställer jag in hello timeout för inaktivitet för Azure belastningsutjämnare
Du kan ange hello timeout i din (csdef) tjänstdefinitionsfilen så här:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="mgVS2015Worker" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10100"   idleTimeoutInMinutes="30" />
    </Endpoints>
  </WorkerRole>
```
Se [New: konfigurerbara Idle Timeout för Azure-belastningsutjämnaren](https://azure.microsoft.com/blog/new-configurable-idle-timeout-for-azure-load-balancer/) för mer information.

## <a name="can-microsoft-internal-engineers-rdp-toocloud-service-instances-without-permission"></a>Kan Microsoft interna engineers RDP toocloud tjänstinstanser utan behörighet?
Microsoft följer en strikt process som inte tillåter interna supporttekniker tooRDP till Molntjänsten utan skriftligt tillstånd (e-post eller andra skriftligt meddelande) från hello ägare eller deras utsedda.

## <a name="why-is-hello-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Varför är hello certifikatkedja för Mina moln tjänsten SSL-certifikat som är ofullständig?
Vi rekommenderar att kunder installerar hello fullständig certifikatkedja (löv certifikat, mellanliggande certifikat och rot-certifikat) i stället för bara hello lövcertifikatet. När du installerar bara hello lövcertifikatet du förlita dig på Windows toobuild hello certifikatkedja genom att gå hello listor över betrodda certifikat. Återkommande nätverks- eller DNS-problem inträffar i Azure eller Windows Update när Windows försöker toovalidate hello certifikat kan hello anses ogiltiga. Genom att installera hela certifikatkedjan för hello kan undvikas problemet. Hej blogg på [hur tooinstall en länkad SSL-certifikat](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) visar hur toodo detta.

## <a name="how-do-i-associate-a-static-ip-address-toomy-cloud-service"></a>Hur jag för att koppla en statisk IP-adress toomy tjänst i molnet?
tooset in en statisk IP-adress måste toocreate en reserverad IP. Den här reserverade IP: N kan vara associerade tooa ny tjänst eller tooan befintliga molndistribution. Se hello efter dokument för ytterligare information:
* [Hur toocreate en reserverad IP-adress](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Reservera hello IP-adressen för en befintlig molntjänst](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Associera en reserverad IP-tooa ny molntjänst](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Associera en reserverad IP-tooa kör distribution](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Koppla en reserverad IP-tooa molntjänst genom att använda en konfigurationsfil för tjänsten](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

## <a name="what-is-hello-quota-limit-for-my-cloud-service"></a>Vad är hello kvotgräns för tjänst i molnet?
Se [tjänstspecifika begränsar](../azure-subscription-service-limits.md#subscription-limits).

## <a name="why-does-hello-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Varför visas hello enhet på min molntjänst för virtuell dator mycket lite ledigt diskutrymme?
Detta är förväntat och den bör inte orsaka några problem tooyour program. Journalnivåer är aktiverat för hello % uproot % enheten i Azure PaaS virtuella datorer, vilket i praktiken innebär att dubbla hello mängden utrymme som filer tar normalt upp. Men det finns flera saker toobe medveten om som i stort sett ändra detta till ett icke-problem.

hello storlek för enhet % approot % beräknas som < storleken på .cspkg + max journalstorleken > + en marginal ledigt utrymme eller 1,5 GB, beroende på vilket som är större. hello storleken på den virtuella datorn påverkar inte den här beräkningen. (hello VM-storlek påverkar endast hello storleken på hello tillfälliga C:-enheten.) 

Det är som inte stöds toowrite toohello % approot % enhet. Om du skriver toohello Azure VM, måste du göra det i en tillfällig LocalStorage resurs (eller andra alternativ, till exempel Blob storage, Azure-filer, osv.). Så är hello mängden ledigt utrymme på hello % approot %-mappen inte beskrivande. Om du inte är säker på om ditt program skriver toohello % approot % enhet kan du låta din tjänst kör för några dagar och jämför hello ”före” och ”efter” storlekar alltid. 

Azure kommer inte att skriva något toohello % approot % enhet. När hello VHD skapas från din .cspkg och monterat på hello Azure VM, hello enda som kan skriva toothis enhet är ditt program. 

hello journal inställningar är icke konfigurerbara, så du inte kan inaktivera den.

## <a name="what-are-hello-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Vad är hello funktioner och möjligheter som ger Azure basic IP-adresser/ID: N och DDOS?
Azure har IP-adresser/ID: N i datacenter fysiska servrar toodefend mot hot. Dessutom kan kan kunderna distribuera säkerhetslösningar som från tredje part, till exempel web application brandväggar, nätverkets brandväggar, program mot skadlig kod, intrångsidentifiering, förebyggande system (ID: N/IP-adresser) och mer. Mer information finns i [skydda dina data och tillgångar och uppfylla globala säkerhetskrav](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft övervakar kontinuerligt servrar, nätverk och program toodetect hot. Azures multipronged hot-metoden använder intrångsidentifiering, distribuerade denial-of-service (DDoS) angrepp förebyggande, intrång testning, beteendeanalys, avvikelseidentifiering och maskininlärning tooconstantly stärka dess skydd och minska riskerna. Microsoft Antimalware för Azure skyddar Azure-molntjänster och virtuella datorer. Du har dessutom säkerhetslösningar för hello alternativet toodeploy från tredje part, till exempel brandväggar för web application, nätverkets brandväggar, program mot skadlig kod, intrång upptäcka och förhindra system (ID: N/IP-adresser) och mycket mer.

## <a name="why-does-iis-stop-writing-toohello-log-directory"></a>IIS varför skriva toohello loggkatalogen?
Du har förbrukat hello lokala lagringskvoten för att skriva toohello loggkatalogen. Om du vill korrigera detta, kan du göra något av tre saker:
* Aktivera diagnostik för IIS och hello diagnostik flyttats regelbundet tooblob lagring.
* Ta manuellt bort loggfiler från hello loggning directory.
* Öka kvotgränsen för lokala resurser.

Mer information finns i följande dokument hello:
* [Lagra och visa diagnostikdata i Azure Storage](cloud-services-dotnet-diagnostics-storage.md)
* [IIS-loggar slutar skriva i Molntjänsten](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

## <a name="what-is-hello-purpose-of-hello-windows-azure-tools-encryption-certificate-for-extensions"></a>Vad är hello syftet hello ”Windows Azure verktyg kryptering för certifikattillägg”?
Dessa certifikat skapas automatiskt när ett tillägg läggs toohello Molntjänsten. Oftast detta är hello BOMULLSTUSS tillägg eller hello RDP-tillägget, men det kan vara andra, till exempel hello program mot skadlig kod eller Logginsamlaren. Dessa certifikat används bara för att kryptera och dekryptera hello privata konfiguration för hello tillägg. hello förfallodatum kontrolleras aldrig, så det ingen spelar roll om hello certifikatet har gått ut. 

Du kan ignorera dessa certifikat. Om du vill rensa hello certifikat, kan du ta bort alla. Azure genereras ett fel om du försöker toodelete ett certifikat som används.

## <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-toohello-instance"></a>Hur kan jag skapa en begäran om certifikatsignering (CSR) utan ”RDP-ing” i toohello instans?
Se hello följande riktlinjer:

>[Hämta ett certifikat för användning med Windows Azure webbplatser (WAWS)](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

Observera att en CSR är bara en textfil. Det har inte toobe som skapats från hello dator där hello certifikatet slutligen används. Även om det här dokumentet är avsedd för en Apptjänst, hello CSR skapa är generisk och gäller även för molntjänster.
