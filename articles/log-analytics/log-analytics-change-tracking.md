---
title: "aaaTrack ändringar med Azure Log Analytics | Microsoft Docs"
description: "hello ändringsspårning lösning i logganalys hjälper dig att identifiera program- och Windows-tjänst-ändringar som sker i din miljö."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2bb1938caad25101e167927200ac3ef495120fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="track-software-changes-in-your-environment-with-hello-change-tracking-solution"></a>Spåra ändringar av programvaran i din miljö med hello ändringsspårning lösning

![Ändra spårning symbol](./media/log-analytics-change-tracking/change-tracking-symbol.png)

Den här artikeln hjälper dig att använda hello ändringsspårning lösning i logganalys tooeasily identifiera ändringar i din miljö. hello lösning spårar ändringar tooWindows och Linux-program, Windows-filer och registernycklar, Windows-tjänster och Linux-daemon. Identifierar konfigurationsändringar kan hjälpa dig hitta operativa problem.

Du installerar hello lösning tooupdate hello typ av agenten som är installerad. Ändringar tooinstalled programvara, tjänster för Windows och Linux-Daemon på hello övervakade servrar är skrivskyddade. Hello informationen skickas sedan toohello logganalys-tjänsten i hello molnet för bearbetning. Logik är tillämpade toohello mottagna data och hello Molntjänsten innehåller hello-data. Genom att använda hello information på hello ändringsspårning instrumentpanelen kan se du enkelt hello ändringar som gjorts i serverinfrastrukturen.

## <a name="installing-and-configuring-hello-solution"></a>Installera och konfigurera hello lösning
Använd följande information tooinstall hello och konfigurera hello lösning.

* Du måste ha en [Windows](log-analytics-windows-agents.md), [Operations Manager](log-analytics-om-agents.md), eller [Linux](log-analytics-linux-agents.md) agenten på varje dator där du vill att toomonitor ändringar.
* Lägg till hello ändringsspårning lösning tooyour OMS-arbetsyta från hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview). Du kan också lägga till hello-lösning med hjälp av hello information i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md). Det krävs ingen ytterligare konfiguration.

### <a name="configure-linux-files-tootrack"></a>Konfigurera tootrack för Linux-filer
Använd hello följa steg tooconfigure filer tootrack på Linux-datorer.

1. I hello OMS-portalen klickar du på **inställningar** (hello växeln symbol).
2. På hello **inställningar** klickar du på **Data**, och klicka sedan på **Linux File Tracking**.
3. Ange hello hela sökvägen, inklusive hello filnamn hello fil att tootrack och klicka sedan på hello under Linux filen ändringsspårning, **Lägg till** symbolen. Till exempel: ”/etc/*.conf”
4. Klicka på **Spara**.  

> [!NOTE]
> Linux-filen spårning har ytterligare funktioner, inklusive directory spårning, recrusion via kataloger och jokertecken spårning.

### <a name="configure-windows-files-tootrack"></a>Konfigurera Windows-filer tootrack
Använd hello följa steg tooconfigure filer tootrack på Windows-datorer.

1. I hello OMS-portalen klickar du på **inställningar** (hello växeln symbol).
2. På hello **inställningar** klickar du på **Data**, och klicka sedan på **Windows File Tracking**.
3. Ange hello hela sökvägen, inklusive hello filnamn hello fil att tootrack och klicka sedan på hello under Windows-filen ändringsspårning, **Lägg till** symbolen. Till exempel: C:\Program Files (x86) \Internet Explorer\iexplore.exe eller C:\Windows\System32\drivers\etc\hosts.
4. Klicka på **Spara**.  
   ![Windows-filen för ändringsspårning](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="configure-windows-registry-keys-tootrack"></a>Konfigurera Windows-registret nycklar tootrack
Använd hello följa steg tooconfigure registret nycklar tootrack på Windows-datorer.

1. I hello OMS-portalen klickar du på **inställningar** (hello växeln symbol).
2. På hello **inställningar** klickar du på **Data**, och klicka sedan på **spårning av Windows-registret**.
3. Ange hello hela nyckeln som du vill tootrack och klicka sedan på hello under Windows-registret ändringsspårning, **Lägg till** symbolen.
4. Klicka på **Spara**.  
   ![Windows-registret med ändringsspårning](./media/log-analytics-change-tracking/windows-registry-change-tracking.png)

### <a name="explanation-of-linux-file-collection-properties"></a>Förklaring av Linux samling filegenskaper
1. **Typ**
   * **Filen** (rapportera filens metadata - storlek, ändringsdatum, hash, etc.)
   * **Directory** (rapporten directory metadata - storlek, ändringsdatum osv.)
2. **Länkar** (hantera Linux symlink refererar till tooother filer eller kataloger)
   * **Ignorera** (ignorera symlinks under recurions toonot inkludera hello filer och kataloger refererar till)
   * **Följ** (Följ hello symlinks under rekursion tooalso inkludera hello filer och kataloger refererar till)
   * **Hantera** (Följ hello symlinks och ändra hello behandling av returnerade innehåll)

   > [!NOTE]   
   > Hej ”hantera” länkar alternativet rekommenderas inte. Filen innehållshämtning stöds inte.

3. **Recurse** (Recurse via mappen nivåer och spåra alla filer som uppfyller hello sökvägen)
4. **Sudo** (aktivera åtkomst till filer eller kataloger som behöver sudo-behörighet)

### <a name="limitations"></a>Begränsningar
hello ändringsspårning lösningen stöder för närvarande inte hello följande objekt:

* Mappar (kataloger) för spårning av Windows-filen
* Rekursion för Windows-spårning
* Jokertecken för spårning av Windows-filen
* Sökvägsvariabler
* Network file system
* Filinnehåll

Andra begränsningar:

* Hej **maximal filstorlek** kolumnen och värdena är inte används i hello aktuella implementeringen.
* Om du samlar in mer än 2 500 filer i hello 30 minuter insamlingscykel kan lösningens prestanda försämras.
* När nätverkstrafiken är hög, kan ändra poster ta tooa maximalt sex timmar toodisplay.
* Om du ändrar hello konfiguration när en dator är avstängd kan hello datorn efter filändringar som hörde toohello tidigare konfiguration.

## <a name="change-tracking-data-collection-details"></a>Ändra data collection detaljer
Ändringsspårning samlar in programvaruinventering och Windows-tjänst metadata med hjälp av hello agenter som du har aktiverat.

hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in för spårning av ändringar.

| Plattform | Styr Agent | Operations Manager-agent | Linux-agent | Azure Storage | Operations Manager som krävs? | Operations Manager agent-data som skickas via management-grupp | Frekvens för samlingen |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Windows- och Linux | &#8226; | &#8226; | &#8226; |  |  | &#8226; | 5 minuter too50 minuter beroende på hello ändringstyp. Se hello i den följande tabellen för mer information. |


hello visar följande tabell hello data collection frekvens för hello typer av ändringar.

| **ändra typen** | **frekvens** | **Har****agent****skicka skillnaderna när?**  |
| --- | --- | --- |
| Windows-registret | 50 minuter | Nej |
| Windows-filen | 30 minuter | Ja. Om ingen ändring har 24 timmar, skickas en ögonblicksbild. |
| Linux-fil | 15 minuter | Ja. Om ingen ändring har 24 timmar, skickas en ögonblicksbild. |
| Windows-tjänster | 30 minuter | Ja, var 30: e minut när ändringar finns. Var 24: e timme en ögonblicksbild som skickas, oavsett ändringen. Därför hello ögonblicksbild skickas även om det finns inga ändringar. |
| Linux-Daemon | 5 minuter | Ja. Om ingen ändring har 24 timmar, skickas en ögonblicksbild. |
| Windows-program | 30 minuter | Ja, var 30: e minut när ändringar finns. Var 24: e timme en ögonblicksbild som skickas, oavsett ändringen. Därför hello ögonblicksbild skickas även om det finns inga ändringar. |
| Linux-programmet | 5 minuter | Ja. Om ingen ändring har 24 timmar, skickas en ögonblicksbild. |

### <a name="registry-key-change-tracking"></a>Ändringen av registernyckeln spårning

Logganalys utför Windows-registret övervakning och spårning med hello ändringsspårning lösning. hello syftet med att övervaka ändringar tooregistry nycklar är toopinpoint punkter där kod från tredje part och skadlig kod kan aktivera. hello följande lista visar hello standard registernycklar som spåras av hello lösning och varför varje spåras.

- HKEY\_lokala\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup
    - Övervakare skript som körs vid start.
- HKEY\_lokala\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown
    - Övervakare skript som körs vid avstängningen.
- HKEY\_lokala\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
    - Övervakar nycklar som har lästs in innan hello användaren loggar in tootheir Windows-konto. hello-nyckeln används för 32-bitars program som körs på 64-bitarsdatorer.
- HKEY\_lokala\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed komponenter
    - Övervakar ändringar tooapplication inställningar.
- HKEY\_lokala\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers
    - Övervakare vanliga autostart transaktioner koppla direkt i Utforskaren och kör vanligtvis i processen med Explorer.exe.
- HKEY\_lokala\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers
    - Övervakare vanliga autostart transaktioner koppla direkt i Utforskaren och kör vanligtvis i processen med Explorer.exe.
- HKEY\_lokala\_MACHINE\Software\Classes\Directory\Background\ShellEx\ContextMenuHandlers
    - Övervakare vanliga autostart transaktioner koppla direkt i Utforskaren och kör vanligtvis i processen med Explorer.exe.
- HKEY\_lokala\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Övervakare för ikon täcker registrering.
- HKEY\_lokala\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Övervakare för ikon täcker registrering för 32-bitars program som körs på 64-bitarsdatorer.
- HKEY\_lokala\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper objekt
    - Övervakare för nya webbläsare helper objektet plugin-program för Internet Explorer. Använda tooaccess hello modellen DOM (Document Object) av hello aktuell sida och toocontrol navigeringen.
- HKEY\_lokala\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper objekt
    - Övervakare för nya webbläsare helper objektet plugin-program för Internet Explorer. Använda tooaccess hello modellen DOM (Document Object) av hello aktuell sida och toocontrol navigeringen för 32-bitars program som körs på 64-bitarsdatorer.
- HKEY\_lokala\_MACHINE\Software\Microsoft\Internet Explorer\Extensions
    - Övervakare för nya tillägg för Internet Explorer, till exempel anpassade verktyget menyer och anpassade knappar.
- HKEY\_lokala\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
    - Övervakare för nya tillägg för Internet Explorer, till exempel anpassade verktyget menyer och anpassade knappar för 32-bitars program som körs på 64-bitarsdatorer.
- HKEY\_lokala\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Övervakar hello 32-bitars drivrutiner som är associerade med wavemapper, wave1 och wave2, msacm.imaadpcm, .msadpcm, .msgsm610 och vidc. Liknande toohello [drivers]-avsnitt i hello SYSTEM. INI-filen.
- HKEY\_lokala\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Övervakare hello 32-bitars drivrutiner som är associerade med wavemapper, wave1 och wave2, msacm.imaadpcm, .msadpcm, .msgsm610 och vidc för 32-bitars program som körs på 64-bitarsdatorer. Liknande toohello [drivers]-avsnitt i hello SYSTEM. INI-filen.
- HKEY\_lokala\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls
    - Övervakare hello lista över kända eller vanliga system-DLL-filer; Det här systemet förhindrar att personer utnyttjar katalogbehörigheter svaga program genom att släppa trojansk häst versioner av system-DLL-filer.
- HKEY\_lokala\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify
    - Övervakare hello lista över paket kan tooreceive händelsemeddelanden från Winlogon hello interaktiv inloggning stöd modell för hello Windows-operativsystem.


## <a name="use-change-tracking"></a>Ändrar spårning
När hello lösningen har installerats kan du visa hello sammanfattning av ändringar för övervakade servrar med hello **ändringsspårning** panelen på hello **översikt** sida i OMS.

![Bild av ändringsspårning sida vid sida](./media/log-analytics-change-tracking/change-tracking-tile.png)

Du kan visa ändringarna tooyour infrastruktur och sedan gå till detaljer för hello följande kategorier:

* Ändringar efter konfigurationstyp för programvara och tjänster för Windows
* Programvaruändringar tooapplications och uppdateringar för enskilda servrar
* Totalt antal ändringar av programvaran för varje program
* Linux-paket
* Windows-tjänsten ändras för enskilda servrar
* Ändringar i Linux-daemon

![Bild av instrumentpanel för ändringsspårning](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![Bild av instrumentpanel för ändringsspårning](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="tooview-changes-for-any-change-type"></a>tooview för alla om du ändrar typ
1. På hello **översikt** klickar du på hello **ändringsspårning** panelen.
2. På hello **ändra spårning** instrumentpanel, granska hello sammanfattningsinformation i ett hello ändra typen blad och klicka på en tooview detaljerad information om den i hello **loggen Sök** sidan.
3. Du kan visa resultaten av tid, detaljerade resultat och Logghistoriken på någon av hello loggen söksidor. Du kan också filtrera efter facets toonarrow hello resultat.

## <a name="next-steps"></a>Nästa steg
* Använd [logga sökningar i logganalys](log-analytics-log-searches.md) tooview detaljerad data för spårning av ändringar.
