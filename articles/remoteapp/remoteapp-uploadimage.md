---
title: "aaaUpload en anpassad avbildning för Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur tooupload en anpassad avbildning för Azure RemoteApp"
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
ms.openlocfilehash: 6ad40fe58795ece37f4c7900be01bc713938da87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Ladda upp en anpassad avbildning för Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Nu när du har skapat en egeninställd mallavbildning eller har uppdaterats med ändringar, måste du tooupload bildbibliotek det bild tooyour Azure RemoteApp. Följ dessa steg.

## <a name="before-you-start"></a>Innan du börjar
1. Verifiera den anpassade avbildningen uppfyller hello [bildkrav](remoteapp-imagereqs.md) och [programkrav](remoteapp-appreqs.md).
2. Installera hello [Azure PowerShell-modulen](/powershell/azure/overview).

## <a name="step-by-step-on-how-tooupload-custom-image"></a>Steg för steg om hur tooupload anpassad avbildning
1. Öppna Azure-hanteringsportalen och navigera toohello RemoteApp-sidan.
2. På hello **mallavbildningarna** klickar du på **överför** på hello hello sidans nederkant.
3. Ange ett eget namn för bilden och ange hello lagringskontoplatsen. Kontrollera hello plats är hello samma plats som RemoteApp-samling eller en plats där du vill toocreate en.
4. När du uppmanas att hämta hello skriptet tooyour lokala dator.
5. Kopiera hello parametrar i hello text rutan tooyour Urklipp.
6. Öppna en kommandotolk med Windows PowerShell.
7. Utökade Windows PowerShell-fönstret från hello navigera toohello samma katalog där du hämtade hello skript.
8. Klistra in hello kopieras kommando och tryck på **RETUR**.
   
   hello överföringen startar och varaktighet beror på många faktorer, inklusive nätverkshastigheten och hello-bild
9. Om överföra misslyckas på grund av avbrott i nätverket eller saker som de som kan du alltid återuppta hello överföringsprocessen du började. tooresume en överföring kör hello skriptet igen med hello samma kommandorad.

> [!WARNING]
> Ändra aldrig hello överför skript. Specifika kontroller har implementerat tooensure som hello avbildningen uppfyller hello avbildningen krav och programkrav.
> 
> 

## <a name="common-problems"></a>Vanliga problem
* Kontrollera att du använder Windows PowerShell, inte Azure PowerShell. Du behöver tooinstall hello Azure PowerShell-modulen eftersom vissa moduler behövs under hello överföringen.
* Ändra aldrig hello skript, verifieringar finns det för din bekvämlighet.
* Om hello vhd-filen hämtar utelåst under överföring, kopiera hello filen eller flytta den överför för tooa ny plats och försök igen. Det kan finnas vissa Windows-process som hindrar överföringen.  

