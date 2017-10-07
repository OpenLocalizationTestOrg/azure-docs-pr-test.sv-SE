---
title: aaaConnect tooAzure Analysis Services med Excel | Microsoft Docs
description: "Lär dig hur tooconnect tooan Azure Analysis Services-servern med hjälp av Excel."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 175b83e7d936716a626aa4b3bf22b5598bb983d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-excel"></a>Anslut till Excel

När du har skapat en server i Azure och distribuerat en tabellmodell tooit, du är klar tooconnect och utforska data.


## <a name="connect-in-excel"></a>Ansluta i Excel

Ansluter tooa server i Excel stöds med hämta Data i Excel 2016. Anslutning med hjälp av hello av guiden Importera tabell i Power Pivot stöds inte. 

**tooconnect i Excel 2016**

1. I Excel 2016 på hello **Data** band klickar du på **hämta externa Data** > **från andra källor** > **från Analysis Services** .

2. I hello guiden i **servernamn**, ange hello servernamn inklusive protokoll och URI. I **logga in autentiseringsuppgifter**väljer **Använd hello följande användarnamn och lösenord**, och skriv sedan hello organisationens namn, till exempel nancy@adventureworks.com, och lösenord.

    ![Ansluta från Excel-inloggning](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. I **Välj databas och tabell**välja hello databas och modell eller perspektiv och klicka sedan på **Slutför**.
   
    ![Ansluta från Excel väljer modellen](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Se även
[Klientbibliotek](analysis-services-data-providers.md)   
[Hantera servern](analysis-services-manage.md)     


