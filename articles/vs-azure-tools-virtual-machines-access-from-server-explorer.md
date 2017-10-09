---
title: "aaaAccessing Azure virtuella datorer från Server Explorer | Microsoft Docs"
description: "Få en översikt över hur tooview skapa och hantera virtuella Azure-datorer (VM) i Server Explorer i Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: f8326aed105a64ca558f766d712cc68701f82c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Åtkomst till virtuella Azure-datorer från Server Explorer
Du kan visa information om dina virtuella datorer som finns i Azure med hjälp av Server Explorer i Visual Studio.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Åtkomst till virtuella datorer i Server Explorer
Om du har virtuella datorer som finns i Azure, kan du komma åt dem i Server Explorer. Du måste först logga in tooyour Azure-prenumeration tooview dina mobila tjänster. toosign, öppna hello snabbmenyn för hello Azure nod i Server Explorer och välj **ansluta tooMicrosoft Azure**.

### <a name="tooget-information-about-your-virtual-machines"></a>tooget information om dina virtuella datorer
1. Välj en virtuell dator i Server Explorer, och välj hello F4 viktiga tooshow fönstret dess egenskaper.
   
    hello i den följande tabellen visar vilka egenskaper som är tillgängligt, men de är alla skrivskyddade. toochange dem, använda hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).
   
   | Egenskap | Beskrivning |
   | --- | --- |
   | DNS-namn |hello URL med hello Internetadressen för hello virtuell dator. |
   | Miljö |Hello-värdet för den här egenskapen är alltid produktion för en virtuell dator. |
   | Namn |hello namnet på hello virtuella datorn. |
   | Storlek |hello storlek hello virtuell dator som visar hello mängden minne och diskutrymme som är tillgänglig. Mer information finns i How To: Konfigurera storlekar för virtuella datorer. |
   | Status |Värden är Start, igång, stoppas, Stoppad och hämtar Status. Om hämtning av Status visas är hello aktuell status okänd. hello värden för den här egenskapen skiljer sig från hello-värden som används på hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885). |
   | Prenumerations-ID |hello prenumerations-ID för ditt Azure-konto. Du kan visa den här informationen på hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885) genom att visa hello egenskaperna för en prenumeration. |
2. Välj en slutpunkt-nod och sedan visar hello **egenskaper** fönster.
3. hello följande tabell beskrivs hello tillgängliga egenskaper för slutpunkter, men de är skrivskyddad. tooadd eller redigera hello-slutpunkter för en virtuell dator använda hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885). 
   
   | Egenskap | Beskrivning |
   | --- | --- |
   | Namn |En identifierare för hello slutpunkt. |
   | Privat Port |hello-port för nätverksprogram åtkomst interna tooyour. |
   | Protokoll |hello-protokollet som hello Transportskiktet för den här slutpunkten använder TCP eller UDP. |
   | Offentlig port |hello-port som används för allmän åtkomst tooyour program. |

## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du använder Azure roller i Visual Studio finns [med hjälp av fjärrskrivbord med Azure-roller](vs-azure-tools-remote-desktop-roles.md).

