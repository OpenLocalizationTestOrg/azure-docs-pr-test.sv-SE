---
title: "aaaLoad data till Azure storage-miljöer för analytics | Microsoft Docs"
description: "Flytta Data tooand från Azure Blob Storage"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 0fea2290991f9fa63d9e46c3a657000e27d95289
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Läs in data i lagringsmiljöer för analys
hello Team datavetenskap processen kräver att data att inhämtas eller läses in i en mängd olika lagringsplatser miljöer toobe analyseras i hello lämpligaste sättet i varje steg i processen för hello eller bearbetades. Datamål som ofta används för bearbetning inkluderar Azure Blob Storage, SQL Azure-databaser, SQL Server på Azure VM, HDInsight (Hadoop) och Azure Machine Learning. 

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Detta **menyn** länkar tootopics som beskriver hur tooingest data till dessa mål miljöer där hello data lagras och bearbetas.

Teknisk och affärsbehov samt hello ursprungliga plats, formatera och storleken på dina data avgör hello mål miljöer i vilka hello data måste toobe inhämtas tooachieve hello mål av dina analyser. Det är inte ovanligt att en scenariot toorequire data toobe flyttas mellan flera miljöer tooachieve hello olika uppgifter krävs tooconstruct en förutsägelsemodell. Den här aktivitetssekvensen kan exempelvis innehålla datagranskning, föregående bearbetning, rensa, ned provtagning och modellen utbildning.

