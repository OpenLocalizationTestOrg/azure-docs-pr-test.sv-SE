---
title: "aaaMigrate användardata från Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur toomigrate användardata till och från Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d7e4fbf1-cb42-4430-94a0-ed6d4676fc86
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: aefc6ccc2c6173754acf6cad06102f27c8cb1d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-data-into-and-out-of-azure-remoteapp"></a>Hur toomigrate data till och från Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Du kan använda många olika verktyg och metoder tootransfer [användardata](remoteapp-upd.md) in och ut från Azure RemoteApp. Här följer några metoder:

* Kopiera och klistra in med hjälp av delning av Urklipp
* Kopiera filer och data tooa-filserver
* Kopiera filerna tooOneDrive för företag via en webbläsare
* Kopiera filerna med hjälp av omdirigering

> [!NOTE]
> Du kan inte aktivera hello OneDrive för företag eller konsumenten sync-agenter – de [stöds inte](remoteapp-onedrive.md) i Azure RemoteApp.
> 
> 

## <a name="use-copy-and-paste-in-file-explorer"></a>Kopiera och klistra in i Utforskaren
Kopiera och klistra in Urklipp hello är aktiverat i RemoteApp-distributioner [som standard](remoteapp-redirection.md). Detta gör att användarna kopiera filer mellan sina lokala datorer och RemoteApp-appar. Ofta via hello normala av appar i RemoteApp-har användare sparat filer tootheir UPD: ar - flytta att data ut från RemoteApp är enkel:

1. [Publicera Utforskaren som en app](remoteapp-publish.md) i RemoteApp-samling. (Observera att detta är en administrativ aktivitet.)
2. Dirigera användare toolaunch hello Utforskaren appen som du har publicerat och toouse som toocopy och klistra in filer både till deras UPD och från den.

## <a name="upload-files-and-data-tooa-file-server-by-using-standard-network-file-copy"></a>Överföra filer och data tooa filserver med hjälp av standardnätverk filkopiering
Ofta organisationer använda servrar toostore allmänna fildata. Om du vet hello-servernamn eller plats, användarna Bläddra hello lokalt nätverk för hello-servern och kopiera sina filer, mycket ut som ovan. Igen du vill toopublish Utforskaren tooRemoteApp och dela den med dina användare.

> [!NOTE]
> hello filservern måste finnas på hello dirigerbara nätverk RemoteApp har distribuerats till.
> 
> 

## <a name="copy-files-tooonedrive-for-business"></a>Kopiera filerna tooOneDrive för företag
Även om du inte aktivera hello OneDrive för företag sync agent i RemoteApp, kan du fortfarande kopiera filer från UPD-tooOneDrive för företag via en webbläsare. 

1. Publicera Utforskaren tooRemoteApp och sedan ber du användarna tooaccess hello filer via appen. 
2. Det är den enklaste tootransfer filer om de komprimeras, så att användarna ska skapa en ZIP-fil som innehåller alla hello filer toomove tooOneDrive för företag.
3. Be användare toogo toohello Office 365-portalen och gå sedan tooOneDrive och överför hello ZIP-filen.

## <a name="copy-files-by-using-drive-redirection"></a>Kopiera filerna med hjälp av omdirigering
Om du har aktiverat [omdirigering av](remoteapp-redirection.md), du redan har skapat en mappad enhet för dina användare. I det här fallet de zip sina filer på hello omdirigerad enhet och spara dem tootheir lokala dator.

## <a name="how-administrators-can-export-data"></a>Hur administratörer kan använda för att exportera data

Tillämpar för Azure RemoteApp kan exportera alla användarprofil-diskar (UPD) för alla samlingar i prenumerationen-tooAzure lagring med hjälp av Azure PowerShell-cmdleten Export-AzureRemoteAppUserDisk.  Det finns ingen möjlighet tooselect enskilda UPD'S.  När hello PowerShell-kommandot utförs kommer varje användare disk vara 50gb fasta diskstorleken och exporterade tooAzure för lagring.  Kostnader för Azure storage påförs direkt för den här lagring.  När du kör detta kommando Se till att finns det inga sessioner som på annat sätt hello exportera misslyckas.

UPDS för domänanslutna Azure RemoteApp-distributioner kan bara användas igen i en RDS-distribution, kopplade icke-domän-distributioner kan inte användas.  Om dessa diskar kommer att användas i en RDS-distribution rekommenderar vi toouse vår [automatiserade skript](https://github.com/arcadiahlyy/aramigration) som ska exportera, konvertera och importera hello UPDS till en RDS-distribution.

