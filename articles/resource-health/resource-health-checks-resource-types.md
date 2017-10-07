---
title: aaaSupported resurstyper via Azure Resource health | Microsoft Docs
description: "Resurstyper som stöds via Azure Resource health"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: resource-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 03/19/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: a37d5c33f6ef6fdfbce0daf392bc7fa57853c7d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Hälsa och resurstyper kontrollerar i Azure resurshälsa
Nedan visas en fullständig lista över alla hello kontroller körs via resurshälsa av resurstyper.

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|Kontroller som utförs|
|---|
|<ul><li>Hello cachenoderna är igång och körs?</li><li>Hello Cache når från inom hello datacenter?</li><li>Har hello Cache uppnåtts hello maximalt antal anslutningar?</li><li> Hello cache har förbrukat det tillgängliga minnet? </li><li>Hello Cache har uppstått ett stort antal sidfel?</li><li>Är hello Cache vid hög belastning?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|Kontroller som utförs|
|---|
|<ul> <li>Har någon hello slutpunkter stoppats, borttagna eller felkonfigurerad?</li><li>Är hello kompletterande portalen tillgänglig för CDN konfigurationsåtgärder?</li><li>Finns det pågående leverans problem med hello CDN-slutpunkter?</li><li>Användare kan ändra hello konfiguration av deras CDN-resurser?</li><li>Sprider konfigurationsändringar i hello förväntade frekvens?</li><li>Användarna kan hantera hello CDN-konfigurationen med hjälp av hello Azure-portalen, PowerShell eller hello API?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Kontroller som utförs|
|---|
|<ul><li>Är hello värdservern igång och körs?</li><li>Hello värden OS startar slutfördes?</li><li>Hello virtuella datorbehållare etablerats och aktiverades?</li><li>Det finns en nätverksanslutning mellan hello-värden och hello storage-konto?</li><li>Hello start av hello gästoperativsystemet slutfördes?</li><li>Finns det pågående planerat underhåll?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/Accounts
|Kontroller som utförs|
|---|
|<ul><li>Kan nås hello konto från inom hello datacenter?</li><li>Är hello kognitiva Services Resursprovidern tillgängliga?</li><li>Är hello kognitiva tjänsten tillgänglig i hello aktuell region?</li><li>Kan läsa åtgärder utföras för hålla hello resursmetadata hello storage-konto?</li><li>Har hello API-anrop kvot uppnåtts?</li><li>Har hello API-anrop Läs-gränsen?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualmachines
|Kontroller som utförs|
|---|
|<ul><li>Är hello server värd för den här virtuella datorn in och kör?</li><li>Hello värden OS startar slutfördes?</li><li>Hello virtuella datorbehållare etablerats och aktiverades?</li><li>Det finns en nätverksanslutning mellan hello-värden och hello storage-konto?</li><li>Hello start av hello gästoperativsystemet slutfördes?</li><li>Finns det pågående planerat underhåll?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/Accounts
|Kontroller som utförs|
|---|
|<ul><li>Kan användare skicka jobb tooData Lake Analytics i hello region?</li><li>Utföra grundläggande jobb kör och fullständig har i hello region?</li><li>Användare kan visa katalogobjekt i hello region?</li>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/Accounts
|Kontroller som utförs|
|---|
|<ul><li>Användare kan överföra tooData Datasjölager i hello region?</li><li>Användare kan hämta data från Data Lake Store i hello region?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Kontroller som utförs|
|---|
|<ul><li>Det har förekommit någon databas eller en samling begäranden som inte hanteras på grund av tooa DocumentDB-tjänsten inte finns?</li><li>Det har förekommit alla begäranden för dokument som inte hanteras på grund av tooa DocumentDB-tjänsten inte finns?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/Connections
|Kontroller som utförs|
|---|
|<ul><li>Ansluts hello VPN-tunnel</li><li>Finns det konfigurationskonflikterna i hello-anslutningen?</li><li>Hello i förväg delade nycklar konfigureras korrekt?</li><li>Nås hello VPN lokala enhet?</li><li>Är hello IPSec/IKE-säkerhetsprincip avvikelser?</li><li>Är hello S2S VPN-anslutningen korrekt etablerade eller i ett felaktigt tillstånd?</li><li>Är hello VNET-till-VNET-anslutningen korrekt etablerade eller i ett felaktigt tillstånd?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|Kontroller som utförs|
|---|
|<ul><li>Är hello VPN-gateway kan nås från hello internet?</li><li>Är hello VPN-Gateway i vänteläge?</li><li>Är hello VPN-tjänsten körs på hello gateway?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Kontroller som utförs|
|---|
|<ul><li> Kan runtime åtgärder som registrering, installation eller skicka utföras på hello namnområde?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|Kontroller som utförs|
|---|
|<ul><li>Är hello värdens operativsystem igång och körs?</li><li>Är hello workspaceCollection kan nås från utanför hello datacentret?</li><li>Är hello PowerBI-Resursprovidern som är tillgängliga?</li><li>Är hello PowerBI Service tillgängliga i hello rätt region?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|Kontroller som utförs|
|---|
|<ul><li>Kan diagnostik åtgärder utföras på klustret hello?</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|Kontroller som utförs|
|---|
|<ul><li> Har det inloggningar toohello databasen?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Kontroller som utförs|
|---|
|<ul><li>Är alla hello värdar där hello jobbet körs in och kör?</li><li>Var hello jobbet toostart?</li><li>Finns det pågående runtime uppgraderingar?</li><li>Hello jobb i ett förväntat tillstånd (t ex igång eller stoppad av kund)?</li><li>Hello jobbet uppstod ut av minne undantag?</li><li>Finns det pågående schemalagda beräkning uppdateringar?</li><li>Är hello Körhanteraren (plan för åtkomstkontroll) tillgängliga?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Kontroller som utförs|
|---|
|<ul><li>Är hello värdservern igång och körs?</li><li>Körs Internet Information Services</li><li>Körs hello belastningsutjämnaren?</li><li>Kan hello Web Service-Plan kan nås från inom hello datacenter?</li><li>Finns hello lagring kontot värd hello platser innehåll för hello serverFarm?</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.Web/Sites
|Kontroller som utförs|
|---|
|<ul><li>Är hello värdservern igång och körs?</li><li>Internet Information server kör?</li><li>Körs hello belastningsutjämnaren?</li><li>Kan nås hello webbprogrammet från inom hello datacenter?</li><li>Hello storage-konto som är värd för hello webbplatsens innehåll är tillgängligt?</li></ul>|

Se dessa resurser toolearn mer om resurshälsa:
-  [Introduktion tooAzure resurshälsa](Resource-health-overview.md)
-  [Vanliga frågor och svar om Azure-resurs hälsotillstånd](Resource-health-faq.md)

