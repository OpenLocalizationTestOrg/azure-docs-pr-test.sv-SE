---
title: "Ladda upp en anpassad avbildning för Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur du laddar upp en anpassad avbildning för Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: 299e0510-1a6b-4fdf-914a-3631b061a360
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: ericor
ms.openlocfilehash: 5a235fac88d6e95ea294bda197641108acb4a09f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Ladda upp en anpassad avbildning för Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Nu när du har skapat en egeninställd mallavbildning eller har uppdaterats med ändringar som du behöver överför avbildningen till din Azure RemoteApp-bildbibliotek. Följ dessa steg.

## <a name="before-you-start"></a>Innan du börjar
1. Verifiera den anpassade avbildningen uppfyller de [bildkrav](remoteapp-imagereqs.md) och [programkrav](remoteapp-appreqs.md).
2. Installera den [Azure PowerShell-modulen](/powershell/azure/overview).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Steg för steg om hur du överför anpassad avbildning
1. Öppna Azure-hanteringsportalen och gå till sidan RemoteApp.
2. På den **mallavbildningarna** klickar du på **överför** längst ned på sidan.
3. Ange ett eget namn för bilden och ange kontot lagringsplatsen. Se till att platsen är samma plats som RemoteApp-samling eller en plats där du vill skapa en.
4. När du uppmanas, hämta skriptet till din lokala dator.
5. Kopiera kommandoparametrarna i textrutan till Urklipp.
6. Öppna en kommandotolk med Windows PowerShell.
7. Gå till samma katalog där du har hämtat skriptet från en upphöjd Windows PowerShell-fönstret.
8. Klistra in den kopierade kommando och tryck på **RETUR**.
   
   Överföringen startar och varaktighet beror på många faktorer, inklusive nätverkshastigheten och storlek
9. Om överföra misslyckas på grund av avbrott i nätverket eller saker som de som kan du alltid återuppta överföringen du började. Kör skriptet igen med samma kommandorad för att återuppta en överföring.

> [!WARNING]
> Ändra aldrig överföringsskriptet. Specifika kontroller har genomförts för att säkerställa att avbildningen uppfyller avbildningen kraven och programkrav.
> 
> 

## <a name="common-problems"></a>Vanliga problem
* Kontrollera att du använder Windows PowerShell, inte Azure PowerShell. Du måste installera Azure PowerShell-modulen eftersom vissa moduler behövs under överföringen.
* Ändra aldrig skriptet, verifieringar finns det för din bekvämlighet.
* Om vhd-filen hämtar utelåst under överföring, kopiera filen eller flytta den till en ny överför plats och försök igen. Det kan finnas vissa Windows-process som hindrar överföringen.  

