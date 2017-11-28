---
title: "aaaImport data från en fil i Azure Machine Learning Studio | Microsoft Docs"
description: "Lär dig hur tooupload en träningsdata fil från din hårddisk tooAzure Machine Learning Studio. Detta skapar en dataset-modul i hello arbetsyta."
keywords: "Importera data, dataformatet, datatyper, datakällor, träningsdata"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;bradsev
ms.openlocfilehash: 636facd9042145382c953a1c75969149ede6f6fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a><span data-ttu-id="90190-105">Importera träningsdata från en fil på hårddisken till Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="90190-105">Import training data from a file on your hard drive into Machine Learning Studio</span></span>
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

<span data-ttu-id="90190-106">Lär dig hur tooupload en fil från din hårddisk toouse som utbildningsdata i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="90190-106">Learn how tooupload a data file from your hard drive toouse as training data in Azure Machine Learning Studio.</span></span> <span data-ttu-id="90190-107">Genom att importera hello datafilen, har du en dataset-modul som är redo för användning i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="90190-107">By importing hello data file, you have a dataset module ready for use in your workspace.</span></span>

## <a name="steps-tooimport-data-from-a-local-file"></a><span data-ttu-id="90190-108">Steg tooimport data från en lokal fil</span><span class="sxs-lookup"><span data-stu-id="90190-108">Steps tooimport data from a local file</span></span>
<span data-ttu-id="90190-109">tooimport data från en lokal hårddisk hello följande:</span><span class="sxs-lookup"><span data-stu-id="90190-109">tooimport data from a local hard drive, do hello following:</span></span>

1. <span data-ttu-id="90190-110">Klicka på **+ ny** längst hello hello Machine Learning Studio-fönstret.</span><span class="sxs-lookup"><span data-stu-id="90190-110">Click **+NEW** at hello bottom of hello Machine Learning Studio window.</span></span>
2. <span data-ttu-id="90190-111">Välj **DATASET** och **från en lokal fil**.</span><span class="sxs-lookup"><span data-stu-id="90190-111">Select **DATASET** and **FROM LOCAL FILE**.</span></span>
3. <span data-ttu-id="90190-112">I hello **ladda upp en ny datamängd** dialogrutan Bläddra toohello-fil som du vill tooupload</span><span class="sxs-lookup"><span data-stu-id="90190-112">In hello **Upload a new dataset** dialog, browse toohello file you want tooupload</span></span>
4. <span data-ttu-id="90190-113">Ange ett namn, identifiera hello datatyp och även ange en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="90190-113">Enter a name, identify hello data type, and optionally enter a description.</span></span> <span data-ttu-id="90190-114">En beskrivning rekommenderas – kan du toorecord alla egenskaper om hello data som du vill tooremember när du använder hello data i framtiden hello.</span><span class="sxs-lookup"><span data-stu-id="90190-114">A description is recommended - it allows you toorecord any characteristics about hello data that you want tooremember when using hello data in hello future.</span></span>
5. <span data-ttu-id="90190-115">Hej kryssrutan **detta är hello ny version av en befintlig datauppsättning** kan du tooupdate en befintlig datamängd med nya data.</span><span class="sxs-lookup"><span data-stu-id="90190-115">hello checkbox **This is hello new version of an existing dataset** allows you tooupdate an existing dataset with new data.</span></span> <span data-ttu-id="90190-116">Klicka på den här kryssrutan och ange hello namnet på en befintlig datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="90190-116">Click this checkbox and then enter hello name of an existing dataset.</span></span>

![Ladda upp en ny datamängd](media/machine-learning-import-data-from-local-file/upload-dataset.png)

<span data-ttu-id="90190-118">Under överföring visas ett meddelande om att filen överförs.</span><span class="sxs-lookup"><span data-stu-id="90190-118">During upload, you'll see a message that your file is being uploaded.</span></span> <span data-ttu-id="90190-119">Överför tiden beror på hello storleken på dina data och hello hastigheten på anslutningstjänst toohello.</span><span class="sxs-lookup"><span data-stu-id="90190-119">Upload time depends on hello size of your data and hello speed of your connection toohello service.</span></span> <span data-ttu-id="90190-120">Om du vet hello filen tar lång tid kan göra du andra saker i Machine Learning Studio medan du väntar.</span><span class="sxs-lookup"><span data-stu-id="90190-120">If you know hello file will take a long time, you can do other things inside Machine Learning Studio while you wait.</span></span> <span data-ttu-id="90190-121">Dock gör stänger hello webbläsare hello data överför toofail.</span><span class="sxs-lookup"><span data-stu-id="90190-121">However, closing hello browser causes hello data upload toofail.</span></span>

## <a name="dataset-module-is-ready-for-use"></a><span data-ttu-id="90190-122">DataSet-modulen är redo för användning</span><span class="sxs-lookup"><span data-stu-id="90190-122">Dataset module is ready for use</span></span>
<span data-ttu-id="90190-123">När data överförs lagras i en dataset-modul och är tillgängliga tooany experiment på arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="90190-123">Once your data is uploaded, it's stored in a dataset module and is available tooany experiment in your workspace.</span></span>

<span data-ttu-id="90190-124">När du redigerar ett experiment kan du hitta hello datauppsättningar som du har skapat i hello **Mina datauppsättningar** listan under hello **sparade datauppsättningar** hello modulpaletten-listan.</span><span class="sxs-lookup"><span data-stu-id="90190-124">When you're editing an experiment, you can find hello datasets you've created in hello **My Datasets** list under hello **Saved Datasets** list in hello module palette.</span></span> <span data-ttu-id="90190-125">Du kan dra och släpp hello dataset till hello experimentet när du vill toouse hello dataset för ytterligare analys och maskininlärning.</span><span class="sxs-lookup"><span data-stu-id="90190-125">You can drag and drop hello dataset onto hello experiment canvas when you want toouse hello dataset for further analytics and machine learning.</span></span>
