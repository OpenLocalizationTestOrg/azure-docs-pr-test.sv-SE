---
title: "Montera Azure file storage från en Windows Azure-dator | Microsoft Docs"
description: "Lagra filen i molnet med Azure file storage och montera din molnbaserade filresurs från en Azure virtuell dator (VM)."
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.openlocfilehash: 8d537bdc882487784baef9f693e4677c76d3bd8d
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/03/2018
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Använda Azure-filresurser med virtuella Windows-datorer 

Du kan använda Azure-filresurser som ett sätt att lagra och komma åt filer från den virtuella datorn. Du kan till exempel lagra ett skript eller en konfigurationsfil för programmet som du vill att alla dina virtuella datorer att dela. I den här artikeln visar vi dig hur du skapar och montera en filresurs som Azure och hur du ladda upp och ner filer.

## <a name="connect-to-a-file-share-from-a-vm"></a>Ansluta till en filresurs från en virtuell dator

Det här avsnittet förutsätter att du redan har en filresurs som du vill ansluta till. Om du behöver skapa en finns [skapa en filresurs](#create-a-file-share) senare i den här artikeln.

1. Logga in på [Azure Portal](https://portal.azure.com).
2. Klicka på den vänstra menyn **lagringskonton**.
3. Välj ditt lagringskonto.
4. I den **översikt** sidan under **Services**väljer **filer**.
5. Välj en filresurs eller klicka på **+ filresurs** att skapa en ny filresurs som ska användas.
6. Klicka på **Anslut** att öppna en sida som visar att kommandoradssyntaxen för att montera filresursen från Windows eller Linux.
7. I **enhetsbeteckning**, Välj den bokstav som du vill använda för att identifiera enheten.
8. Välj vilken syntax du använder och klicka på kopieringsknappen till höger för att kopiera den till Urklipp. Klistra in det någon plats där du enkelt kan komma åt den. 
8. Anslut till den virtuella datorn och öppna en kommandotolk.
9. Klistra in i den redigerade anslutning syntaxen och tryck **RETUR**.
10. När anslutningen har skapats kan du få meddelandet **kommandot har slutförts.**
11. Kontrollera anslutningen genom att skriva in enhetsbeteckningen för att växla till den här enheten och skriver sedan **dir** att se innehållet i filresursen.



## <a name="create-a-file-share"></a>Skapa en filresurs 
1. Logga in på [Azure Portal](https://portal.azure.com).
2. Klicka på den vänstra menyn **lagringskonton**.
3. Välj ditt lagringskonto.
4. I den **översikt** sidan under **Services**väljer **filer**.
5. Tjänsten File-sidan klickar du på **+ filresurs**.
6. Fyll i filresursens namn. Filresursnamn kan använda gemena bokstäver, siffror och bindestreck som enda. Namnet får inte börja med ett bindestreck och du kan inte använda flera bindestreck efter varandra. 
7. Fyll i en gräns för hur stor filen kan vara, upp till 5 120 GB.
8. Klicka på **OK** att skapa filresursen.
   
## <a name="upload-files"></a>Överföra filer
1. Logga in på [Azure Portal](https://portal.azure.com).
2. Klicka på den vänstra menyn **lagringskonton**.
3. Välj ditt lagringskonto.
4. I den **översikt** sidan under **Services**väljer **filer**.
5. Välj en filresurs.
6. Klicka på **överför** att öppna den **ladda upp filer** sidan.
7. Klicka på mappikonen för att bläddra i det lokala filsystemet för en fil som ska överföras.   
8. Klicka på **överför** att överföra filen till filresursen.

## <a name="download-files"></a>Hämta filer
1. Logga in på [Azure Portal](https://portal.azure.com).
2. Klicka på den vänstra menyn **lagringskonton**.
3. Välj ditt lagringskonto.
4. I den **översikt** sidan under **Services**väljer **filer**.
5. Välj en filresurs.
6. Högerklicka på filen och välj **hämta** att ladda ned den till din lokala dator.
   

## <a name="next-steps"></a>Nästa steg

Du kan också skapa och hantera filresurser med hjälp av PowerShell. Mer information finns i [Kom igång med Azure File storage i Windows](../../storage/files/storage-dotnet-how-to-use-files.md).
