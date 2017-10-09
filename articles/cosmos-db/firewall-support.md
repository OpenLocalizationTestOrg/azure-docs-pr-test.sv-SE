---
title: "aaaAzure Cosmos-DB-brandväggsstöd & IP-åtkomstkontroll | Microsoft Docs"
description: "Lär dig hur toouse IP-principer för åtkomstkontroll för brandväggen stöder på Azure Cosmos DB databasen konton."
keywords: "IP-åtkomstkontroll, brandväggsstöd"
services: cosmos-db
author: shahankur11
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: c1b9ede0-ed93-411a-ac9a-62c113a8e887
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ankshah
ms.openlocfilehash: b5cdbdb28e9d7ee0fd0ea54aad277167b699929f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-firewall-support"></a>Azure DB Cosmos-brandväggsstöd
toosecure data som lagras i Azure DB som Cosmos-databaskonto, Azure Cosmos DB tillhandahåller stöd för en hemlighet baserat [auktoriseringsmodellen](https://msdn.microsoft.com/library/azure/dn783368.aspx) som använder en stark hashbaserad meddelandeautentiseringskod (HMAC). Nu kan dessutom toohello hemlighet baserad auktoriseringsmodellen, Azure Cosmos DB stöder drivs IP-baserade åtkomstkontroller för inkommande brandväggsstöd-princip. Den här modellen är mycket lik toohello brandväggsregler vid traditionella system och ger en extra nivå av säkerhet toohello Azure Cosmos DB databaskonto. Med den här modellen kan du nu konfigurera en Azure Cosmos DB databasen konto toobe endast nås från en godkänd uppsättning datorer eller molntjänster. Komma åt tooAzure Cosmos DB resurser från dessa godkända uppsättningar av datorer och tjänster kräver fortfarande hello anroparen toopresent en giltig auktoriserings-token.

## <a name="ip-access-control-overview"></a>IP-åtkomstkontroll: översikt
Ett konto för Azure DB som Cosmos-databasen är tillgänglig från offentliga internet som standard så länge hello begäran åtföljs av en giltig auktoriserings-token. tooconfigure IP-policy-baserad åtkomstkontroll hello användaren måste ange hello uppsättning IP-adresser och IP-adressintervall i CIDR formuläret toobe inkluderad som hello tillåts lista över klientens IP-adresser för en viss databas-konto. När den här konfigurationen används blockeras alla begäranden från datorer utanför den här listan över tillåtna av hello-servern.  hello anslutningen processchema för hello IP-baserad åtkomstkontroll är beskrivs i följande diagram hello.

![Diagram som visar hello anslutningsprocessen för IP-baserad åtkomstkontroll](./media/firewall-support/firewall-support-flow.png)

## <a name="connections-from-cloud-services"></a>Anslutningar från molntjänster
I Azure är cloud services ett mycket vanligt sätt som värd för tjänsten mellannivålogik med hjälp av Azure Cosmos DB. tooenable åtkomst tooan Azure Cosmos DB databaskonto från en molnbaserad tjänst hello offentliga IP-adressen för hello Molntjänsten måste vara tillagda toohello tillåts lista över IP-adresser som är associerade med din Azure Cosmos DB databaskonto av [konfigurera Hej IP-princip för åtkomstkontroll](#configure-ip-policy).  Detta säkerställer att alla rollinstanser molntjänster åtkomst tooyour Azure Cosmos DB konto. Du kan hämta IP-adresser för dina molntjänster i hello Azure-portalen som visas i följande skärmbild hello.

![Skärmbild som visar hello offentliga IP-adressen för en molnbaserad tjänst som visas i hello Azure-portalen](./media/firewall-support/public-ip-addresses.png)

När du skalar upp din molntjänst genom att lägga till ytterligare rollen instanser har de nya instanserna automatiskt databasen åtkomstkonto toohello Azure Cosmos DB eftersom de är en del av hello samma molntjänst.

## <a name="connections-from-virtual-machines"></a>Anslutningar från virtuella datorer
[Virtuella datorer](https://azure.microsoft.com/services/virtual-machines/) eller [skalningsuppsättningar i virtuella](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) kan också vara används toohost mellannivå tjänster med hjälp av Azure Cosmos DB.  tooconfigure hello Azure Cosmos DB konto tooallow dataåtkomst från virtuella datorer, offentliga IP-adresser för virtuell dator och/eller virtuella datorn måste vara konfigurerad som en hello tillåtna IP-adresser för dina Azure Cosmos DB databaskonto av [Konfigurera princip för åtkomstkontroll för hello IP](#configure-ip-policy). Du kan hämta IP-adresser för virtuella datorer i hello Azure-portalen som visas i följande skärmbild hello.

![Skärmbild som visar en offentlig IP-adress för en virtuell dator visas i hello Azure-portalen](./media/firewall-support/public-ip-addresses-dns.png)

När du lägger till ytterligare virtuella instanser toohello gruppen finns de automatiskt åtkomst tooyour Azure Cosmos DB konto.

## <a name="connections-from-hello-internet"></a>Anslutningar från hello internet
När du åtkomst till en Azure-Cosmos-DB databasen konto från en dator på hello internet, vara hello klientens IP-adress eller IP-adressintervall för hello datorn tillagda toohello tillåts lista över IP-adress för hello Azure Cosmos DB databaskonto. 

## <a id="configure-ip-policy"></a>Konfigurera princip för åtkomstkontroll för hello IP
princip för åtkomstkontroll för hello IP kan anges i hello Azure-portalen eller programmässigt via [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), eller hello [REST API](/rest/api/documentdb/) genom att uppdatera hello `ipRangeFilter` egenskapen. IP-adressintervall måste vara kommatecken avgränsade och får inte innehålla blanksteg. Exempel: ”13.91.6.132,13.91.6.1/24”. När du uppdaterar din databaskonto via metoderna, vara säker på att toopopulate alla hello egenskaper tooprevent återställer toodefault inställningar.

> [!NOTE]
> Genom att aktivera en IP-principer för åtkomstkontroll för din Azure Cosmos DB databaskonto tillåts alla åtkomst tooyour Azure Cosmos DB databaskonto från datorer utanför hello konfigurerad lista med IP-adressintervall blockeras. Tack vare den här modellen kommer surfning hello data plan åtgärden från hello portal också att blockerade tooensure hello integriteten för åtkomstkontroll.

toosimplify utveckling hello Azure-portalen hjälper dig att identifiera och lägga till hello IP för din klient datorn toohello tillåts lista, så att appar som körs på datorn kan komma åt hello Azure DB som Cosmos-konto. Observera att hello klientens IP-adress här har identifierats som visas av hello-portalen. Det kan vara hello klientens IP-adress för datorn, men det kan också vara hello IP-adressen för din nätverks-gateway. Glöm inte tooremove före den pågående tooproduction.

tooset hello IP-princip för åtkomstkontroll i hello Azure-portalen navigera toohello Azure DB som Cosmos-kontoblad klickar du på **brandväggen** i navigeringsmenyn hello Klicka **ON** 

![Skärmbild som visar hur tooopen hello brandväggen bladet i hello Azure-portalen](./media/firewall-support/azure-portal-firewall.png)

I hello nya rutan, anger du om hello Azure-portalen kan komma åt hello konto och lägga till andra adresser och intervall efter behov och sedan klickar du på **spara**.  

> [!NOTE]
> När du aktiverar en IP-principer för åtkomstkontroll, behöver du tooadd hello IP-adress för hello Azure portal toomaintain åtkomst. hello portal IP-adresser är:
> |Region|IP-adress|
> |------|----------|
> |Alla regioner utom de som anges nedan| 104.42.195.92|
> |Tyskland|51.4.229.218|
> |Kina|139.217.8.252|
> |Arizona (USA-förvaltad region)|52.244.48.71|
>

![Skärmbild som visar en hur tooconfigure brandväggens inställningar i hello Azure-portalen](./media/firewall-support/azure-portal-firewall-configure.png)

## <a name="troubleshooting-hello-ip-access-control-policy"></a>Felsöka hello IP-princip för åtkomstkontroll
### <a name="portal-operations"></a>Portalen åtgärder
Genom att aktivera en IP-principer för åtkomstkontroll för din Azure Cosmos DB databaskonto tillåts alla åtkomst tooyour Azure Cosmos DB databaskonto från datorer utanför hello konfigurerad lista med IP-adressintervall blockeras. Om du vill tooenable portal plan dataåtgärder som surfning samlingar och dokument i frågan, måste du därför tooexplicitly Tillåt Azure portal åtkomst med hjälp av hello **brandväggen** bladet i hello portal. 

![Skärmbild som visar en hur tooenable åtkomst toohello Azure-portalen](./media/firewall-support/azure-portal-access-firewall.png)

### <a name="sdk--rest-api"></a>SDK & Rest API
För säkerhetsskäl åtkomst via SDK eller REST-API från datorer som inte finns på listan över tillåtna hello returneras ett allmänt 404 gick inte att hitta svar utan ytterligare information. Kontrollera hello IP tillåts lista som konfigurerats för din Azure Cosmos DB databasen tooensure hello rätt princip kontokonfiguration tillämpade tooyour Azure Cosmos DB konto.

## <a name="next-steps"></a>Nästa steg
Information om nätverk prestandatips finns [prestandatips](performance-tips.md).

