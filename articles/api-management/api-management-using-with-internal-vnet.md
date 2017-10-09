---
title: "aaaHow toouse Azure API Management med internt virtuellt nätverk | Microsoft Docs"
description: "Lär dig hur toosetup och konfigurera Azure API Management i internt virtuellt nätverk."
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
ms.openlocfilehash: 8d25de14e0c0bebe7ba7b47ca432ea4e45dde312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a>Med hjälp av Azure API Management-tjänsten med internt virtuellt nätverk
Med virtuella Azure-nätverk (Vnet), kan API-hantering hantera API: er inte kan nås på hello Internet. Ett antal VPN-tekniker är tillgängliga toomake hello anslutning och API-hantering kan distribueras i två huvudsakliga lägen i hello VNET:
* Extern
* internt

## <a name="overview"> </a>Översikt
När API Management har distribuerats i ett internt virtuellt nätverk-läge måste är alla hello slutpunkter (gateway, developer-portalen, publisher portal, direkthantering och Git) bara synliga i ett virtuellt nätverk som du styr åtkomst till. Inget av slutpunkter hello är registrerad på hello offentliga DNS-servern.

Använder API-hantering i internt läge kan uppnå du hello följande scenarier
* På ett säkert sätt göra API: er som finns i ditt privata datacenter som är tillgängliga med 3 parter utanför den med hjälp av plats-till-plats eller ExpressRoute VPN-anslutningar.
* Aktivera hybrid cloud scenarier genom att exponera dina molnbaserade API: er och lokala API: er via en vanliga gateway.
* Hantera dina API: er som finns i flera geografiska platser med hjälp av en enda Gateway-slutpunkt. 

## <a name="enable-vpn"></a>Skapar en API-hantering i internt virtuellt nätverk
hello API Management-tjänsten i interna virtuella nätverk finns bakom en intern Balancer(ILB) belastningen. hello hello ILB IP-adress är i hello [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) intervall.  

### <a name="enable-vnet-connection-using-azure-portal"></a>Aktivera anslutning till virtuellt nätverk med hjälp av Azure portal
Först skapa hello API Management-tjänsten genom att följa stegen i hello [skapa API Management-tjänsten][Create API Management service]. Sedan ställa in API-hantering toobe distribueras i ett virtuellt nätverk.

![Menyn för att ställa in APIM i internt virtuellt nätverk][api-management-using-internal-vnet-menu]

När hello distribution lyckas, bör du se hello interna virtuella IP-adressen för din tjänst på hello instrumentpanel.

![API Management-instrumentpanel med interna virtuella nätverk konfigureras][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a>Aktivera anslutning till VNET med Powershell-cmdlets
Du kan också aktivera VNET-anslutning med hello PowerShell-cmdlets.

* **Skapa en API Management-tjänst i ett VNET**: Använd hello cmdlet [ny AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate ett Azure API Management-tjänst i ett VNET och konfigurera den toouse hello interna virtuella nätverk typen.

* **Distribuera en befintlig API Management-tjänst i ett VNET**: Använd cmdlet hello [uppdatering AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove en befintlig Azure API Management-tjänsten i ett virtuellt nätverk och konfigurera den toouse Internt VNET-typen.

## <a name="apim-dns-configuration"></a>DNS-konfiguration
När du använder API-hantering i läget för externa virtuella nätverk, hanteras DNS av Azure. Internt virtuellt nätverk läge har du toomanage egna DNS.

> [!NOTE]
> API Management-tjänsten lyssnar inte toorequests kommer på IP-adresser. Endast svarar den toorequests toohello värdnamn som konfigurerats på dess slutpunkter (som innehåller gateway, developer-portalen, publisher Portal, direkthantering slutpunkt och git).

### <a name="access-on-default-host-names"></a>Åtkomst på standard värdnamn:
När du skapar en API Management-tjänst i offentliga Azure-molnet, till exempel med namnet ”contoso”, konfigureras hello följande slutpunkter som standard.

>   Gateway / Proxy – contoso.azure api.net

> Publisher Portal och Developer Portal - contoso.portal.azure api.net

> Direkthantering slutpunkt - contoso.management.azure api.net

>   GIT - contoso.scm.azure api.net

tooaccess dessa slutpunkter för API Management-tjänsten, kan du skapa en virtuell dator i en ansluten toohello virtuella undernätverk som API Management har distribuerats. Under förutsättning att hello interna virtuella IP-adressen för din tjänst är 10.0.0.5, kan du göra hello mappning hosts-filen (% SystemDrive%\drivers\etc\hosts) enligt följande:

> 10.0.0.5 contoso.azure-api.net

> 10.0.0.5 contoso.portal.azure-api.net

> 10.0.0.5 contoso.management.azure-api.net

> 10.0.0.5 contoso.scm.azure-api.net

Du kan sedan komma åt alla hello slutpunkter från hello virtuell dator som du skapade. Om du använder en anpassad DNS-server i ett virtuellt nätverk kan du också skapa en DNS-poster och åtkomst till dessa slutpunkter från var som helst i ditt virtuella nätverk. 

### <a name="access-on-custom-domain-names"></a>Åtkomst på anpassade domännamn:
Om du inte vill tooaccess hello API Management-tjänsten med hello standard värdnamn, kan du konfigurera anpassade domännamn för alla dina slutpunkter som nedan

![Ställa in anpassad domän för API Management][api-management-custom-domain-name]

Du kan skapa A-poster i DNS-Server-tooaccess dessa slutpunkter som endast är tillgänglig från din virtuella nätverk.

## <a name="related-content"></a>Relaterat innehåll
* [Vanliga problem med nätverket konfiguration när du konfigurerar APIM i virtuella nätverk][Common Network Configuration Issues]
* [Virtuella nätverk vanliga frågor och svar](../virtual-network/virtual-networks-faq.md)
* [Skapa en post i DNS](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
