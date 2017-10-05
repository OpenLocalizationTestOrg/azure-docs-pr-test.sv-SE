---
title: "Importera data från en fil till Azure Machine Learning Studio | Microsoft Docs"
description: "Lär dig mer om att överföra filen en utbildning data från hårddisken till Azure Machine Learning Studio. Detta skapar en dataset-modul i arbetsytan."
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
ms.openlocfilehash: 18010864160ceb2d76aea37196e6944bbe426457
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a><span data-ttu-id="2c977-105">Importera träningsdata från en fil på hårddisken till Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="2c977-105">Import training data from a file on your hard drive into Machine Learning Studio</span></span>
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

<span data-ttu-id="2c977-106">Lär dig hur du överför en fil från hårddisken ska användas som utbildningsdata i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="2c977-106">Learn how to upload a data file from your hard drive to use as training data in Azure Machine Learning Studio.</span></span> <span data-ttu-id="2c977-107">Genom att importera filen har du en dataset-modul som är redo för användning i din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="2c977-107">By importing the data file, you have a dataset module ready for use in your workspace.</span></span>

## <a name="steps-to-import-data-from-a-local-file"></a><span data-ttu-id="2c977-108">Steg för att importera data från en lokal fil</span><span class="sxs-lookup"><span data-stu-id="2c977-108">Steps to import data from a local file</span></span>
<span data-ttu-id="2c977-109">Om du vill importera data från en lokal hårddisk måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="2c977-109">To import data from a local hard drive, do the following:</span></span>

1. <span data-ttu-id="2c977-110">Klicka på **+ ny** längst ned i fönstret Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="2c977-110">Click **+NEW** at the bottom of the Machine Learning Studio window.</span></span>
2. <span data-ttu-id="2c977-111">Välj **DATASET** och **från en lokal fil**.</span><span class="sxs-lookup"><span data-stu-id="2c977-111">Select **DATASET** and **FROM LOCAL FILE**.</span></span>
3. <span data-ttu-id="2c977-112">I den **ladda upp en ny datamängd** dialogrutan, bläddra till filen som du vill ladda upp</span><span class="sxs-lookup"><span data-stu-id="2c977-112">In the **Upload a new dataset** dialog, browse to the file you want to upload</span></span>
4. <span data-ttu-id="2c977-113">Ange ett namn, identifiera datatypen och även ange en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="2c977-113">Enter a name, identify the data type, and optionally enter a description.</span></span> <span data-ttu-id="2c977-114">En beskrivning rekommenderas – du kan registrera alla egenskaper om de data som du vill spara när du använder data i framtiden.</span><span class="sxs-lookup"><span data-stu-id="2c977-114">A description is recommended - it allows you to record any characteristics about the data that you want to remember when using the data in the future.</span></span>
5. <span data-ttu-id="2c977-115">Kryssrutan **detta är den nya versionen av en befintlig datauppsättning** kan du uppdatera en befintlig datamängd med nya data.</span><span class="sxs-lookup"><span data-stu-id="2c977-115">The checkbox **This is the new version of an existing dataset** allows you to update an existing dataset with new data.</span></span> <span data-ttu-id="2c977-116">Klicka på den här kryssrutan och sedan ange namnet på en befintlig datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="2c977-116">Click this checkbox and then enter the name of an existing dataset.</span></span>

![Ladda upp en ny datamängd](media/machine-learning-import-data-from-local-file/upload-dataset.png)

<span data-ttu-id="2c977-118">Under överföring visas ett meddelande om att filen överförs.</span><span class="sxs-lookup"><span data-stu-id="2c977-118">During upload, you'll see a message that your file is being uploaded.</span></span> <span data-ttu-id="2c977-119">Överför tiden beror på storleken på dina data och hastigheten på anslutningen till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2c977-119">Upload time depends on the size of your data and the speed of your connection to the service.</span></span> <span data-ttu-id="2c977-120">Om du vet att filen tar lång tid kan göra du andra saker i Machine Learning Studio medan du väntar.</span><span class="sxs-lookup"><span data-stu-id="2c977-120">If you know the file will take a long time, you can do other things inside Machine Learning Studio while you wait.</span></span> <span data-ttu-id="2c977-121">Dock gör stänger webbläsaren dataöverföringen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="2c977-121">However, closing the browser causes the data upload to fail.</span></span>

## <a name="dataset-module-is-ready-for-use"></a><span data-ttu-id="2c977-122">DataSet-modulen är redo för användning</span><span class="sxs-lookup"><span data-stu-id="2c977-122">Dataset module is ready for use</span></span>
<span data-ttu-id="2c977-123">När data överförs lagras i en dataset-modul och är tillgänglig för alla experiment på arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="2c977-123">Once your data is uploaded, it's stored in a dataset module and is available to any experiment in your workspace.</span></span>

<span data-ttu-id="2c977-124">När du redigerar ett experiment kan du hitta datauppsättningar som du har skapat i den **Mina datauppsättningar** listan den **sparade datauppsättningar** lista på modulpaletten.</span><span class="sxs-lookup"><span data-stu-id="2c977-124">When you're editing an experiment, you can find the datasets you've created in the **My Datasets** list under the **Saved Datasets** list in the module palette.</span></span> <span data-ttu-id="2c977-125">Du kan dra och släpp dataset till arbetsytan för experimentet när du vill använda datauppsättningen för ytterligare analys och maskininlärning.</span><span class="sxs-lookup"><span data-stu-id="2c977-125">You can drag and drop the dataset onto the experiment canvas when you want to use the dataset for further analytics and machine learning.</span></span>
