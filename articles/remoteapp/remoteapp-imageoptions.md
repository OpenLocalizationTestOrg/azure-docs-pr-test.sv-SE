---
title: aaaCreate en Azure RemoteApp-avbildning | Microsoft Docs
description: "Lär dig mer om hello alternativen för att skapa avbildningar för Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: cb0f9424-6185-45a1-abe9-c23f1edf34f2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 54e63b6fa13addfcda96ce581910e1ac48d91e70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-remoteapp-image"></a><span data-ttu-id="01329-103">Skapa en Azure RemoteApp-avbildning</span><span class="sxs-lookup"><span data-stu-id="01329-103">Create an Azure RemoteApp image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="01329-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="01329-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="01329-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="01329-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="01329-106">Azure RemoteApp använder avbildningar toohold hello appar som du delar med dina användare.</span><span class="sxs-lookup"><span data-stu-id="01329-106">Azure RemoteApp uses images toohold hello apps that you share with your users.</span></span> <span data-ttu-id="01329-107">(Vi ta avbildningen och använda den toocreate VMs - som har vad hello användare komma åt när de loggar in på Azure RemoteApp.) toocreate en Azure RemoteApp-samling med ditt val av program, om det är molnet eller hybrid, du genom att skapa en bild med de program som är installerade.</span><span class="sxs-lookup"><span data-stu-id="01329-107">(We take your image and use it toocreate VMs - that's what hello users access when they sign into Azure RemoteApp.) toocreate an Azure RemoteApp collection with your choice of applications, whether it is cloud or hybrid, you  start by creating an image with those applications installed.</span></span> <span data-ttu-id="01329-108">Skapa sedan en samling som använder avbildningen, och tilldela användare toohello samling och publicera appar toothose användare.</span><span class="sxs-lookup"><span data-stu-id="01329-108">Then, create a collection that uses that image, assign users toohello collection, and publish apps toothose users.</span></span>

<span data-ttu-id="01329-109">Du har flera alternativ för att skapa eller använda avbildningar.</span><span class="sxs-lookup"><span data-stu-id="01329-109">You have several options for creating or using images.</span></span> <span data-ttu-id="01329-110">grundläggande hello [krav](remoteapp-imagereqs.md) för en bild är att den kör Windows Server 2012 R2 och ha hello Remote värd för fjärrskrivbordssession (RDSH)-rollen installerad.</span><span class="sxs-lookup"><span data-stu-id="01329-110">hello basic [requirement](remoteapp-imagereqs.md) for an image is that it run Windows Server 2012 R2 and have hello Remote Desktop Session Host (RDSH) role installed.</span></span> <span data-ttu-id="01329-111">Hur du får som är där det blir intressant.</span><span class="sxs-lookup"><span data-stu-id="01329-111">How you get that is where things get interesting.</span></span>

<span data-ttu-id="01329-112">Har du följande alternativ när det gäller tooimages hello:</span><span class="sxs-lookup"><span data-stu-id="01329-112">You have hello following options when it comes tooimages:</span></span>

* <span data-ttu-id="01329-113">Du kan importera och använda en [bilden baserat på en virtuell dator i Azure](remoteapp-image-on-azurevm.md).</span><span class="sxs-lookup"><span data-stu-id="01329-113">You can import and use an [image based on an Azure virtual machine](remoteapp-image-on-azurevm.md).</span></span> <span data-ttu-id="01329-114">Det här är bra för av branschspecifika appar som kräver anpassade inställningar.</span><span class="sxs-lookup"><span data-stu-id="01329-114">This is good for line-of-business apps that require custom settings.</span></span> <span data-ttu-id="01329-115">Du kan anpassa hello avbildningen toowork för hello app.</span><span class="sxs-lookup"><span data-stu-id="01329-115">You can customize hello image toowork for hello app.</span></span>
* <span data-ttu-id="01329-116">Du kan [skapa och ladda upp en anpassad avbildning](remoteapp-create-custom-image.md).</span><span class="sxs-lookup"><span data-stu-id="01329-116">You can [create and upload a custom image](remoteapp-create-custom-image.md).</span></span> <span data-ttu-id="01329-117">Det här är bra om du redan har en avbildning som du använder för din lokala Remote Desktop Services-distribution.</span><span class="sxs-lookup"><span data-stu-id="01329-117">This is good if you already have an image that you use for your on-premises Remote Desktop Services deployment.</span></span>
* <span data-ttu-id="01329-118">Du kan använda någon av hello [mallavbildningarna](remoteapp-images.md) ingår i RemoteApp-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="01329-118">You can use one of hello [template images](remoteapp-images.md) included in your RemoteApp subscription.</span></span> <span data-ttu-id="01329-119">Dessa avbildningar skapas och underhålls av hello RemoteApp-teamet och innehåller vissa standard program (till exempel hello Office-paket) som du kan göra tillgängliga tooyour användare.</span><span class="sxs-lookup"><span data-stu-id="01329-119">These images are created and maintained by hello RemoteApp team and contain some standard applications (like hello Office suite) that you can make available tooyour users.</span></span> <span data-ttu-id="01329-120">Observera att endast hello Office 365 Pro Plus avbildningen kan användas i en Produktionsinställning.</span><span class="sxs-lookup"><span data-stu-id="01329-120">Note that only hello Office 365 Pro Plus image can be used in a production setting.</span></span>

<span data-ttu-id="01329-121">Oavsett var du får en avbildning eller hur du skapar, kommer du att du förstår hello toomake [appkrav](remoteapp-appreqs.md) tooensure som din app fungerar bra i RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="01329-121">Regardless of where you get your image or how you create it, you'll want toomake sure you understand hello [app requirements](remoteapp-appreqs.md) tooensure that your app works well in RemoteApp.</span></span> <span data-ttu-id="01329-122">Hello nästa steg är toocreate en [moln](remoteapp-create-cloud-deployment.md) eller [hybrid](remoteapp-create-hybrid-deployment.md) samling.</span><span class="sxs-lookup"><span data-stu-id="01329-122">Then, hello next step is toocreate a [cloud](remoteapp-create-cloud-deployment.md) or [hybrid](remoteapp-create-hybrid-deployment.md) collection.</span></span>

