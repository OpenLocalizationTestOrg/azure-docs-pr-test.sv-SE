---
title: "Konfiguration och hantering av problem för Microsoft Azure Cloud Services FAQ | Microsoft Docs"
description: "Den här artikeln innehåller vanliga frågor om konfiguration och hantering av Microsoft Azure Cloud Services."
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
ms.openlocfilehash: 42b5d2947df92b4486fe149d046168208083dde2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Konfiguration och hantering av problem för Azure Cloud Services: vanliga frågor (FAQ)

Den här artikeln innehåller vanliga frågor och svar om konfiguration och hantering av problem för [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Du kan också kontakta den [Cloud Services VM-storlek sidan](cloud-services-sizes-specs.md) storlek information.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="how-do-i-add-nosniff-to-my-website"></a>Hur lägger jag till ”nosniff” till min webbplats?
För att hindra klienter från identifiering MIME-typer, lägger du till en inställning i din *web.config* fil.

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

Du kan också lägga till detta som en inställning i IIS. Använd följande kommando med den [vanliga uppgifter för Start](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

## <a name="how-do-i-customize-iis-for-a-web-role"></a>Hur anpassar IIS för en webbroll?
Använd IIS startskriptet från den [vanliga uppgifter för Start](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.

## <a name="i-cannot-scale-beyond-x-instances"></a>Det går inte att skala upp X instanser
Azure-prenumerationen har en gräns på antal kärnor som du kan använda. Skalning fungerar inte om du har använt alla tillgängliga kärnor. Till exempel om du har en gräns på 100 kärnor, innebär det du kan ha 100 A1 storlek virtuella instanser för din molntjänst eller 50 A2 storlek instanser för virtuella datorer.

## <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Hur kan jag Implementera rollbaserad åtkomst för molntjänster?
Cloud Services stöder inte rollbaserad åtkomstkontroll (RBAC)-modellen eftersom den inte är en Azure Resource Manager-baserad tjänst.

Se [Azure RBAC kontra klassiska prenumerationsadministratörer](../active-directory/role-based-access-control-what-is.md#azure-rbac-vs-classic-subscription-administrators).

## <a name="how-do-i-set-the-idle-timeout-for-azure-load-balancer"></a>Hur ställer jag in timeout vid inaktivitet för Azure belastningsutjämnare
Du kan ange timeout-värdet i din (csdef) tjänstdefinitionsfilen så här:

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

## <a name="can-microsoft-internal-engineers-rdp-to-cloud-service-instances-without-permission"></a>Kan Microsoft interna engineers RDP till molnet tjänstinstanser utan behörighet?
Microsoft följer en strikt process som inte tillåter internt engineers till RDP till Molntjänsten utan skriftligt tillstånd (e-post eller andra skriftligt meddelande) från ägare eller deras utsedda.

## <a name="why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Varför är certifikatkedjan för Mina moln tjänsten SSL-certifikat som är ofullständig?
Vi rekommenderar att kunder installera hela certifikatkedjan (löv certifikat, mellanliggande certifikat och rot-certifikat) i stället för bara lövcertifikatet. När du installerar bara lövcertifikatet du förlita dig på Windows för att skapa certifikatkedjan genom att gå listan över betrodda certifikat. Återkommande nätverks- eller DNS-problem inträffar i Azure eller Windows Update när Windows försöker verifiera certifikatet anses certifikatet ogiltiga. Genom att installera hela certifikatkedjan kan undvikas problemet. Blogg på [hur du installerar en länkad SSL-certifikatet](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) visar hur du gör detta.

## <a name="how-do-i-associate-a-static-ip-address-to-my-cloud-service"></a>Hur associera en statisk IP-adress till min Molntjänsten?
Om du vill konfigurera en statisk IP-adress, måste du skapa en reserverad IP. Den här reserverade IP: N kan vara kopplad till en ny molntjänst eller till en befintlig distribution. Finns i följande dokument för ytterligare information:
* [Så här skapar du en reserverad IP-adress](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Reserverad IP-adressen för en befintlig molntjänst](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Associera en reserverad IP-adress till en ny molntjänst](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Associera en reserverad IP-adress till en aktiv distribution](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Associera en reserverad IP-adress till en molntjänst med hjälp av en konfigurationsfil för tjänsten](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

## <a name="what-is-the-quota-limit-for-my-cloud-service"></a>Vad är kvotgräns för tjänst i molnet?
Se [tjänstspecifika begränsar](../azure-subscription-service-limits.md#subscription-limits).

## <a name="why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Varför visas enheten på min molntjänst för virtuell dator mycket lite ledigt diskutrymme?
Detta är förväntat och den bör inte orsaka eventuella problem i tillämpningsprogrammet. Journalnivåer är aktiverat för % uproot % enheten i Azure PaaS virtuella datorer, vilket i praktiken innebär att dubbla mängden utrymme som filer tar normalt upp. Men det finns flera saker du bör känna till som i stort sett göra detta till ett icke-problem.

Storlek % approot % enhet beräknas som < storleken på .cspkg + max journalstorleken > + en marginal ledigt utrymme eller 1,5 GB, beroende på vilket som är större. Storleken på den virtuella datorn påverkar inte den här beräkningen. (VM-storlek påverkar endast storleken på den tillfälliga C:-enheten.) 

Den stöds inte för att skriva till % approot % enhet. Om du skriver till Azure VM, måste du göra det i en tillfällig LocalStorage resurs (eller andra alternativ, till exempel Blob storage, Azure-filer, osv.). Mängden ledigt utrymme på mappen % approot % är därför inte beskrivande. Om du inte är säker på om ditt program skrivs till % approot % enheten kan du alltid låta tjänsten kör för några dagar och jämför den ”före” och ”efter” storlekar. 

Azure kommer inte att skriva något % approot % enheten. När den virtuella Hårddisken skapas från din .cspkg och monterat på Azure VM, är det enda som kan skriva till den här enheten ditt program. 

Ändringsjournalen inställningarna är icke konfigurerbara, så du inte kan inaktivera den.

## <a name="what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Vad är funktioner och möjligheter som ger Azure basic IP-adresser/ID: N och DDOS?
Azure har IP-adresser/ID: N i datacenter fysiska servrar att skydda mot hot. Dessutom kan kan kunderna distribuera säkerhetslösningar som från tredje part, till exempel web application brandväggar, nätverkets brandväggar, program mot skadlig kod, intrångsidentifiering, förebyggande system (ID: N/IP-adresser) och mer. Mer information finns i [skydda dina data och tillgångar och uppfylla globala säkerhetskrav](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft övervakar kontinuerligt servrar, nätverk och program för att identifiera hot. Azures intrångsidentifiering multipronged hothantering metoden använder, distribuerade denial of service (DDoS) mot attacker, intrång tester, beteendebaserade analytics, avvikelseidentifiering och maskininlärning att ständigt förbättra sin skydd och minska riskerna. Microsoft Antimalware för Azure skyddar Azure-molntjänster och virtuella datorer. Du har möjlighet att distribuera säkerhetslösningar från tredje part dessutom till exempel brandväggar för web application, nätverkets brandväggar, program mot skadlig kod, intrång upptäcka och förhindra system (ID: N/IP-adresser) och mycket mer.

## <a name="why-does-iis-stop-writing-to-the-log-directory"></a>Varför sluta skriva till loggkatalogen i IIS?
Du har uttömt alla lokala lagringskvoten för att skriva till loggkatalogen. Om du vill korrigera detta, kan du göra något av tre saker:
* Aktivera diagnostik för IIS och diagnostiken flyttats regelbundet till blob storage.
* Ta manuellt bort loggfilerna från loggningskatalog.
* Öka kvotgränsen för lokala resurser.

Mer information finns i följande dokument:
* [Lagra och visa diagnostikdata i Azure Storage](cloud-services-dotnet-diagnostics-storage.md)
* [IIS-loggar slutar skriva i Molntjänsten](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

## <a name="what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions"></a>Vad är syftet med ”Windows Azure verktyg kryptering för certifikattillägg”?
Dessa certifikat skapas automatiskt när ett tillägg läggs till i Molntjänsten. Oftast filnamnstillägget BOMULLSTUSS eller RDP-tillägget, men kan det bero på andra, till exempel program mot skadlig kod eller Logginsamlaren tillägget. Dessa certifikat används bara för att kryptera och dekryptera den privata konfigureringen för tillägget. Utgångsdatumet kontrolleras aldrig, så det ingen spelar roll om certifikatet har upphört att gälla. 

Du kan ignorera dessa certifikat. Om du vill rensa certifikat som kan du försöka ta bort alla. Azure genereras ett fel om du försöker ta bort ett certifikat som används.

## <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance"></a>Hur kan jag skapa en begäran om certifikatsignering (CSR) utan ”RDP-ing” i till instansen?
Se följande Riktlinjedokument:

>[Hämta ett certifikat för användning med Windows Azure webbplatser (WAWS)](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

Observera att en CSR är bara en textfil. Det behöver inte skapas från datorn där certifikatet slutligen ska användas. Även om det här dokumentet är avsedd för en Apptjänst, CSR-skapa är generisk och gäller även för molntjänster.
