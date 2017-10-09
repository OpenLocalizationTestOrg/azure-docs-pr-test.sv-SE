---
title: "aaaTesting hello prestanda för en molnbaserad tjänst | Microsoft Docs"
description: "Testa hello prestanda för en tjänst i molnet med hjälp av hello Visual Studio-profiler"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7a5501aa-f92c-457c-af9b-92ea50914e24
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 98bd775e6ffcf948e737c5ec26399c81f4770fe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="testing-hello-performance-of-a-cloud-service"></a>Testa hello prestanda för en tjänst i molnet
## <a name="overview"></a>Översikt
Du kan testa hello prestanda för en tjänst i molnet i hello följande sätt:

* Använda Azure-diagnostik toocollect information om begäranden och anslutningar och tooreview platsstatistik som visar hur hello-tjänsten utför ur customer. tooget igång med, se [konfigurera diagnostik för virtuella datorer och Azure Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=623009).
* Använd hello Visual Studio profiler tooget en detaljerad analys av hello beräkningar aspekter av hur hello-tjänsten körs. Som det här avsnittet beskriver, kan du använda hello profileraren toomeasure prestanda som en tjänst som körs i Azure. Information om hur toouse hello profileraren toomeasure prestanda som en tjänst körs lokalt i en beräkningsemulatorn finns [testar hello prestanda för ett Azure Cloud Service lokalt i hello Compute Emulator med hello Visual Studio-Profiler ](http://go.microsoft.com/fwlink/p/?LinkId=262845).

## <a name="choosing-a-performance-testing-method"></a>Att välja en metod för prestandatestning
### <a name="use-azure-diagnostics-toocollect"></a>Använd Azure-diagnostik toocollect:
* Statistik på webbsidor eller tjänster, till exempel begäranden och anslutningar.
* Statistik över roller, till exempel hur ofta en roll har startats om.
* Allmän information om minnesanvändning, till exempel hello procentandel av tiden som hello skräpinsamlingen tar eller hello minne uppsättning den pågående rollen.

### <a name="use-hello-visual-studio-profiler-to"></a>Använd hello Visual Studio-profiler till:
* Avgör vilka funktioner ta hello de flesta tid.
* Mäter hur lång tid tar för varje del av ett beräkningsmässigt intensiva program.
* Jämför detaljerad prestandarapporter för två versioner av en tjänst.
* Analysera minnesallokering i detalj än hello enskilda minnesallokering.
* Analysera samtidighet problem i flertrådade kod.

När du använder hello profiler, kan du samla in data när en molnbaserad tjänst körs lokalt eller i Azure.

### <a name="collect-profiling-data-locally-to"></a>Samla in profileringsdata lokalt till:
* Testa hello prestanda för en del av en tjänst i molnet, till exempel hello körning av specifika arbetsroll som inte kräver en realistisk simulerade belastning.
* Testa hello prestanda för en tjänst i molnet i isolering under kontrollerade förhållanden.
* Testa hello prestanda för en tjänst i molnet innan du distribuerar den tooAzure.
* Testa hello prestanda för en tjänst i molnet privat, utan att störa hello befintliga distributioner.
* Testa hello prestanda för hello tjänsten utan att det medför kostnader för körs i Azure.

### <a name="collect-profiling-data-in-azure-to"></a>Samla in profileringsdata i Azure för att:
* Testa hello prestanda för en tjänst i molnet under en simulerad eller verkliga belastning.
* Använd hello instrumentation metoden för att samla in profileringsdata, som det här avsnittet beskrivs senare.
* Testa hello prestanda för hello tjänsten i hello samma miljö som när hello-tjänsten körs i produktion.

Du vanligtvis simulera en belastningen tootest molntjänster under normal eller betonar villkor.

## <a name="profiling-a-cloud-service-in-azure"></a>Profilering av en tjänst i molnet i Azure
När du publicerar Molntjänsten från Visual Studio kan du profilen hello-tjänsten och ange hello profilering inställningar som ger du hello information som du vill. En profilering session startas för varje instans av en roll. Mer information om hur toopublish tjänsten från Visual Studio, se [publicering tooan Azure Cloud Service från Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

toounderstand mer information om prestanda profilering i Visual Studio finns [nybörjare guiden tooPerformance Profiling](https://msdn.microsoft.com/library/azure/ms182372.aspx) och [Analysera prestanda med hjälp av verktyg för profilering](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

> [!NOTE]
> Du kan aktivera IntelliTrace eller profilering när du publicerar Molntjänsten. Du kan aktivera båda.
> 
> 

### <a name="profiler-collection-methods"></a>Profileraren samling metoder
Du kan använda annan samling metoder för profilering, baserat på dina prestandaproblem:

* **CPU-provtagning** -den här metoden samlar in programstatistik som är användbara för inledande analys av problem med CPU-användning. CPU-sampling är hello föreslagna metod för att starta de flesta prestanda undersökningar. Det finns en låg påverkan på hello-program som du profilering när du samlar in CPU provtagning data.
* **Instrumentation** -den här metoden samlar in detaljerad tidsinställning data som är användbar för fokuserad analys och för att analysera i/o-prestanda. hello instrumentation metoden registrerar varje post och avsluta funktionsanropet hello funktioner i en modul under en profilering kör. Den här metoden är användbar för att samla in detaljerade tidsinställning information om en del av din kod och för att förstå hello effekten av inkommande och utgående åtgärder på programmets prestanda. Den här metoden är inaktiverad för en dator som kör ett 32-bitars operativsystem. Det här alternativet är tillgängligt endast när du kör hello Molntjänsten i Azure, inte lokalt i hello beräkningsemulatorn.
* **Minnesallokering för .NET** -den här metoden samlar in .NET Framework minne allokering data med hjälp av hello provtagning profilering metod. hello insamlade data omfattar hello antalet och storleken på allokerade objekt.
* **Concurrency** -den här metoden samlar in konkurrens resursdata och process- och Körningsdata som är användbara vid analys av flertrådade och flerprocessig program. hello samtidighet metoden samlar in data för varje händelse som blockerar körning av din kod, till exempel när en tråd väntar låst åtkomst tooan programmet resurs toobe frigöras. Den här metoden är användbar för att analysera flertrådiga program.
* Du kan också aktivera **nivå interaktion profilering**, som innehåller ytterligare information om hello körningstider för synkron ADO.NET anropar i flera nivåer program som kommunicerar med en eller flera funktioner databaser. Du kan samla in data för nivån interaktion med någon av hello profilering metoder. Läs mer om nivån interaktion profilering [nivå interaktioner visa](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Konfigurera inställningar för profilering
Hej följande bild visar hur tooconfigure profilering inställningarna från hello i dialogrutan Publicera Azure-program.

![Konfigurera profilering inställningar](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

> [!NOTE]
> tooenable hello **Aktivera profilering** kryssrutan, måste du ha hello profiler som är installerad på hello datorn att du använder toopublish Molntjänsten. Hello profiler installeras som standard när du installerar Visual Studio.
> 
> 

### <a name="tooconfigure-profiling-settings"></a>tooconfigure profilering inställningar
1. Öppna hello snabbmenyn för din Azure-projekt i Solution Explorer och välj sedan **publicera**. Detaljerade anvisningar om hur toopublish en molnbaserad tjänst bör se [publicering av ett moln med hjälp av tjänsten hello Azure-verktyg](http://go.microsoft.com/fwlink/p?LinkId=623012).
2. I hello **publicera Azure-programmet** dialogrutan, Välj hello **avancerade inställningar** fliken.
3. tooenable profilering, Välj hello **Aktivera profilering** kryssrutan.
4. tooconfigure inställningarna profilering väljer hello **inställningar** hyperlänk. hello dialogrutan profilering inställningar.
5. Från hello **vilken metod för profilering skulle du som toouse** alternativknappar, Välj hello typ av profilering som du behöver.
6. toocollect hello nivå interaktion profilering data, Välj hello **aktivera nivå interaktion profilering** kryssrutan.
7. toosave hello inställningar, väljer hello **OK** knappen.
   
    När du publicerar det här programmet är de här inställningarna används toocreate hello profilering sessionen för varje roll.

## <a name="viewing-profiling-reports"></a>Visa profilering rapporter
En profilering session skapas för varje instans av en roll i Molntjänsten. tooview profilering av dina rapporter för varje session från Visual Studio kan du visa hello Server Explorer-fönstret och välj hello Azure Compute-nod tooselect en instans av en roll. Du kan sedan visa hello profilering rapport som visas i följande illustration hello.

![Visa profilering rapport från Azure](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="tooview-profiling-reports"></a>tooview profilering rapporter
1. tooview hello Server Explorer-fönstret i Visual Studio på hello menyn fältet Välj vyn Server Explorer.
2. Välj hello Azure Compute-nod och välj hello Azure-distribution nod för hello molntjänst som du valde tooprofile när du har publicerat från Visual Studio.
3. tooview profilering rapporter för en instans, Välj hello roll i hello service, öppna hello snabbmenyn för en specifik instans och sedan **visa profilering rapport**.
   
    hello rapport, en .vsp-fil, nu laddas ned från Azure och hello status hello hämtning visas i hello Azure-aktivitetsloggen. När hello nedladdningen är klar hello profilering rapporten visas på en flik i hello Redigeraren för Visual Studio med namnet <Role name>  *<Instance Number>*  <identifier>.vsp. Sammanfattningsdata för hello rapporten visas.
4. toodisplay olika vyer av hello rapporten i hello aktuell vy listan, Välj hello typ av vy som du vill. Mer information finns i [profilering verktyg rapportvyer](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Nästa steg
[Felsökning av molntjänster](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Publishing tooan Azure Cloud Service från Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

