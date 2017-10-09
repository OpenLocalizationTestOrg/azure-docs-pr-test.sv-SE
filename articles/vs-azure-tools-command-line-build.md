---
title: "aaaCommand rad build för Azure | Microsoft Docs"
description: "Kommandoradsverktyget build för Azure"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2017
ms.author: kraigb
ms.openlocfilehash: 295b61ba162dd4373ee3f56cc1462decb3e16762
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="building-azure-projects-from-hello-command-line"></a>Skapa Azure-projekt från hello kommandorad
Du kan skapa produkter i build-labbmiljöer där Visual Studio inte har installerats med hello Microsoft skapa Engine (MSBuild). MSBuild använder ett XML-format för projektfiler som extensible och stöds fullt ut av Microsoft. Använda hello MSBuild-filformat, kan du beskriva vad objekt måste vara inbyggda för en eller flera plattformar och konfigurationer.

Du kan också köra MSBuild hello kommandoraden och det här avsnittet beskrivs den metoden. Du kan skapa specifika konfigurationer av ett projekt genom att ange egenskaper på hello-kommandoraden. På liknande sätt kan du också definiera hello mål som MSBuild-versioner. Mer information om kommandoradsparametrar och MSBuild finns [MSBuild Kommandoradsreferens](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>MSBuild-parametrar
hello enklaste sättet toocreate ett paket är toorun MSBuild med hello `/t:Publish` alternativet. Som standard det här kommandot skapar en katalog i relationen toohello rotmapp för hello projekt som `<ProjectDirectory>\bin\Configuration\app.publish\`. När du skapar ett Azure-projekt genereras två filer: hello paketfilen sig själv och hello medföljande konfigurationsfil:

* Appaketfil (`project.cspkg`)
* Konfigurationsfilen (`ServiceConfiguration.TargetProfile.cscfg`)

Som standard innehåller en service-konfigurationsfilen för lokala (felsökning) versioner och ett annat för molnet (mellanlagring eller produktion) versioner varje Azure-projekt. Du kan lägga till eller ta bort service configuration-filer efter behov. När du skapar ett paket i Visual Studio, tillfrågas du vilka service-konfigurationsfilen tooinclude tillsammans med hello-paketet. När du skapar ett paket med hjälp av MSBuild ingår hello lokala tjänstkonfiguration filen som standard. tooinclude en annan tjänstkonfiguration fil, ange hello `TargetProfile` -egenskapen för hello MSBuild-kommandot (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Om du vill toouse en alternativ katalog för hello lagras paket- och filer, ange hello sökväg med hjälp av hello `/p:PublishDir=Directory\` alternativ, inklusive hello avslutande omvänt avgränsare.

## <a name="next-steps"></a>Nästa steg
När hello paketet är skapat kan distribuera du den tooAzure. En genomgång som visar hur tooautomate som bearbetas, finns i [kontinuerlig leverans för molntjänster i Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).

