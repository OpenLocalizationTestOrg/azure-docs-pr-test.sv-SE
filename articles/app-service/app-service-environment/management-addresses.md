---
title: "aaaAzure Apptjänstmiljö management adresser"
description: "Visar hello management adresser används toocommand en Apptjänst-miljö"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: b34b6266dc3a35915421b14bf34eddc07c2825c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-environment-management-addresses"></a>App-miljö management adresser

Hej Environment(ASE) för App Service är en distribution av hello Azure App Service till ett undernät i ditt virtuella Azure-nätverk (VNet).  Hej ASE måste vara tillgänglig från hello Azure App Service så att den kan hanteras.  Den här ASE hantering av trafik passerar hello användarstyrd nätverk.  Det kommer från Azure App Service management-servrar toohello offentliga VIP som är associerad med hello ASE.  Mer information om hello ASE nätverk beroenden läsa [nätverk överväganden och hello Apptjänstmiljö][networking].  Allmän information om hello ASE börjar du med [introduktion toohello Apptjänstmiljö][intro].

Det här dokumentet innehåller hello källa IP-adresser för hantering av trafik toohello ASE. Du kan använda dessa adresser toocreate Nätverkssäkerhetsgrupper toolock ned inkommande trafik eller använda dem i vägtabeller efter behov.  toouse denna information som du behöver toouse:

* hello IP-adresser som anges för alla regioner
* hello IP-adresser som matchar toohello region som din ASE distribueras till.

hello inkommande hantering av trafik som kommer in från dessa IP-adresser tooports 454 och 455.

| Region | Adresser |
|--------|-----------|
| Alla regioner | 70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141 |
| Södra centrala USA & norra centrala USA | 23.102.188.65, 191.236.154.88 |
| Australien, sydost & Östra Australien | 23.101.234.41, 104.210.90.65 |
| USA, Väst & USA, Öst | 104.45.227.37, 191.236.60.72 |
| Västra Europa & Norra Europa | 191.233.94.45, 191.237.222.191 |
| West centrala USA & västra USA 2 | 13.78.148.75, 13.66.225.188 |
| Centrala USA & östra USA 2 | 104.43.165.73, 104.46.108.135 |
| Östasien & Sydostasien | 23.99.115.5, 104.215.158.33 |
| Östra & Japan, Väst | 104.41.185.116, 191.239.104.48 |
| Kanada Central & Kanada, Öst | 40.85.230.101, 40.86.229.100 |
| Storbritannien, Väst & Storbritannien, Syd | 51.141.8.34, 51.140.185.75 |
| Sydkorea söder & Korea Central | 52.231.200.177, 52.231.32.117 |
| Södra & södra centrala USA| 104.41.46.178, 23.102.188.65 |
| Centrala Indien & södra Indien | 104.211.98.24, 104.211.225.66 |
| Västra Indien & södra Indien | 104.211.160.229, 104.211.225.66 |


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
