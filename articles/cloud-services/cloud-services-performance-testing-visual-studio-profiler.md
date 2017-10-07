---
title: "en molnet tjänsten lokalt i hello Compute Emulator aaaProfiling | Microsoft Docs"
services: cloud-services
description: "Undersöka prestandaproblem i molntjänster med hello Visual Studio-profiler"
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
tags: 
ms.assetid: 25e40bf3-eea0-4b0b-9f4a-91ffe797f6c3
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: fc37c85dad4db4cc0310f73afad56fc0fe5f3963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="testing-hello-performance-of-a-cloud-service-locally-in-hello-azure-compute-emulator-using-hello-visual-studio-profiler"></a>Testa hello prestanda för en moln-tjänsten lokalt i hello Azure Compute Emulator med hello Visual Studio-Profiler
En mängd olika verktyg och tekniker är tillgängliga för att testa hello prestanda för molntjänster.
När du publicerar en cloud service tooAzure du har Visual Studio samla in profileringsdata och analysera den lokalt, enligt beskrivningen i [profilering ett Azure-program][1].
Du kan också använda diagnostik tootrack räknare för en mängd olika prestanda, enligt beskrivningen i [med hjälp av prestandaräknare i Azure][2].
Du kan också tooprofile programmet lokalt i hello beräkningsemulatorn innan du distribuerar den toohello moln.

Den här artikeln beskriver hello CPU urvalsmetoden för profilering, vilket kan göras lokalt i hello-emulatorn. CPU-provtagning är en metod för profilering som inte är mycket påträngande. Vid en angiven insamlingsintervall tar hello profiler en ögonblicksbild av hello anropsstacken. hello data som samlas in under en viss tidsperiod, och visas i en rapport. Den här metoden för profilering tenderar tooindicate där i ett beräkningsmässigt intensiva program de flesta av hello CPU arbetet görs.  Detta ger hello möjlighet toofocus på hello ”varm path” där programmet tillbringar hello de flesta tid.

## <a name="1-configure-visual-studio-for-profiling"></a>1: Konfigurera Visual Studio för profilering
Det finns först ett fåtal konfigurationsalternativ för Visual Studio som kan vara användbara när profilering. toomake uppfattning om hello profilering rapporter måste du symboler (.pdb-filer) för programmet och även symboler för bibliotek. Vill du att du refererar till hello tillgängliga symbolen servrar toomake. toodo detta på hello **verktyg** -menyn i Visual Studio väljer **alternativ**, Välj **Debugging**, sedan **symboler**. Kontrollera att Microsoft Symbol-servrar visas **Symbol filplatser (.pdb)**.  Du kan också referera http://referencesource.microsoft.com/symbols som kan ha ytterligare symbol-filer.

![Symbolalternativ][4]

Om du vill kan du förenkla hello rapporterar att hello profiler genererar genom att ange bara min kod. Med bara min kod aktiverat, förenklas funktionen anropsstackar så som anropar helt interna toolibraries och hello .NET Framework är dolda från hello rapporter. På hello **verktyg** -menyn, Välj **alternativ**. Expandera hello **prestandaverktyg** nod, och välj **allmänna**. Markera kryssrutan för hello för **aktivera bara min kod för profiler rapporter**.

![Bara min kod alternativ][17]

Du kan använda dessa instruktioner med ett befintligt projekt eller med ett nytt projekt.  Om du skapar ett nytt projekt tootry hello tekniker som beskrivs nedan, Välj en C# **Azure Cloud Service** projektet och välj en **Webbroll** och en **Arbetsrollen**.

![Azure Cloud Service Projektroller][5]

Till exempel kan lägga till vissa Kodprojekt tooyour som tar mycket tid och visar vissa uppenbara prestandaproblem. Till exempel lägga till följande kod tooa arbetsrollsprojektet hello:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Anropa den här koden från hello RunAsync metod i hello worker rollen RoleEntryPoint-härledd klass. (Ignorera hello varning om hello-metoden körs synkront.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace hello following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Skapa och köra Molntjänsten lokalt utan felsökning (Ctrl + F5) med hello lösning konfigurationsuppsättning för**versionen**. Detta säkerställer att alla filer och mappar skapas för att köra programmet hello lokalt och säkerställer att alla hello emulatorerna har startats. Starta hello Compute Emulator UI från hello Aktivitetsfältet tooverify som kör arbetsrollen.

## <a name="2-attach-tooa-process"></a>2: Anslut tooa process
I stället för profilering hello programmet genom att starta från hello Visual Studio 2010 IDE, måste du koppla hello profileraren tooa körs. 

tooattach hello profileraren tooa processen på hello **analysera** -menyn, Välj **Profiler** och **Lägg till/ta bort**.

![Koppla profil][6]

Hitta hello WaWorkerHost.exe process för en arbetsroll.

![WaWorkerHost process][7]

Om din projektmapp finns på en nätverksenhet, ber hello profileraren dig tooprovide en annan plats toosave hello profilering rapporter.

 Du kan också koppla tooa webbroll genom att koppla tooWaIISHost.exe.
Om det finns flera arbetsprocesser roll i ditt program, måste du toouse hello processID toodistinguish dem. Du kan fråga hello processID via programmering genom att öppna processobjektet för hello. Till exempel om du lägger till den här koden toohello kör metoden hello RoleEntryPoint-härledd klass i en roll titta du på loggen i hello Compute Emulator UI tooknow vilka processen tooconnect till.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

tooview hello logg, start hello Compute Emulator UI.

![Starta hello Compute Emulator UI][8]

Öppna hello worker-rollen loggen konsolfönstret i hello Compute Emulator UI genom att klicka på namnlisten i fönstret hello-konsolen. Du kan se hello process-ID i hello-loggen.

![Visa process-ID][9]

Ett du har kopplat utför hello steg i programmets användargränssnitt (vid behov) tooreproduce hello scenario.

När du vill toostop profilering, Välj hello **stoppa profilering** länk.

![Stoppa profilering alternativet][10]

## <a name="3-view-performance-reports"></a>3: Visa prestandarapporter
hello systemprestanda-rapport för ditt program visas.

Nu hello profileraren stoppas utförandet, sparar data i en .vsp-fil och visar en rapport som visar en analys av data.

![Profileraren rapport][11]

Om du ser String.wstrcpy i hello visa varm sökväg, klicka på bara min kod toochange hello tooshow användarkod endast.  Om du ser String.Concat kan du prova att trycka på knappen för hello visar All kod.

Du bör se hello sammanfoga metoden och String.Concat tar upp en stor del av hello körningstid.

![Analys av rapporten][12]

Om du har lagt till hello sträng sammanfogning koden i den här artikeln bör du se en varning i hello uppgiftslista för den här. Du kan också se en varning om att det finns en orimlig mängd skräpinsamling förfaller toohello antal strängar som skapas och tagits bort.

![Prestanda varningar][14]

## <a name="4-make-changes-and-compare-performance"></a>4: gör ändringar och jämföra prestanda
Du kan också jämföra hello prestanda före och efter en kodändring.  Stoppa hello körs och redigera hello kod tooreplace hello sträng sammanfogning igen med hello används för StringBuilder:

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Göra en annan prestanda kör och jämför hello prestanda. I hello prestanda Explorer, om hello körs finns i hello samma session, du kan bara markera båda rapporterna öppna hello snabbmenyn och välj **jämför Prestandarapporterna**. Om du vill toocompare med en körning i en annan prestanda session, öppna hello **analysera** -menyn och välj **jämför Prestandarapporterna**. Ange båda filerna i hello dialogrutan som visas.

![Jämför prestanda rapporter alternativet][15]

hello rapporter Markera skillnaderna mellan två hello-körs.

![Jämförelserapport][16]

Grattis! Du har kommit igång med hello profiler.

## <a name="troubleshooting"></a>Felsökning
* Kontrollera att du profilering en slutversion och starta utan felsökning.
* Kör hello prestanda guiden om hello Lägg till/ta bort alternativet inte är aktiverad på hello Profiler-menyn.
* Använd hello Compute Emulator UI tooview hello status för ditt program. 
* Om du har problem med start av program i hello-emulatorn eller bifoga hello profiler, Stäng hello beräkningsemulatorn och starta om den. Om det inte löser problemet hello, startar du om datorn. Det här problemet kan inträffa om du använder hello Compute Emulator toosuspend och ta bort körs distributioner.
* Om du har använt någon av hello profilering kommandon från kommandoraden, särskilt hello globala inställningar, se till att VSPerfClrEnv /globaloff har anropats och att VsPerfMon.exe har avslutats.
* Om när provtagning, hello meddelande ”PRF0025: inga data samlades in”, kontrollera att hello processen bifogade toohas CPU-aktivitet. Program som inte gör några beräkningsarbete kan inte skapa några provtagning data.  Det är också möjligt att hello processen avslutades innan alla provtagning utfördes. Kontrollera toosee som hello Run-metoden för en roll som du profilering inte avslutas.

## <a name="next-steps"></a>Nästa steg
Instrumentering Azure binärfiler i hello-emulatorn stöds inte i hello Visual Studio profiler, men om du vill tootest minnesallokering, kan du välja alternativet när profilering. Du kan också välja samtidighet profilering, som hjälper dig att avgöra om trådar ha lagt tid konkurrerande för Lås eller tjänstnivån interaktion profilering, som hjälper dig att spåra prestandaproblem vid interaktion mellan nivåer för ett program, mest ofta mellan hello datanivå och en arbetsroll.  Du kan visa hello databasfrågor som genererar din app och använda hello profilering data tooimprove din användning av hello-databasen. Information om nivån interaktion profilering läser du blogginlägget hello [genomgång: med hello nivå interaktion Profiler i Visual Studio Team System 2010][3].

[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
