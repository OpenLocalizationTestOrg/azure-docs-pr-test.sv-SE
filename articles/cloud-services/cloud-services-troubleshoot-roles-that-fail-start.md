---
title: aaaTroubleshoot roller som inte toostart | Microsoft Docs
description: "Här följer några vanliga orsaker till varför en tjänst i molnet roll misslyckas toostart. Det finns också lösningar toothese problem."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: e2fbecb08a10984add79dfc74e73de6869bb314f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-service-roles-that-fail-toostart"></a>Felsöka Cloud Service-roller som inte toostart
Här följer några vanliga problem och lösningar relaterade tooAzure molntjänster roller som inte toostart.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>DLL-filer som saknas eller beroenden
Sluta att svara roller och roller som reglering mellan **initierar**, **upptagen**, och **stoppar** tillstånd kan orsakas av DLL-filer eller sammansättningar saknas.

Symtom på DLL: er eller sammansättningar saknas kan vara:

* Din rollinstans Bläddra igenom **initierar**, **upptagen**, och **stoppar** tillstånd.
* Rollinstansen har flyttats för**klar** men hello sidan visas inte om du navigerar tooyour webbprogram.

Det finns flera metoder för att undersöka problemen.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Diagnostisera problem med saknas DLL-filen i en webbroll
När du navigerar tooa webbplats som distribueras i en webbroll och hello webbläsaren visar en server fel liknande toohello följande, kan det indikera att en DLL-fil saknas.

![Serverfel i programmet '/'.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Diagnostisera problem genom att stänga av anpassade fel
Mer information om fel kan visas genom att konfigurera hello web.config för hello web rollen tooset hello anpassade fel läge tooOff och omdistribuera hello-tjänsten.

tooview slutföra fler fel utan att använda Fjärrskrivbord:

1. Öppna hello lösning i Microsoft Visual Studio.
2. I hello **Solution Explorer**letar du upp hello web.config-filen och öppna den.
3. Leta upp hello system.web-avsnittet i filen web.config för hello och Lägg till följande rad hello:

    ```xml
    <customErrors mode="Off" />
    ```
4. Spara hello-filen.
5. Paketera och distribuera hello-tjänsten.

När hello-tjänsten är nytt, visas ett felmeddelande med hello namnet hello sammansättningen eller DLL-filen saknas.

## <a name="diagnose-issues-by-viewing-hello-error-remotely"></a>Diagnostisera problem genom att visa hello fel via fjärranslutning
Du kan använda Fjärrskrivbord tooaccess hello-rollen och visa fullständig felinformation via fjärranslutning. Använd följande steg tooview hello fel med hjälp av fjärrskrivbord hello:

1. Kontrollera att Azure SDK 1.3 eller senare är installerad.
2. Under hello distribution av hello lösning med hjälp av Visual Studio, välja för ”konfigurera anslutningar till fjärrskrivbord...”. Mer information om hur du konfigurerar hello fjärrskrivbordsanslutning finns [med hjälp av fjärrskrivbord med Azure-roller](../vs-azure-tools-remote-desktop-roles.md).
3. I hello Microsoft klassiska Azure-portalen när hello instans visar statusen **klar**, klicka på någon av hello rollinstanser.
4. Klicka på hello **Anslut** ikon i hello **fjärråtkomst** område i hello menyfliksområdet.
5. Logga in toohello virtuell dator med hjälp av hello-autentiseringsuppgifter som angavs under hello fjärrskrivbord konfiguration.
6. Öppna ett kommandofönster.
7. Skriv `IPconfig`.
8. Anteckna värdet för hello IPV4-adress.
9. Öppna Internet Explorer.
10. Ange hello-adress och hello namn på hello webbprogram. Till exempel `http://<IPV4 Address>/default.aspx`.

Navigera toohello returnerar webbplats nu mer utförlig felmeddelanden:

* Serverfel i programmet '/'.
* Beskrivning: Ett ohanterat undantag uppstod under hello körning av hello aktuella webbegäran. Granska hello stackspårning för mer information om felet hello och ursprung i hello kod.
* Undantagsinformation: System.IO.FIleNotFoundException: Det gick inte att läsa in filen eller sammansättningen ' Microsoft.WindowsAzure.StorageClient, Version = 1.1.0.0 kultur = neutral, PublicKeyToken = 31bf856ad364e35' eller en av dess beroenden. hello går inte att hitta angivna hello-filen.

Exempel:

![Explicit serverfel i programmet '/'](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-hello-compute-emulator"></a>Diagnostisera problem med hjälp av hello beräkningsemulatorn
Du kan använda hello Microsoft Azure compute emulator toodiagnose och felsökning av problem med saknade beroenden och web.config-fel.

För bästa resultat i den här metoden för diagnos bör du använda en dator eller virtuell dator som har en ren installation av Windows. toobest simulera hello Azure-miljön kan använda Windows Server 2008 R2 x64.

1. Installera hello fristående versionen av hello [Azure SDK](https://azure.microsoft.com/downloads/).
2. Skapa hello molntjänstprojekt på hello utvecklingsdator.
3. Navigera toohello hello molntjänstprojekt bin\debug mapp i Utforskaren.
4. Kopiera hello .csx mapp- och .cscfg-filen toohello datorn att du använder toodebug hello problem.
5. Öppna en Azure SDK-kommandotolksfönster och Skriv på hello ren maskin, `csrun.exe /devstore:start`.
6. I hello kommandotolk, Skriv `run csrun <path too.csx folder> <path too.cscfg file> /launchBrowser`.
7. När hello roll startar visas detaljerad felinformation i Internet Explorer. Du kan också använda standard Windows felsökningsverktyg toofurther diagnostisera hello problem.

## <a name="diagnose-issues-by-using-intellitrace"></a>Diagnostisera problem med hjälp av IntelliTrace
Du kan använda för arbetaren och web-roller som använder .NET Framework 4 [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), som finns i Microsoft Visual Studio Enterprise.

Följ dessa steg toodeploy hello-tjänsten med IntelliTrace aktiverad:

1. Bekräfta att Azure SDK 1.3 eller senare är installerat.
2. Distribuera hello lösning med hjälp av Visual Studio. Under distributionen, kontrollera hello **aktivera IntelliTrace för .NET 4 roller** kryssrutan.
3. När hello instans startar öppna hello **Server Explorer**.
4. Expandera hello **Azure\\molntjänster** nod och leta upp hello-distribution.
5. Expandera hello-distribution tills du ser hello rollinstanser. Högerklicka på någon av hello instanser.
6. Välj **visa IntelliTrace-loggar**. Hej **IntelliTrace sammanfattning** öppnas.
7. Leta upp hello undantag avsnitt i hello sammanfattning. Om det finns undantag, hello-avsnittet är märkt **Undantagsdata**.
8. Expandera hello **Undantagsdata** och leta efter **System.IO.FileNotFoundException** fel liknande toohello följande:

![Undantagsdata saknas filen eller sammansättningen](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Adressen saknas DLL: er och sammansättningar
tooaddress saknas DLL-fil och montering fel, Följ dessa steg:

1. Öppna hello lösningen i Visual Studio.
2. I **Solution Explorer**öppnar hello **referenser** mapp.
3. Klicka på hello sammansättningen som identifierats i hello-fel.
4. I hello **egenskaper** rutan hitta **kopiera lokala egenskapen** och ange hello värdet för**SANT**.
5. Omdistribuera hello-Molntjänsten.

När du har kontrollerat att alla fel har åtgärdats, kan du distribuera hello tjänst utan att kontrollera hello **aktivera IntelliTrace för .NET 4 roller** kryssrutan.

## <a name="next-steps"></a>Nästa steg
Visa mer [felsökning artiklar](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) för molntjänster.

toolearn hur tootroubleshoot moln rolltjänst problem med hjälp av Azure PaaS diagnostikdata för datorn, se [Kevin Williamson bloggserie](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
