---
title: aaaAzure ordlista - Azure ordlistan | Microsoft Docs
description: "Använda hello Azure ordlista toounderstand moln terminologi på hello Azure-plattformen. Den här korta Azure ordlistan innehåller definitioner för vanliga molnet villkoren för Azure."
keywords: Azure ordlista, molnet terminologi, Azure ordlista, termer, molnet villkor
services: na
documentationcenter: na
author: MonicaRush
manager: jhubbard
editor: 
ms.assetid: d7ac12f7-24b5-4bcd-9e4d-3d76fbd8d297
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: monicar
ms.openlocfilehash: 486bbbfc71a48a6ebc39b14f7ab71f8604b7a6b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-hello-azure-platform"></a>Ordlista för Microsoft Azure: en ordlista med molnet terminologin hello Azure-plattformen

hello ordlista för Microsoft Azure är en kort dictionary med molnet terminologi för hello Azure-plattformen. Se även:

* [Microsoft Azure och Amazon Web Services](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) -definitioner av Azure-tjänster och motsvarigheterna AWS.<!-- I propose toolink toohttps://azure.microsoft.com/en-us/services/ instead of this -->
* [Molnet databehandling villkoren](https://azure.microsoft.com/overview/cloud-computing-dictionary/) -Allmänt branschen molnet villkoren.

## <a name="account"></a>Konto
Ett konto som har använt tooaccess och hantera en Azure-prenumeration. Det har ofta kallas tooas en Azure-konto även om ett konto kan vara något av följande: ett befintligt arbets, skolan, eller personliga Microsoft-konto, eller en Office 365 användarnamn och lösenord. Du kan också skapa ett konto toomanage en Azure-prenumeration när du registrerar dig för hello [kostnadsfri utvärderingsversion](https://azure.microsoft.com).  
Se [registrera dig för en Azure-prenumeration med ditt Office 365-konto](billing/billing-use-existing-office-365-account-azure-subscription.md) och [konton som du kan använda toosign i](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="api-app"></a>API-app
Ett annat namn för [Apptjänst-app](#app-service-app).

## <a name="app-service-app"></a>Apptjänst-app
Hej beräkningsresurser som [Azure App Service](app-service/app-service-value-prop-what-is.md) tillhandahåller för hantering av en [webbplatsen eller programmet](app-service-web/app-service-web-overview.md), [webb-API](app-service-api/app-service-api-apps-why-best-platform.md), eller [mobilappsserverdel](app-service-mobile/app-service-mobile-value-prop.md). Apptjänst-appar är också enligt tooas *Apptjänster*, *webbappar*, *API apps*, och *mobilappar*.

## <a name="availability-set"></a>Tillgänglighetsuppsättning
En samling med virtuella datorer som hanteras tillsammans tooprovide programmet redundans och tillförlitlighet. hello användning av en tillgänglighetsuppsättning garanterar att minst en virtuell dator under en planerad eller oplanerad underhållshändelse är tillgänglig.  
Se [hantera hello tillgängligheten för virtuella Windows-datorer](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och [hantera hello tillgängligheten för virtuella Linux-datorer](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="classic-model"></a>Azure klassiska distributionsmodellen
En av två [distributionsmodeller](resource-manager-deployment-model.md) används toodeploy resurser i Azure (hello ny modell är Azure Resource Manager). Vissa Azure tjänster stöder endast hello Resource Manager-distributionsmodellen, vissa stöder endast hello som klassiska distributionsmodellen och vissa stöder både. hello-dokumentationen för varje Azure-tjänst anger vilka modeller som de stöder.

## <a name="cli"></a>Azure-kommandoradsgränssnittet (CLI)
Ett kommandoradsgränssnitt som kan använda toomanage Azure services från Windows-, macOS- och Linux.  Vissa tjänster eller tjänstens funktioner kan hanteras endast via PowerShell eller hello CLI. Se [Azure CLI 2.0](/cli/azure/overview)

## <a name="powershell"></a>Azure PowerShell
Ett kommandoradsgränssnitt toomanage Azure services via Kommandotolken från Windows-datorer. Vissa tjänster eller tjänstens funktioner kan hanteras endast via PowerShell eller hello CLI.
Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview)

## <a name="arm-model"></a>Azure Resource Manager-distributionsmodellen
En av två [distributionsmodeller](resource-manager-deployment-model.md) används toodeploy resurser i Microsoft Azure (hello andra är hello klassiska distributionsmodellen). Vissa Azure tjänster stöder endast hello Resource Manager-distributionsmodellen, vissa stöder endast hello som klassiska distributionsmodellen och vissa stöder både. hello-dokumentationen för varje Azure-tjänst anger vilka modeller som de stöder.

## <a name="fault-domain"></a>feldomän
Hej samling av virtuella datorer i en tillgänglighetsuppsättning som kan eventuellt inte hello samma tid. Ett exempel är en grupp datorer i ett rack som delar en gemensam käll- och strömbrytare. I Azure separeras automatiskt hello virtuella datorer i en tillgänglighetsuppsättning över flera feldomäner.  
Se [hantera hello tillgängligheten för virtuella Windows-datorer](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [hantera hello tillgängligheten för virtuella Linux-datorer](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)  

## <a name="geo"></a>GEO-replikering
En definierad gräns för data land som innehåller vanligtvis två eller flera regioner. hello gränser kan göras inom eller utanför gränser och påverkas av skatt förordning. Varje geo har minst en region. Asien/Stillahavsområdet och Japan är exempel på regioner. Kallas även *geografi*.  
Se [Azure-regioner](best-practices-availability-paired-regions.md)

## <a name="geo-replication"></a>GEO-replikering
hello processen automatiskt replikera innehåll, till exempel blobbar, tabeller och köer i ett regionalt par.  
Se [aktiv Geo-replikering för Azure SQL-databas](sql-database/sql-database-geo-replication-overview.md)
<!-- hello meaning of "geo" in this term seems toobe different than hello meaning provided in hello "geo" entry -->

## <a name="image"></a>Bild
En fil som innehåller hello operativsystemet och konfiguration som kan använda toocreate valfritt antal virtuella datorer. Det finns två typer av avbildningar i Azure: VM-avbildning och OS-avbildningen. En VM-avbildning innehåller ett operativsystem och alla diskar anslutna tooa virtuella datorn när hello avbildningen skapas. En operativsystemavbildning innehåller endast en generaliserad operativsystem med diskkonfigurationer inga data.  
Se [navigera och välj virtuella Windows-avbildningar i Azure med PowerShell eller hello CLI](virtual-machines/windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="limits"></a>Gränser
hello antal resurser som kan skapas eller hello prestanda prestandamått som kan uppnås. Gränserna är vanligtvis kopplad till prenumerationer, tjänster och erbjudanden.  
Se [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](azure-subscription-service-limits.md)

## <a name="load-balancer"></a>Belastningsutjämnare
En resurs som distribuerar inkommande trafik mellan datorer i ett nätverk. I Azure distribuerar en belastningsutjämnare trafik toovirtual datorer som definierats i en belastningsutjämnare. En [belastningsutjämnare](load-balancer/load-balancer-overview.md) kan vara mot internet, eller det kan vara interna.  

## <a name="mobile-app"></a>mobilapp
Ett annat namn för [Apptjänst-App](#app-service-app).

## <a name="offer"></a>Erbjudande
hello prissättning, krediter och tillhörande villkor gäller tooan Azure-prenumeration.  
Se hello [informationssidan för Azure-erbjudande](https://azure.microsoft.com/support/legal/offer-details/)

## <a name="portal"></a>portal
hello säker webbportalen används toodeploy och hantera Azure-tjänster.  Det finns två portaler: hello [Azure-portalen](http://portal.azure.com/) och hello [klassiska portalen](http://manage.windowsazure.com/). Vissa tjänster finns i båda portaler, medan andra är bara tillgängliga i en eller hello andra. Hej [Azure portal tillgänglighet diagram](https://azure.microsoft.com/features/azure-portal/availability/) listor tjänster som är tillgängliga i vilken portal.

## <a name="region"></a>Region
Ett område i en geo som inte mellan gränser och innehåller en eller flera datacenter. Priser regionala tjänster och erbjudandetyper exponeras på hello regionnivå. En region vanligtvis paras ihop med en annan region, vilket kan vara upp tooseveral hundra mil bort. Regional par kan användas som en mekanism för katastrofåterställning och scenarier med hög tillgänglighet. Kallas även tooas *plats*.  
Se [Azure-regioner](best-practices-availability-paired-regions.md)

## <a name="resource"></a>Resursen
Ett objekt som är en del av din Azure-lösning. Varje Azure-tjänst kan du toodeploy olika typer av resurser, t.ex databaser eller virtuella datorer.   
Se [översikt över Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

## <a name="resource-group"></a>Resursgrupp
En behållare i Resource Manager som innehåller relaterade resurser för ett program. hello resursgrupp kan innehålla alla hello resurser för ett program eller bara de resurser som är logiskt grupperade. Du kan välja du hur tooallocate resurser tooresource grupper baserat på vad som är hello bäst för din organisation.  
Se [översikt över Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

## <a name="arm-template"></a>Resource Manager-mall
En JSON-fil som deklarativt definierar en eller flera Azure-resurser och som definierar beroenden mellan hello distribuerade resurser. hello mallen kan vara används toodeploy hello resurser konsekvent och flera gånger.  
Se [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md)

## <a name="resource-provider"></a>Resursprovidern
En tjänst som tillhandahåller hello resurser som du kan distribuera och hantera via Resource Manager. Varje resursprovider ger åtgärder för att arbeta med hello resurser som distribueras. Resursproviders kan nås via hello Azure-portalen, Azure PowerShell och flera programmeringsspråk SDK.  
Se [översikt över Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

## <a name="role"></a>Rollen
Ett sätt för att styra åtkomsten som kan tilldelas toousers, grupper och tjänster. Roller är kan tooperform åtgärder som att skapa, hantera, och läsas på Azure-resurser.  
Se [RBAC: inbyggda roller](active-directory/role-based-access-built-in-roles.md)

## <a name="sla"></a>servicenivåavtal (SLA)
hello avtal som beskriver Microsofts åtaganden för drifttid och anslutningar. Varje Azure-tjänsten har en specifik SLA.  
Se [serviceavtal](https://azure.microsoft.com/support/legal/sla/)

## <a name="sas"></a>signatur för delad åtkomst (SAS)
En signatur som gör att du toogrant begränsad åtkomst tooa resurs, utan att utsätta din kontonyckel. Till exempel [Azure Storage använder SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) toogrant klienten åtkomst tooobjects, till exempel BLOB. [IoT-hubb använder SAS](iot-hub/iot-hub-devguide-security.md#security-tokens) toogrant enheter behörighet toosend telemetri.

## <a name="storage-account"></a>Storage-konto
Ett konto som ger dig åtkomst till toohello Azure Blob, kön, tabell, och Filtjänster i Azure Storage. Hej lagringskontonamnet definierar hello unikt namnområde för Azure Storage-dataobjekt.  
Se [om Azure storage-konton](storage/common/storage-create-storage-account.md)

## <a name="subscription"></a>prenumeration
Kundens avtal med Microsoft som gör dem tooobtain Azure services. hello prenumeration priser och tillhörande villkor regleras av hello erbjudande valt för hello prenumeration.
Se [prenumerationsavtalet för Microsoft Online](https://azure.microsoft.com/support/legal/subscription-agreement/) och [hur Azure-prenumerationer är associerade med Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md)

## <a name="tag"></a>Taggen
En indexering term som gör att du toocategorize resurser enligt tooyour krav för att hantera och fakturering. När du har en komplex samling resurser som kan du använda taggar toovisualize tillgångarna hello sätt som passar hello. Du kan till exempel markera resurser som har en liknande roll i organisationen eller toohello tillhör samma avdelning.  
Se [med hjälp av taggar tooorganize Azure-resurser](resource-group-using-tags.md)

## <a name="update-domain"></a>Uppdatera domänen
Hej samling av virtuella datorer i en tillgänglighetsuppsättning som uppdateras med hello samtidigt. Virtuella datorer i hello samma uppdateringsdomän startas om tillsammans under planerat underhåll. Azure startar aldrig om mer än en uppdateringsdomän i taget. Kallas också tooas en uppgraderingsdomän.  
Se [hantera hello tillgängligheten för virtuella Windows-datorer](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och [hantera hello tillgängligheten för virtuella Linux-datorer](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vm"></a>virtuell dator
hello programvara implementering av en fysisk dator som kör ett operativsystem. Flera virtuella datorer kan köras samtidigt på hello samma maskinvara. I Azure är virtuella datorer tillgängliga i olika storlekar.  
Se [dokumentation för virtuella datorer](https://azure.microsoft.com/documentation/services/virtual-machines/)

## <a name="vm-extension"></a>tillägg för virtuell dator
En resurs som implementerar beteenden eller funktioner som antingen hjälper andra program fungerar eller ange hello möjligheten för toointeract med en dator som körs. Du kan exempelvis använda hello VM åtkomst tillägget tooreset eller ändra värdena för fjärråtkomst på en virtuell Azure-dator.
<!-- This definition seems obscure toome; maybe a list of examples would work better than a conceptual definition? -->
Se [om virtuella datortillägg och funktioner (Windows)](virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [om virtuella datortillägg och funktioner (Linux)](virtual-machines/linux/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="vnet"></a>virtuellt nätverk
Ett nätverk som tillhandahåller anslutningar mellan dina Azure-resurser som är isolerad från andra Azure klienter. En [Azure VPN-Gateway](vpn-gateway/vpn-gateway-about-vpngateways.md) kan du upprätta anslutningar mellan virtuella nätverk och [mellan ett virtuellt nätverk och ett lokalt nätverk](vpn-gateway/vpn-gateway-plan-design.md). Du kan helt styra hello IP-Adressblock, DNS-inställningar, säkerhetsprinciper och Routingtabellerna inom ett virtuellt nätverk.  
Se [översikt över virtuella nätverk](virtual-network/virtual-networks-overview.md)  

## <a name="web-app"></a>Webbapp
Ett annat namn för [Apptjänst-App](#app-service-app).

## <a name="see-also"></a>Se även

* [Kom igång med Azure](https://azure.microsoft.com/get-started/)
* [Molnet resource center](https://azure.microsoft.com/resources/)  
* [Azure för dina affärsprogram](https://azure.microsoft.com/overview/business-apps-on-azure/)
* [Azure i ditt datacenter](https://azure.microsoft.com/overview/business-apps-on-azure/)

