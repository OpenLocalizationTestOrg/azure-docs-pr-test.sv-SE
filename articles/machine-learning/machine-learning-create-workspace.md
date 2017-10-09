---
title: aaaCreate Machine Learning-arbetsytan | Microsoft Docs
description: "Hur toocreate en arbetsyta för Azure Machine Learning Studio"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye;bradsev;ahgyger
ms.openlocfilehash: 178293af222365993fade666124f34269d892325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Skapa och dela en Azure Machine Learning-arbetsyta
Den här menyn länkar tootopics som beskriver hur tooset in hello olika datavetenskap miljöer används av hello Cortana Analytics processen (CAP).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

toouse Azure Machine Learning Studio, behöver du toohave Machine Learning-arbetsytan. Den här arbetsytan innehåller hello verktyg du behöver toocreate, hantera och publicera experiment.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="toocreate-a-workspace"></a>toocreate en arbetsyta
1. Logga in toohello [Azure-portalen](https://portal.azure.com/)

    > [!NOTE]
    > toosign i och skapa en arbetsyta måste du toobe administratör Azure-prenumeration. 
    >
    > 

2. Klicka på **+ nytt**

3. Välj **Intelligence + analys**, klickar du på **Machine Learning-arbetsytan**, klicka på **skapa**

4. Ange arbetsyteinformation om

    - Hej *Arbetsytenamn* kan vara upp too260 tecken, inte slutar med ett blanksteg. hello-namnet får inte innehålla följande tecken:`< > * % & : \ ? + /`
    - Hej *web service-plan* du välja (eller skapa), tillsammans med hello associerade *prisnivån* du väljer, används om du distribuerar webbtjänster från den här arbetsytan.

    ![Skapa en ny arbetsyta](media/machine-learning-create-workspace/create-new-workspace.png)

5. Klicka på **Skapa**

När hello arbetsytan har distribuerats kan öppna du den i Machine Learning Studio.

1. Bläddra tooMachine Learning Studio på [https://studio.azureml.net/](https://studio.azureml.net/).

2. Välj din arbetsyta i hello övre högra hörnet.

    ![Välj arbetsyta](media/machine-learning-create-workspace/open-workspace.png)

3. Klicka på **Mina experiment**.

    ![Öppna experiment](media/machine-learning-create-workspace/my-experiments.png)

Information om hur du hanterar din arbetsyta finns [hantera en Azure Machine Learning-arbetsytan](machine-learning-manage-workspace.md).
Om det uppstår ett problem uppstod när din arbetsyta, se [felsökningsguide: skapa och ansluta tooa Machine Learning-arbetsytan](machine-learning-troubleshooting-creating-ml-workspace.md).


## <a name="sharing-an-azure-machine-learning-workspace"></a>Dela en Azure Machine Learning-arbetsytan
När Machine Learning-arbetsytan har skapats kan erbjuda du användare tooyour arbetsytan tooshare åtkomst tooyour arbetsytan och alla dess experiment, datauppsättningar, bärbara datorer och så vidare. Du kan lägga till användare i en av två roller:

* **Användaren** -arbetsytan användare kan skapa, öppna, ändra och ta bort experiment, datauppsättningar, etc. hello arbetsytan.
* **Ägare** – bjuda in en ägare och ta bort användare hello arbetsytan i tillägg toowhat en användare kan göra.

> [!NOTE]
> hello administratörskontot som skapar hello arbetsyta läggs automatiskt toohello arbetsytan som arbetsyta ägare. Men andra administratörer eller användare i att prenumerationen inte är automatiskt beviljas åtkomst till toohello arbetsytan – du behöver tooinvite dem explicit.
> 
> 

### <a name="tooshare-a-workspace"></a>tooshare en arbetsyta

1. Logga in tooMachine Learning Studio på [https://studio.azureml.net/Home](https://studio.azureml.net/Home)

2. I hello vänstra panelen, klickar du på **inställningar**

3. Klicka på hello **användare** fliken

4. Klicka på **bjuda in fler användare** på hello hello sidans nederkant

    ![Inställningar för Studio](media/machine-learning-create-workspace/settings.png)

5. Ange en eller flera e-postadresser. hello krävs ett giltigt microsoftkonto eller ett organisationskonto (från Azure Active Directory).

6. Välj om du vill tooadd hello användare som ägare eller användare.

7. Klicka på hello **OK** markering knappen.

Varje användare som du lägger till kommer att få ett e-postmeddelande med instruktioner om hur toosign i toohello delas arbetsytan.

> [!NOTE]
> För användare toobe kan toodeploy eller hantera webbtjänster i den här arbetsytan, de måste vara en deltagare eller en administratör i hello Azure-prenumeration. 



