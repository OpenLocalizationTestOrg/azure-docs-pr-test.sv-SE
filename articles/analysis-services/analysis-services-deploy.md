---
title: "aaaDeploy tooAzure Analysis Services med hjälp av SSDT | Microsoft Docs"
description: "Lär dig hur toodeploy en tabellmodell tooan Azure Analysis Services-servern med hjälp av SSDT."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 5f1f0ae7-11de-4923-a3da-888b13a3638c
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2017
ms.author: owend
ms.openlocfilehash: e3f3771fe32a37b9e0173c274080c647152edd4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-model-from-ssdt"></a>Distribuera en modell från SSDT
När du har skapat en server i din Azure-prenumeration, är du redo toodeploy tooit en tabellmodell-databasen. Du kan använda SQL Server Data Tools (SSDT) toobuild och distribuera en tabellmodell projekt som du arbetar med. 

## <a name="prerequisites"></a>Krav
tooget igång, behöver du:

* **Analysis Services-server** i Azure. Det finns fler toolearn [skapa en Azure Analysis Services-server](analysis-services-create-server.md).
* **Tabellmodell projekt** i SSDT eller en befintlig tabellmodell på hello 1200 eller högre kompatibilitetsnivå. Har du aldrig skapat någon? Försök hello [Adventure Works Internet försäljning tabular modellering kursen](https://msdn.microsoft.com/library/hh231691.aspx).
* **Lokala gateway** – om en eller flera datakällor är lokalt i din organisations nätverk, behöver du tooinstall en [lokala datagateway](analysis-services-gateway.md). hello gateway krävs för din server i molnet hello ansluta tooyour lokala datakällor tooprocess och uppdatera data i hello-modellen.

> [!TIP]
> Innan du distribuerar måste du kontrollera att du kan bearbeta hello data i tabellerna. Klicka på **Modell** > **Bearbeta** > **Bearbeta alla** i SSDT. Om bearbetningen misslyckas kan inte du distribuera.
> 
> 

## <a name="toodeploy-a-tabular-model-from-ssdt"></a>toodeploy en tabellmodell från SSDT

1. Innan du distribuerar måste tooget hello servernamn. I **Azure-portalen** > server > **översikt** > **servernamn**, kopiera hello servernamn.
   
    ![Hämta servernamnet i Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. I SSDT > **Solution Explorer**, högerklicka på hello project > **egenskaper**. I **distribution** > **Server** klistra in hello servernamn.   
   
    ![Klistra in servernamnet i egenskapen för distributionsservern](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. I **Solution Explorer** högerklickar du på **Egenskaper** och klickar sedan på **Distribuera**. Du kanske ange toosign i tooAzure.
   
    ![Distribuera tooserver](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    Distributionens status visas i båda hello utdata och distribuera.
   
    ![Status för distribution](./media/analysis-services-deploy/aas-deploy-status.png)

Det är allt finns tooit!


## <a name="troubleshooting"></a>Felsökning
Om distributionen misslyckas när du distribuerar metadata är förmodligen eftersom SSDT inte kunde ansluta tooyour server. Kontrollera att du kan ansluta tooyour server med hjälp av SSMS. Kontrollera sedan hello Distributionsserver-egenskapen för hello projekt är korrekt.

Om distributionen misslyckas för en tabell är förmodligen eftersom servern inte kunde ansluta tooa datakälla. Om datakällan är lokalt i din organisations nätverk kan vara att tooinstall en [lokala datagateway](analysis-services-gateway.md).

## <a name="next-steps"></a>Nästa steg
Nu när du har tabellmodell distribuerade tooyour servern är klar tooconnect tooit. Du kan [ansluta tooit med SSMS](analysis-services-manage.md) toomanage den. Och du kan [ansluta tooit med hjälp av ett klientverktyg](analysis-services-connect.md) som Power BI, Power BI Desktop eller Excel och börjar skapa rapporter.

