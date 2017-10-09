---
title: "aaaAzure anslutningssträngen för Service Fabric image store | Microsoft Docs"
description: "Förstå hello image store-anslutningssträng"
services: service-fabric
documentationcenter: .net
author: alexwun
manager: timlt
editor: 
ms.assetid: 00f8059d-9d53-4cb8-b44a-b25149de3030
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: alexwun
ms.openlocfilehash: 83f5ad75b5df07726997da3173722028255b8cae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-imagestoreconnectionstring-setting"></a>Förstå hello ImageStoreConnectionString inställning

I vissa av vår dokumentation nämna vi utan som beskriver vad det verkligen innebär en kort hello förekomsten av en ”ImageStoreConnectionString”-parametern. Och efter att gå via en artikel som [distribuera och ta bort program med hjälp av PowerShell][10], det ser ut som du är kopiera och klistra in hello värdet som visas i hello klustermanifestet hello mål kluster. Så hello måste vara kan konfigureras per kluster, men när du skapar ett kluster via hello [Azure-portalen][11], det finns inga alternativ tooconfigure den här inställningen och det är alltid ”fabric: Avbildningsarkiv”. Vad är hello syftet med den här inställningen?

![Klustermanifestet][img_cm]

Service Fabric startas som en plattform för intern Microsoft-användning av många olika team så vissa aspekter av det är mycket anpassningsbara - hello ”Image Store” är en aspekt. Hello Image Store är i princip en pluggable databas för att lagra programpaket. När ditt program är distribuerade tooa nod i klustret hello, hämtar noden hello innehållet i ditt programpaket från hello Image Store. Hej ImageStoreConnectionString är en inställning som innehåller alla hello nödvändig information för både klienter och noderna toofind hello rätt Image Store för ett kluster.

Det finns tre möjliga typer av Image Store-providers och deras motsvarande anslutningssträngar är följande:

1. Image Store-tjänsten: ”fabric: Avbildningsarkiv”

2. Filsystem: ”file:[file systemsökvägen]”

3. Azure Storage ”: xstore:DefaultEndpointsProtocol = https; AccountName = [...]; AccountKey = [...]; Behållaren = [...] ”

hello providertyp som används i produktionen är hello Image Store-tjänsten, som är en tillståndskänslig beständiga systemtjänst som du ser i Service Fabric Explorer. 

![Image Store-tjänsten][img_is]

Värd hello Image Store i en systemtjänst i själva hello klustret eliminerar externa beroenden för hello paketdatabasen och ger oss mer kontroll över hello ort lagringsutrymme. Framtida förbättringar runt hello Image Store är sannolikt tootarget hello Image Store-providern först om inte exklusivt. hello anslutningssträngen för hello Image Store Service-providern har inte någon unik information eftersom hello klienten är redan anslutet toohello målkluster. hello klienten behöver bara tooknow riktad hello systemtjänst protokoll som ska användas.

hello filsystemet providern används i stället för hello Image Store-tjänsten för lokala en ruta kluster under utveckling toobootstrap hello klustret något snabbare. hello skillnaden är vanligtvis mindre, men det är en användbar optimering för de flesta avdelningen under utvecklingen. Det är möjligt toodeploy lokala en-box-kluster med hello andra providern lagringstyper samt, men det finns vanligtvis ingen anledning toodo så eftersom hello utveckla och testning arbetsflödet fortsätter att vara hello samma oavsett providern. Än denna användning finns hello filsystemet och Azure Storage-providers endast för bakåtkompatibilitet.

Så medan hello ImageStoreConnectionString konfigureras använda du vanligtvis bara hello standardinställningen. När du publicerar tooAzure via [Visual Studio][12], hello parametern anges automatiskt i enlighet med detta åt dig. Hello anslutningssträngen är alltid ”fabric: Avbildningsarkiv” för programdistribution tooclusters finns i Azure. Även om när osäkra, värdet alltid kan verifieras genom att hämta hello klustermanifestet av [PowerShell](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricclustermanifest), [.NET](https://msdn.microsoft.com/library/azure/mt161375.aspx), eller [REST](https://docs.microsoft.com/rest/api/servicefabric/get-a-cluster-manifest). Testa både lokalt och driftskluster ska alltid vara konfigurerade toouse hello Image Store Service provider samt.

### <a name="next-steps"></a>Nästa steg
[Distribuera och ta bort program med hjälp av PowerShell][10]

<!--Image references-->
[img_is]: ./media/service-fabric-image-store-connection-string/image_store_service.png
[img_cm]: ./media/service-fabric-image-store-connection-string/cluster_manifest.png

[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-cluster-creation-via-portal.md
[12]: service-fabric-publish-app-remote-cluster.md
