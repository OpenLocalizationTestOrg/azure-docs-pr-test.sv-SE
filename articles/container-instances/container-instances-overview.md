---
title: "aaaAzure behållaren instanser översikt | Azure-dokument"
description: "Förstå Azure Container Instances"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0662ede1260b15d9841bfc2c3c4cec4c30338d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances"></a>Azure Container Instances

Behållare blir snabbt hello önskad sätt toopackage, distribuera och hantera molnprogram. Behållarinstanser som Azure erbjuder hello snabbaste och enklaste sättet toorun en behållare i Azure, utan att behöva tooprovision alla virtuella datorer och utan att behöva tooadopt en högre nivå tjänst. 

Azure Container Instances är en bra lösning för alla scenarier som kan fungera i isolerade behållare, däribland enkla program, automatisering av uppgifter och att skapa jobb. För scenarier där du måste ha fullständig behållaren orchestration, inklusive tjänstidentifiering över flera behållare, automatisk skalning och samordnade programuppgraderingar rekommenderar vi hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).

## <a name="fast-startup-times"></a>Snabba starttider

Med behållare får du betydande startfördelar jämfört med virtuella datorer. Du kan starta en behållare i Azure i sekunder utan hello måste tooprovision och hantera virtuella datorer med Azure Container instanser.

## <a name="hypervisor-level-security"></a>Säkerhet på hypervisornivå

Tidigare har behållare erbjudit isolering av programberoenden och resursstyrning men har inte ansetts vara tillräckligt strikta för fientlig användning med flera innehavare. Med Azure Container Instances är ditt program lika isolerat i en behållare som i en virtuell dator.

## <a name="custom-sizes"></a>Anpassade storlekar

Behållare är vanligtvis optimerade toorun bara ett enda program, men hello exakta behov av dessa program kan variera avsevärt. Med Azure Container Instances kan du begära exakt det du behöver när det gäller processorkärnor och minne. Du betalar utifrån vad du begär fakturerats av hello andra, så du kan finjustera optimera dina utgifter baserat på dina behov.

## <a name="public-ip-connectivity"></a>Offentlig IP-anslutning

Med Azure Container instanser, kan du exponera behållarna direkt toohello internet med en offentlig IP-adress. I framtida hello, kommer vi Expandera våra nätverk funktioner tooinclude integrering med virtuella nätverk, belastningsutjämnare och andra delar av hello Azure nätverksinfrastruktur.

## <a name="persistent-storage"></a>Beständig lagring

tooretrieve och beständiga tillstånd med Azure Container instanser, vi erbjuder direkt montering av Azure filer resurser.

## <a name="linux-and-windows-containers"></a>Linux- och Windows-behållare

Du kan schemalägga både Windows och Linux-behållare med hello samma API med Azure Container instanser. Bara ange hello OS bastypen och allt annat är identiska.

## <a name="co-scheduled-groups"></a>Samordna schemalagda grupper

Azure Container Instances stöder schemaläggning av grupper med flera behållare som delar en värddator, lokalt nätverk, lagring och livscykel. Detta gör att du toocombine huvudsakliga programmet med andra i en stödjande roll, t.ex loggning.

## <a name="next-steps"></a>Nästa steg

Försök att distribuera en behållare tooAzure med ett enda kommando med hjälp av vår [Snabbstartsguide](container-instances-quickstart.md).
