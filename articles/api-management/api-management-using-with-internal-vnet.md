---
title: "Hur du använder Azure API Management med internt virtuellt nätverk | Microsoft Docs"
description: "Lär dig mer om att installera och konfigurera Azure API Management i internt virtuellt nätverk."
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: 
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 55248387c7e78d05c1cf1afd615b7b921e9669d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a>Med hjälp av Azure API Management-tjänsten med internt virtuellt nätverk
Med virtuella Azure-nätverk (Vnet), kan API-hantering hantera API: er som inte är tillgänglig på Internet. Ett antal VPN-tekniker som är tillgängliga för att ansluta och API-hantering kan distribueras i två huvudsakliga lägen i VNET:
* Extern
* internt

## <a name="overview"> </a>Översikt
När API Management har distribuerats i ett internt virtuellt nätverk-läge måste åtkomst alla Tjänsteslutpunkter (gateway, developer-portalen, publisher portal, direkthantering och Git) visas bara i ett virtuellt nätverk som du styr till. Inget av Tjänsteslutpunkter registreras på den offentliga DNS-servern.

Använder API-hantering i internt läge kan uppnå du följande scenarier
* På ett säkert sätt göra API: er som finns i ditt privata datacenter som är tillgängliga med 3 parter utanför den med hjälp av plats-till-plats eller ExpressRoute VPN-anslutningar.
* Aktivera hybrid cloud scenarier genom att exponera dina molnbaserade API: er och lokala API: er via en vanliga gateway.
* Hantera dina API: er som finns i flera geografiska platser med hjälp av en enda Gateway-slutpunkt. 

## <a name="enable-vpn"></a>Skapar en API-hantering i internt virtuellt nätverk
API Management-tjänst i interna virtuella nätverk ligger bakom en intern belastning Balancer(ILB). IP-adressen för ILB finns i den [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) intervall.  

### <a name="enable-vnet-connection-using-azure-portal"></a>Aktivera anslutning till virtuellt nätverk med hjälp av Azure portal
Skapa först API Management-tjänsten genom att följa stegen [skapa API Management-tjänsten][Create API Management service]. Sedan ställa in API-hantering som ska distribueras i ett virtuellt nätverk.

![Menyn för att ställa in APIM i internt virtuellt nätverk][api-management-using-internal-vnet-menu]

När distributionen lyckas, bör du se interna virtuella IP-adressen för din tjänst på instrumentpanelen.

![API Management-instrumentpanel med interna virtuella nätverk konfigureras][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a>Aktivera anslutning till VNET med Powershell-cmdlets
Du kan också aktivera VNET-anslutning med hjälp av PowerShell-cmdlets.

* **Skapa en API Management-tjänst i ett VNET**: Använd cmdlet [ny AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) att skapa en Azure API Management-tjänst i ett VNET och konfigurera den att använda typen interna virtuella nätverk.

* **Distribuera en befintlig API Management-tjänst i ett VNET**: Använd cmdlet [uppdatering AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) att flytta en befintlig Azure API Management-tjänst i ett virtuellt nätverk och konfigurera den att använda Internt VNET-typen.

## <a name="apim-dns-configuration"></a>DNS-konfiguration
När du använder API-hantering i läget för externa virtuella nätverk, hanteras DNS av Azure. Du måste hantera egna DNS för den interna virtuella nätverk läge.

> [!NOTE]
> API Management-tjänsten lyssnar inte till lyssnar på IP-adresser. Den bara svarar på förfrågningar om att värdnamnet som konfigurerats på dess slutpunkter (som innehåller gateway, developer-portalen, publisher Portal, direkthantering slutpunkt och git).

### <a name="access-on-default-host-names"></a>Åtkomst på standard värdnamn:
När du skapar en API Management-tjänst i offentliga Azure-molnet, till exempel med namnet ”contoso”, konfigureras följande Tjänsteslutpunkter som standard.

>   Gateway / Proxy – contoso.azure api.net

> Publisher Portal och Developer Portal - contoso.portal.azure api.net

> Direkthantering slutpunkt - contoso.management.azure api.net

>   GIT - contoso.scm.azure api.net

Om du vill få åtkomst till dessa slutpunkter för API Management-tjänsten måste skapa du en virtuell dator i ett undernät som är anslutet till det virtuella nätverket som API Management har distribuerats. Under förutsättning att den interna virtuella IP-adressen för din tjänst är 10.0.0.5, kan du göra värdarna filen mappning (% SystemDrive%\drivers\etc\hosts) enligt följande:

> 10.0.0.5 contoso.azure-api.net

> 10.0.0.5 contoso.portal.azure-api.net

> 10.0.0.5 contoso.management.azure-api.net

> 10.0.0.5 contoso.scm.azure-api.net

Du kan sedan komma åt alla Tjänsteslutpunkter från den virtuella datorn som du skapade. Om du använder en anpassad DNS-server i ett virtuellt nätverk kan du också skapa en DNS-poster och åtkomst till dessa slutpunkter från var som helst i ditt virtuella nätverk. 

### <a name="access-on-custom-domain-names"></a>Åtkomst på anpassade domännamn:
Om du inte vill att få åtkomst till API Management-tjänsten med standard-värdnamn, kan du konfigurera anpassade domännamn för alla dina slutpunkter som nedan

![Ställa in anpassad domän för API Management][api-management-custom-domain-name]

Du kan skapa en poster i DNS-servern för att få åtkomst till dessa slutpunkter som endast är tillgänglig från din virtuella nätverk.

## <a name="related-content"></a>Relaterat innehåll
* [Vanliga problem med nätverket konfiguration när du konfigurerar APIM i virtuella nätverk][Common Network Configuration Issues]
* [Virtuella nätverk vanliga frågor och svar](../virtual-network/virtual-networks-faq.md)
* [Skapa en post i DNS](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
