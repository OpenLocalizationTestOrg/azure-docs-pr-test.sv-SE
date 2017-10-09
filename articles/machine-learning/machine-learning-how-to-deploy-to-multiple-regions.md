---
title: "aaaHow toodeploy en webbtjänst toomultiple regioner | Microsoft Docs"
description: "Steg toodeploy (kopiera) en ny webbtjänst tooother regioner."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: cgronlun
ms.assetid: 36c60411-f2db-4ee2-9b66-b1f1d77a8f44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 21fcdb96f118c60ed98b60b1b2df833766c7c8bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-web-service-toomultiple-regions"></a>Hur toodeploy en webbtjänst toomultiple regioner
hello nya Azure-webbtjänster kan du tooeasily distribuera en web service toomultiple regioner utan att behöva flera prenumerationer eller arbetsytor. 

Priser är regionspecifika, därför måste du definiera ett faktureringsavtal för varje region där du ska distribuera hello-webbtjänsten.

## <a name="toocreate-a-plan-in-another-region"></a>toocreate en plan i en annan region
1. Logga in på [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/).
2. Klicka på hello **planer** menyalternativet.
3. Klicka på hello planer över visa sidan, **ny**.
4. Från hello **prenumeration** listrutan, Välj hello prenumeration i vilka hello ny plan kommer att finnas.
5. Från hello **Region** listrutan, Välj en region för hello ny plan. hello planera alternativ för hello valda regionen visas i hello **planera alternativ** på hello-sidan.
6. Från hello **resursgruppen** listrutan, Välj en resursgrupp för hello-plan. För mer information om resursgrupper finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. I **schemanamn** hello-typnamn för hello plan.
8. Under **alternativ**, klicka på hello fakturering nivå för hello ny plan.
9. Klicka på **Skapa**.

## <a name="deploying-hello-web-service-tooanother-region"></a>Distribuera hello web service tooanother region
1. Klicka på hello **Web Services** menyalternativet.
2. Välj hello webbtjänst som du distribuerar tooa ny region.
3. Klicka på **kopiera**.
4. I **Webbtjänstnamn**, ange ett nytt namn för hello-webbtjänsten.
5. I **Web tjänstbeskrivningen**, ange en beskrivning för hello-webbtjänsten.
6. Från hello **prenumeration** listrutan, Välj hello prenumeration i vilka hello nya webbtjänsten kommer att finnas.
7. Från hello **resursgruppen** listrutan, Välj en resursgrupp för hello-webbtjänsten. För mer information om resursgrupper finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
8. Från hello **Region** listrutan, Välj hello region i vilken toodeploy hello-webbtjänsten.
9. Från hello **lagringskonto** listrutan, Välj ett lagringskonto i vilka toostore hello-webbtjänsten.
10. Från hello **prisplan** listrutan en plan i hello region som du valde i steg 8.
11. Klicka på **kopiera**.

