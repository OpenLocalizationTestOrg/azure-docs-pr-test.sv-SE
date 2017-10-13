---
title: "Exempel på Azure CLI - Apptjänst | Microsoft Docs"
description: "Exempel på Azure CLI - Apptjänst"
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 6718694af487929d193dae54ecb2d85ece64887a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cli-samples"></a>Azure CLI-exempel

Följande tabell innehåller länkar till bash-skript som skapats med hjälp av Azure CLI.

| | |
|-|-|
|**Skapa app**||
| [Skapa en webbapp och distribuera kod från GitHub](./scripts/app-service-cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar en Azure webbapp och distribuerar kod från en offentlig GitHub-databas. |
| [Skapa en webbapp med kontinuerlig distribution från GitHub](./scripts/app-service-cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar ett Azure-webbapp med kontinuerlig publicering från en GitHub-databas som du äger. |
| [Skapa en webbapp och distribuera kod från en lokal Git-databas](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar ett Azure-webbapp och konfigurerar push kod från en lokal Git-lagringsplats. |
| [Skapa en webbapp och distribuera kod till en mellanlagringsmiljö](./scripts/app-service-cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar en Azure-app med en distributionsplats för mellanlagring kodändringar. |
| [Skapa en ASP.NET Core-webbapp i en Docker-behållare](./scripts/app-service-cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar ett Azure-webbapp på Linux och läser in en Docker-avbildning från Docker-hubben. |
|**Konfigurera appen**||
| [Mappa en anpassad domän till en webbapp](./scripts/app-service-cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar en Azure webbapp och mappa ett anpassat domännamn till den. |
| [Koppla en anpassad SSL-certifikatet till en webbapp](./scripts/app-service-cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar en Azure webbapp och binda SSL-certifikatet för ett anpassat domännamn till den. |
|**Skala appen**||
| [Skala en webbapp manuellt](./scripts/app-service-cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar en Azure webbapp och skalas över 2 instanser. |
| [Skala en webbapp över hela världen med en arkitektur med hög tillgänglighet](./scripts/app-service-cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar två Azure-webbappar i två olika geografiska regioner och gör dem tillgängliga via en enda slutpunkt med hjälp av Azure Traffic Manager. |
|**Ansluta appen till resurser**||
| [Ansluta en webbapp till en SQL-databas](./scripts/app-service-cli-app-service-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar ett Azure webbapp och en SQL-databas och sedan lägger till databasanslutningssträngen app-inställningar. |
| [Ansluta en webbapp till ett lagringskonto](./scripts/app-service-cli-app-service-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Skapar ett Azure webbapp och ett lagringskonto och sedan lägger till lagringsanslutningssträngen i app-inställningar. |
| [Ansluta en webbapp till ett redis-cache](./scripts/app-service-cli-app-service-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar ett Azure webbapp och ett redis-cache och sedan lägger till redis-anslutningsinformationen i appinställningar.) |
| [Ansluta en webbapp till Cosmos DB](./scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar ett Azure webbapp och en Cosmos-DB och sedan lägger till Cosmos-DB-anslutningsinformationen i app-inställningar. |
|**Övervaka app**||
| [Övervaka en webbapp med webbserverloggar](./scripts/app-service-cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Skapar ett Azure-webbapp, aktiverar loggning för den och hämtar loggarna till din lokala dator. |
| | |