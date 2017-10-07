---
title: "aaaManage Azure Data Lake Analytics med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toomanage Data Lake Analytics räkenskaper, data datakällor, användare, och jobb."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: f63ccdfae79772c92e92462194e8cdc636a73dc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-by-using-hello-azure-portal"></a>Hantera Azure Data Lake Analytics med hjälp av hello Azure-portalen
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Lär dig hur toomanage Azure Data Lake Analytics-konton, konto datakällor, användare och jobb med hjälp av hello Azure-portalen. avsnitt om toosee om hur du använder andra verktyg, klicka på fliken hello överst på hello sidan.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Hantera Data Lake Analytics-konton

### <a name="create-an-account"></a>Skapa ett konto

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **Nytt** > **Intelligence + analytics** > **Data Lake Analysis**.
3. Välj värden för hello följande objekt: 
   1. **Namnet**: hello namnet på hello Data Lake Analytics-konto.
   2. **Prenumerationen**: hello Azure-prenumeration som används för hello-kontot.
   3. **Resursgruppen**: hello Azure-resursgrupp i vilket toocreate hello-konto. 
   4. **Plats**: hello Azure-datacenter för hello Data Lake Analytics-konto. 
   5. **Data Lake Store**: hello standard store toobe används för hello Data Lake Analytics-konto. hello Azure Data Lake Store-konto och hello Data Lake Analytics-kontot måste vara i hello samma plats.
4. Klicka på **Skapa**. 

### <a name="delete-a-data-lake-analytics-account"></a>Ta bort ett Data Lake Analytics-konto

Innan du tar bort ett Data Lake Analytics-konto kan du ta bort dess Data Lake Store-standardkontot.

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. Klicka på **Ta bort**.
3. Hello konto typnamn.
4. Klicka på **Ta bort**.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a>Hantera datakällor

Data Lake Analytics stöder hello följande datakällor:

* Data Lake Store
* Azure Storage

Du kan använda Data Explorer toobrowse datakällor och utföra grundläggande filhanteringsåtgärder. 

### <a name="add-a-data-source"></a>Lägga till en datakälla

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. Klicka på **datakällor**.
3. Klicka på **lägga till en datakälla**.
    
   * tooadd ett Data Lake Store-konto behöver du hello-konto och toohello konto toobe kan tooquery den.
   * tooadd Azure Blob storage måste hello storage-konto och hello kontonyckel. toofind dem, gå toohello storage-konto i hello-portalen.

## <a name="set-up-firewall-rules"></a>Konfigurera brandväggsregler

Du kan använda Data Lake Analytics toofurther Lås åtkomst tooyour Data Lake Analytics-konto på hello nätverksnivån. Du kan aktivera en brandvägg, ange en IP-adress eller definiera ett intervall med IP-adresser för dina betrodda klienter. När du har aktiverat dessa åtgärder kan endast klienter som har hello IP-adresser inom intervallet hello definierats ansluta toohello store.

Om andra Azure-tjänster, t.ex. Azure Data Factory eller virtuella datorer kan ansluta till toohello Data Lake Analytics-konto, kontrollerar du att **Tillåt Azure Services** är aktiverat **på**. 

### <a name="set-up-a-firewall-rule"></a>Konfigurera en brandväggsregel

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. På menyn hello hello vänster **brandväggen**.

## <a name="add-a-new-user"></a>Lägg till en ny användare

Du kan använda hello **guiden Lägg till användare** tooeasily etablera nya Data Lake-användare.

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. På vänster under hello **komma igång**, klickar du på **guiden Lägg till användare**.
3. Välj en användare och klicka sedan på **Välj**.
4. Välj en roll och klicka sedan på **Välj**. tooset upp en ny developer toouse Azure Data Lake, Välj hello **Data Lake Analytics Developer** roll.
5. Välj hello åtkomstkontrollistor (ACL) för hello U-SQL-databaser. När du är nöjd med dina val klickar du på **Välj**.
6. Välj hello ACL: er för filer. Hello standardlagringsplatsen inte ändrar hello ACL: er för rotmappen för hello ”/” och för hello/system mapp. Klicka på **Välj**.
7. Granska alla valda ändringarna och klicka sedan på **kör**.
8. När hello guiden är klar klickar du på **klar**.

## <a name="manage-role-based-access-control"></a>Hantera rollbaserad åtkomstkontroll

Du kan använda rollbaserad åtkomstkontroll (RBAC) toocontrol hur användarna samverkar med hello-tjänsten som andra Azure-tjänster.

hello standard RBAC-roller har hello följande funktioner:
* **Ägare**: kan skicka jobb, övervaka jobb, avbryta jobb från alla användare och konfigurera hello-konto.
* **Deltagare**: kan skicka jobb, övervaka jobb, avbryta jobb från alla användare och konfigurera hello-konto.
* **Läsaren**: övervaka jobb.

Använda hello Data Lake Analytics Developer tooenable U-SQL-utvecklare toouse hello Data Lake Analytics rolltjänsten. Du kan använda hello Data Lake Analytics Developer roll:
* Skicka jobb.
* Övervaka jobb status och hello jobb som skickats av någon användare.
* Se hello U-SQL-skript från jobb som skickats av någon användare.
* Avbryt endast egna jobb.

### <a name="add-users-or-security-groups-tooa-data-lake-analytics-account"></a>Lägga till användare eller säkerhet grupper tooa Data Lake Analytics-konto

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. Klicka på **åtkomstkontroll (IAM)** > **Lägg till**.
3. Välj en roll.
4. Lägga till en användare.
5. Klicka på **OK**.

>[!NOTE]
>Om en användare eller en säkerhetsgrupp måste toosubmit jobb, måste de också behörighet till hello store-konto. Mer information finns i [skyddar data som lagras i Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a>Hantera jobb

### <a name="submit-a-job"></a>Skicka ett jobb

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.

2. Klicka på **nytt jobb**. Konfigurera följande för varje jobb:

    1. **Jobbnamnet**: hello namnet på hello jobb.
    2. **Prioritet**: lägre nummer har högre prioritet. Om två jobb i kö, körs hello ett lägre värde för prioritet först.
    3. **Parallellitet**: hello maxantalet beräkning bearbetar tooreserve för det här jobbet.

3. Klicka på **Skicka jobb**.

### <a name="monitor-jobs"></a>Övervaka jobb

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. Klicka på **visa alla jobb**. En lista över alla hello-aktiva och nyligen klar jobb i hello-konto visas.
3. Du kan också klicka på **Filter** toohelp som du hittar hello jobb efter **tidsintervall**, **jobbnamn**, och **författare** värden. 

### <a name="monitoring-pipeline-jobs"></a>Övervaka pipeline-jobb
Jobb som är en del av en pipeline fungerar tillsammans, vanligtvis sekventiellt tooaccomplish ett specifikt scenario. Du kan till exempel ha en pipeline som rensar, extraherar, omvandlar, aggregerar användning för kunden insikter. Pipeline-jobb identifieras med hello ”Pipeline” egenskapen när hello jobbet har skickats. Jobb som schemalagts med ADF V2 har automatiskt den här egenskapen fylls i. 

tooview en lista över U-SQL-jobb som är del av pipelines: 

1. Gå tooyour Data Lake Analytics-konton i hello Azure-portalen.
2. Klicka på **jobbet insikter**. Hej ”alla” jobbfliken kommer att återställas, visar en lista över körs, i kö och avslutats jobb.
3. Klicka på hello **Pipeline jobb** fliken. En lista över pipeline jobb visas tillsammans med statistik för varje pipelinen.

### <a name="monitoring-recurring-jobs"></a>Övervaka återkommande jobb
Ett återkommande jobb är en hello samma affärslogik men använder olika indata varje gång det körs. Vi rekommenderar bör återkommande jobb alltid lyckas och har stabilt körningstid; övervakning av dessa beteenden säkerställer hello jobbet är felfri. Återkommande jobb identifieras med hello ”återkommande”-egenskap. Jobb som schemalagts med ADF V2 har automatiskt den här egenskapen fylls i.

tooview en lista över U-SQL-jobb som är återkommande: 

1. Gå tooyour Data Lake Analytics-konton i hello Azure-portalen.
2. Klicka på **jobbet insikter**. Hej ”alla” jobbfliken kommer att återställas, visar en lista över körs, i kö och avslutats jobb.
3. Klicka på hello **återkommande jobb** fliken. En lista över återkommande jobb visas tillsammans med statistik för alla återkommande jobb.

## <a name="manage-policies"></a>Hantera principer

### <a name="account-level-policies"></a>Kontonivå principer

Dessa principer tillämpas tooall jobb i ett Data Lake Analytics-konto.

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>Maximalt antal Australien i ett Data Lake Analytics-konto
En princip styr hello totala antalet Analytics-enheter (Australien) kan använda ditt Data Lake Analytics-konto. Hello-värdet är som standard too250. Till exempel om det här värdet anges too250 Australien, du kan ha ett jobb som körs med 250 Australien tilldelade tooit eller 10 jobb som körs med 25 Australien varje. Ytterligare jobb som skickats köas förrän hello jobb som körs är klar. När jobb som körs är slutförda Australien är frigörs för hello köade jobb toorun.

toochange hello antal Australien för ditt Data Lake Analytics-konto:

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. Klicka på **Egenskaper**.
3. Under **maximala Australien**, flytta hello skjutreglaget tooselect ett värde eller ange hello värde i textrutan för hello. 
4. Klicka på **Spara**.

> [!NOTE]
> Om du behöver mer än hello standard (250) Australien, hello-portalen klickar du på **hjälp + Support** toosubmit en supportbegäran. hello antalet Australien som är tillgängliga i ditt Data Lake Analytics-konto kan du öka.
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Maximalt antal jobb som kan köras samtidigt
En princip styr hur många jobb kan köras på hello samtidigt. Det här värdet är som standard too20. Om Data Lake Analytics har Australien som är tillgängliga, är nya jobb schemalagda toorun omedelbart tills hello Totalt antal jobb som körs hello värdet för den här principen. När du når hello maximalt antal jobb som kan köras samtidigt efterföljande jobb ställs i kö i prioritetsordning tills en eller flera av de jobb som körs är klar (beroende på Australien tillgänglighet).

toochange hello antalet jobb som kan köras samtidigt:

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. Klicka på **Egenskaper**.
3. Under **maximala antalet av kör jobb**, flytta hello skjutreglaget tooselect ett värde eller ange hello värde i textrutan för hello. 
4. Klicka på **Spara**.

> [!NOTE]
> Om du behöver mer än hello standard (20) antal jobb i hello-portalen klickar du på toorun **hjälp + Support** toosubmit en supportbegäran. hello antalet jobb som kan köras samtidigt i ditt Data Lake Analytics-konto kan du öka.
>

#### <a name="how-long-tookeep-job-metadata-and-resources"></a>Hur länge tookeep jobbet metadata och resurser 
När användarna kör U-SQL-jobb, behåller alla relaterade filer hello Data Lake Analytics-tjänsten. Relaterade filer inkluderar hello U-SQL-skript, hello DLL-filer som refereras i hello U-SQL-skript, kompilerade resurser och statistik. hello-filer finns i hello /system/ mapp för hello standardkontot för lagring av Azure Data Lake. Den här principen styr hur länge dessa resurser lagras innan de tas bort automatiskt (hello standard är 30 dagar). Du kan använda dessa filer för felsökning och prestandajustering av jobb som du ska köra i hello framtida.

toochange hur länge tookeep jobbet metadata och resurser:

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. Klicka på **Egenskaper**.
3. Under **dagar tooRetain Jobbfrågor**, flytta hello skjutreglaget tooselect ett värde eller ange hello värde i textrutan för hello.  
4. Klicka på **Spara**.

### <a name="job-level-policies"></a>Nivå principer
Med principer för jobbet på objektnivå kan du styra hello maximala Australien och hello maximal prioritet som enskilda användare (eller medlemmar i specifika säkerhetsgrupper) kan ställa in för jobb som de skickar. Detta kan du styra hello kostnader av användare. Du kan också hello effekt som schemalagda jobb kan ha på hög prioritet produktionsjobb som körs i hello samma Data Lake Analytics-konto.

Data Lake Analytics har två principer som du kan ange nivån för hello jobb:

* **Australien gränsen per jobb**: användare kan bara skicka jobb som har ett toothis antal Australien. Som standard är den här gränsen hello samma som hello Australien maxgränsen för hello-konto.
* **Prioritet**: användare kan bara skicka jobb som har ett värde för prioritet lägre än eller lika med toothis. Observera att ett högre värde innebär en lägre prioritet. Detta är som standard too1, vilket är hello högsta möjliga prioritet.

Det finns en standardprincip som anges för varje konto. hello standardprincipen gäller tooall användare av hello-konto. Du kan ange ytterligare principer för specifika användare och grupper. 

> [!NOTE]
> Kontonivå och nivå principer tillämpas samtidigt.
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a>Lägg till en princip för en specifik användare eller grupp

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. Klicka på **Egenskaper**.
3. Under **jobbet skicka gränser**, klicka på hello **Lägg till princip** knappen. Sedan, Välj eller ange hello följande inställningar:
    1. **Beräkna principnamn**: Ange ett principnamn, tooremind du hello syftet med hello principen.
    2. **Välj användare eller grupp**: Välj hello användare eller grupper som principen gäller.
    3. **Ange hello jobbet Australien gränsen**: hello Australien gränsen som gäller toohello valda användare eller grupp.
    4. **Ange hello prioritet gränsen**: hello prioritet gränsen som gäller toohello valda användare eller grupp.

4. Klicka på **OK**.

5. hello nya principen visas i hello **standard** princip tabell under **jobbet skicka gränser**. 

#### <a name="delete-or-edit-an-existing-policy"></a>Ta bort eller redigera en befintlig princip

1. Gå tooyour Data Lake Analytics-konto i hello Azure-portalen.
2. Klicka på **Egenskaper**.
3. Under **jobbet skicka gränser**, hitta hello princip som du vill tooedit.
4.  toosee hello **ta bort** och **redigera** i hello längst till höger kolumn hello tabell, klicka på **...** .

### <a name="additional-resources-for-job-policies"></a>Ytterligare resurser för jobbet principer
* [Blogginlägget för principen: översikt](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Principer för kontonivå blogginlägget](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Principer jobb på blogginlägget](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Nästa steg

* [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Kom igång med Data Lake Analytics med hjälp av hello Azure-portalen](data-lake-analytics-get-started-portal.md)
* [Hantera Azure Data Lake Analytics med hjälp av Azure PowerShell](data-lake-analytics-manage-use-powershell.md)

