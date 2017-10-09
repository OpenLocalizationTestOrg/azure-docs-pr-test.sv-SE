---
title: "aaaMount Azure file storage från en Windows Azure-dator | Microsoft Docs"
description: "Lagra filen i hello moln med Azure file storage och montera din molnbaserade filresurs från en Azure virtuell dator (VM)."
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 965f1c1b3f0d07fec6d86f9312a05e02e8ce7fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Använda Azure-filresurser med virtuella Windows-datorer 

Du kan använda Azure-filresurser som ett sätt toostore och filerna från den virtuella datorn. Du kan till exempel lagra ett skript eller en programkonfigurationsfil som du vill att alla virtuella datorer tooshare. I det här avsnittet visar vi du hur toocreate och montera en Azure-filresurs och hur tooupload och hämta filer.

## <a name="connect-tooa-file-share-from-a-vm"></a>Ansluta tooa filresurs från en virtuell dator

Det här avsnittet förutsätter att du redan har en filresurs som du vill tooconnect till. Om du behöver toocreate en finns [skapa en filresurs](#create-a-file-share) senare i det här avsnittet.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. På hello vänstra menyn **lagringskonton**.
3. Välj ditt lagringskonto.
4. I hello **översikt** sidan under **Services**väljer **filer**.
5. Välj en filresurs.
6. Klicka på **Anslut** tooopen en sida som visar hello kommandoradssyntaxen för montering hello filresursen från Windows eller Linux.
7. Markera hello syntaxen för hello kommandot och klistra in den i anteckningar eller någon annanstans där du enkelt kan komma åt den. 
8. Redigera hello syntax tooremove hello inledande ** > ** och Ersätt *[enhetsbeteckning]* med hello enhetsbeteckning (till exempel **Y:**) där du vill ha toomount hello filresurs.
8. Anslut tooyour VM och öppna en kommandotolk.
9. Klistra in hello redigeras anslutning syntax och tryck **RETUR**.
10. När hello anslutningen har skapats kan du få hello-meddelande **hello kommandot har slutförts.**
11. Kontrollera hello anslutningen genom att skriva in hello enhet bokstav tooswitch toothat enhet och skriv sedan **dir** toosee hello innehållet i hello filresurs.



## <a name="create-a-file-share"></a>Skapa en filresurs 
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. På hello vänstra menyn **lagringskonton**.
3. Välj ditt lagringskonto.
4. I hello **översikt** sidan under **Services**väljer **filer**.
5. På hello File Service klickar du på **+ filresurs** toocreate din första filresurs. \
6. Fyll i hello filresursnamn. Filresursnamn kan använda gemena bokstäver, siffror och bindestreck som enda. hello namn kan inte börja med ett bindestreck och du kan inte använda flera bindestreck efter varandra. 
7. Fyll i en gräns för hur stor hello-fil kan vara upp too5120 GB.
8. Klicka på **OK** toodeploy hello filresurs.
   
## <a name="upload-files"></a>Överföra filer
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. På hello vänstra menyn **lagringskonton**.
3. Välj ditt lagringskonto.
4. I hello **översikt** sidan under **Services**väljer **filer**.
5. Välj en filresurs.
6. Klicka på **överför** tooopen hello **ladda upp filer** sidan.
7. Klicka på hello mappen ikonen toobrowse det lokala filsystemet för en fil tooupload.   
8. Klicka på **överför** tooupload hello filen toohello filresurs.

## <a name="download-files"></a>Hämta filer
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. På hello vänstra menyn **lagringskonton**.
3. Välj ditt lagringskonto.
4. I hello **översikt** sidan under **Services**väljer **filer**.
5. Välj en filresurs.
6. Högerklicka på hello-filen och välja **hämta** toodownload den tooyour lokal dator.
   

## <a name="next-steps"></a>Nästa steg

Du kan också skapa och hantera filresurser med hjälp av PowerShell. Mer information finns i [Kom igång med Azure File storage i Windows](../../storage/files/storage-dotnet-how-to-use-files.md).
