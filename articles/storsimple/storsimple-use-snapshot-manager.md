---
title: "användargränssnittet för aaaStorSimple Snapshot Manager | Microsoft Docs"
description: "Beskriver hello StorSimple Snapshot Manager-användargränssnittet och förklarar hur toouse den toomanage säkerhetskopieringsjobb och hello säkerhetskopiera katalog."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: c7d91892-2881-41a2-a7a2-908dc3646493
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.custom: 
ms.openlocfilehash: 865520fdaf1b559714b52b428ad136b084d65c99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-user-interface-toomanage-backup-jobs-and-backup-catalog"></a>Använd StorSimple Snapshot Manager användaren gränssnittet toomanage säkerhetskopieringsjobb och säkerhetskopieringskatalogen

## <a name="overview"></a>Översikt
Hej StorSimple Snapshot Manager har ett intuitivt användargränssnitt som du använder tootake och hantera säkerhetskopior. Den här självstudiekursen innehåller en introduktion toohello användargränssnitt och sedan förklarar hur toouse hello-komponenter. En detaljerad beskrivning av hello StorSimple Snapshot Manager finns [vad är StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Konsolen beskrivning
tooview hello användaren gränssnitt, klicka på ikonen för hello StorSimple Snapshot Manager på skrivbordet. hello visas-konsolen som visas i följande illustration hello.

![Fönster för StorSimple Snapshot Manager](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

hello konsolfönstret har fem viktiga element. Klicka på hello länken för en fullständig beskrivning av varje element.

* [Menyraden](#menu-bar) 
* [Verktygsfältet](#tool-bar) 
* [Omfattning](#scope-pane) 
* [Resultatfönstret](#results-pane) 
* [Åtgärdsfönstret](#actions-pane) 

Dessutom hello StorSimple Snapshot Manager stöder [tangentbord navigering och ett antal genvägar](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Konsolen hjälpmedel
Hej StorSimple Snapshot Manager-användargränssnittet stöder hello funktioner finns hello Windows-operativsystem och hello Microsoft Management Console (MMC), samt vissa StorSimple Snapshot Manager – specifika kortkommandon. 

* En beskrivning av hello Windows hjälpmedelsfunktioner gå för[kortkommandon för Windows](https://support.microsoft.com/kb/126449). 
* En beskrivning av hello MMC hjälpmedelsfunktioner gå för[hjälpmedel för MMC 3.0](https://technet.microsoft.com/library/cc766075.aspx)
* En beskrivning av hello StorSimple Snapshot Manager hjälpmedelsfunktioner gå för[tangentbord navigering och genvägar](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Menyraden
hello menyraden hello överst i konsolfönstret hello innehåller [filen](#file-menu), [åtgärd](#action-menu), [visa](#view-menu), [Favoriter](#favorites-menu), [fönster ](#window-menu), och [hjälp](#help-menu) menyer.

Klicka på ett objekt i hello menyn fältet toosee en lista över tillgängliga kommandon på menyn. hello följande exempel visar hello **visa** menyn som valts på hello menyraden.

![Visa-menyn som valts](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Arkivmenyn
Hej **filen** menyn innehåller standardkommandon för Microsoft Management Console (MMC).

#### <a name="menu-access"></a>Menyåtkomst
tooview hello **filen** -menyn klickar du på **filen** hello menyraden. hello följande meny visas.

![StorSimple Snapshot Manager Arkiv-menyn](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Menyn Beskrivning
hello följande tabell beskrivs objekt som visas på hello **filen** menyn.

| Menyalternativ | Beskrivning |
|:--- |:--- |
| Ny |Klicka på **ny** toocreate en ny konsol utifrån hello StorSimple Snapshot Manager. |
| Öppet |Klicka på **öppna** tooopen en befintlig konsol. |
| Spara |Klicka på **spara** toosave hello aktuella konsolen. |
| Spara som |Klicka på **Spara som** toocreate en ny, bytt namn till instans av aktuella hello-konsolen. Använd hello **Spara som** alternativet toocustomize en vy och spara den för senare hämtning. Du kan till exempel skapa StorSimple Snapshot Manager snapin-moduler som punkt toospecific-servrar. |
| Lägg till/ta bort snapin-modul |Klicka på **Lägg till/ta bort snapin-modulen** tooadd eller ta bort snapin-moduler och tooorganize noder i hello **omfång** fönstret. Mer information finns för[Lägg till, ta bort och ordna snapin-moduler och tillägg i MMC 3.0](https://technet.microsoft.com/library/cc722035.aspx). |
| Alternativ |Klicka på **alternativ** toochange hello konsolen ikonen Ange åtkomstlägen för användare och behörigheter eller ta bort konsolen filer tooincrease ledigt diskutrymme. |
| Lista över sökvägar |Klicka på en sökväg i hello numrerad lista tooreopen en fil som du nyligen öppnat. |
| Avsluta |Klicka på **avsluta** tooclose hello **filen** menyn. |

### <a name="action-menu"></a>Åtgärd-menyn
Använd hello **åtgärd** menyn tooselect från tillgängliga åtgärder. tillgängliga tooyou för hello-objekt är beroende av hello val som du gör i hello **omfång** rutan eller **resultat** fönstret.

#### <a name="menu-access"></a>Menyåtkomst
tooview hello **åtgärd** -menyn, gör du något av följande hello:

* Högerklicka på ett objekt i hello **omfång** rutan eller **resultat** fönstret.
* Markera ett objekt i hello **omfång** rutan eller **resultat** rutan och klicka sedan på **åtgärd** hello menyraden. 

Till exempel om du väljer hello översta noden i hello **omfång** fönstret och sedan högerklicka eller klicka **åtgärd** i hello menyrad hello följande meny visas.

![StorSimple Snapshot Manager Åtgärd-menyn](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

Hej **åtgärder** fönstret (på hello höger i konsolen hello) innehåller hello samma lista med åtgärder som hello **åtgärd** menyn. Dessutom hello **åtgärder** rutan innehåller hello **visa** menyalternativ som gör att du toocreate en anpassad vy av hello **resultat** fönstret.

![Öppna i åtgärdsfönstret med Visa-menyn](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Menyn Beskrivning
hello följande tabell innehåller en alfabetisk lista över åtgärder för StorSimple Snapshot Manager. 

* Hej **åtgärd** kolumnen visas åtgärder som du kan utföra på noder och resultat. 
* Hej **navigering** kolumn förklarar hur toodisplay hello lämpliga **åtgärd** menyn så att du kan välja hello-åtgärd. Vissa åtgärder visas i flera **åtgärd** menyer. För dessa åtgärder, väljer du ett **navigering** alternativet från hello numrerad lista. 
* Hej **beskrivning** kolumnen beskriver hur toouse varje åtgärd i hello **åtgärd** -menyn eller åtgärdsfönstret och förklarar vad det gör.

> [!NOTE]
> Hej **åtgärder** rutan och **åtgärd** menyer innehåller fler alternativ, till exempel **visa**, **nytt fönster härifrån**, ** Uppdatera**, **Exportera lista**, och **hjälp**. Dessa alternativ är tillgängliga som en del av hello MMC och är inte specifik tooStorSimple Snapshot Manager. hello tabell innehåller beskrivningar av dessa alternativ.
> 
> 

| Åtgärd | Navigering | Beskrivning |
|:--- |:--- |:--- |
| Autentisera |Klicka på hello **enheter** nod och högerklicka på en enhet i hello **resultat** fönstret. |Klicka på **autentisera** tooenter hello lösenord som du konfigurerade för hello enhet. |
| Klona |Expandera **säkerhetskopieringskatalogen**, expandera **Molnögonblicksbilder**, klicka på en säkerhetskopia av datum och välj sedan en volym i hello **resultat** fönstret. |Klicka på **klona** toocreate en kopia av ett moln ögonblicksbild och lagra den på en plats som du har angett. |
| Konfigurera en enhet |Högerklicka på hello **enheter** nod. |Klicka på **konfigurera en enhet** tooconfigure en enstaka enhet eller flera enheter tooconnect toohello Windows-värd. |
| Skapa princip för säkerhetskopiering |Gör något av följande hello:<ul><li>Högerklicka på **Säkerhetskopieringsprinciper**.</li><li>Klicka eller expandera **volym grupper**, högerklicka på en volym-grupp.</li><li>Klicka på eller expandera **säkerhetskopieringskatalog**, högerklicka på en volym-grupp.</li></ul> |Klicka på **skapa säkerhetskopieringsprincip** tooconfigure en schemalagd säkerhetskopiering för en grupp för volymen. |
| Skapa volymen grupp |Gör något av följande hello:<ul><li>Klicka på hello **volymer** nod och högerklicka sedan på en volym i hello **resultat** fönstret.</li><li>Högerklicka på hello **volym grupper** nod.</li></ul> |Klicka på **skapa volymen grupp** tooassign volymer tooa volym grupp. |
| Ta bort |Klicka på en nod eller ett resultat (det här objektet visas i många **åtgärd** menyer och **åtgärder** fönster.) |Klicka på **ta bort** toodelete hello nod eller ett resultat som du har valt. När hello bekräftelse i dialogrutan Bekräfta eller Avbryt hello borttagning. |
| Information |Klicka på hello **enheter** nod och högerklicka sedan på en enhet i hello **resultat** fönstret. |Klicka på **information** toosee hello konfigurationsinformation för en enhet. |
| Redigera |Klicka på **Säkerhetskopieringsprinciper**, och högerklicka sedan på en princip i hello **resultat** fönstret. |Klicka på **redigera** toochange hello schemat för säkerhetskopiering för en grupp för volymen. |
| Exportera lista |Klicka på en nod eller resultatet (det här objektet visas på alla **åtgärd** menyer och **åtgärder** fönster.) |Klicka på **exportera listan** toosave en lista i en fil med kommaavgränsade värden (CSV). Du kan sedan importera den här filen i ett kalkylbladsprogram för analys. |
| Hjälp |Klicka på en nod eller ett resultat. (Det här objektet visas på alla **åtgärd** menyer och **åtgärder** fönster.) |Klicka på **hjälp** tooopen onlinehjälpen i ett nytt webbläsarfönster. |
| Nytt fönster härifrån |Klicka på en nod eller resultatet (det här objektet visas på alla **åtgärd** menyer och **åtgärder** fönster.) |Klicka på **nytt fönster härifrån** tooopen ett nytt fönster för StorSimple Snapshot Manager. |
| Uppdatera |Klicka på en nod eller resultatet (det här objektet visas på alla **åtgärd** menyer och **åtgärder** fönster.) |Klicka på **uppdatera** tooupdate hello som för närvarande visas StorSimple Snapshot Manager-fönstret. |
| Uppdatera enheten |Klicka på hello **enheter** nod och högerklicka på en enhet i hello **resultat** fönstret. |Klicka på **uppdatera enheten** toosynchronize en ansluten enhet med StorSimple Snapshot Manager. |
| Uppdatera enheter |Högerklicka på hello **enheter** nod. |Klicka på **uppdatera enheter** toosynchronize din lista över anslutna enheter med StorSimple Snapshot Manager. |
| Skanna volymer |Högerklicka på hello **volymer** nod. |Klicka på **skanna volymer** tooupdate hello lista över volymer som visas i hello **resultat** fönstret. |
| Återställ |Expandera **säkerhetskopieringskatalogen**, expandera en volym-grupp, expandera **lokala ögonblicksbilder** eller **Molnögonblicksbilder**, högerklicka på en säkerhetskopia. |Klicka på **återställa** tooreplace hello aktuella grupp volymdata med hello data från hello valda säkerhetskopian. |
| Säkerhetskopiera |Gör något av följande hello:<ul><li>Expandera **volym grupper**, högerklicka på en volym-grupp.</li><li>Expandera **säkerhetskopieringskatalog**, högerklicka på en volym-grupp.</li></ul> |Klicka på **ta säkerhetskopiering** toostart ett säkerhetskopieringsjobb omedelbart. |
| Växla import visning |Högerklicka på hello översta noden i hello **omfång** fönstret (hello **StorSimple Snapshot Manager** nod i hello exempel). |Klicka på **växla import visa** tooshow eller dölja hello volym grupper och tillhörande säkerhetskopior som har importerats från hello StorSimple Enhetshanteraren service instrumentpanelen. |

### <a name="view-menu"></a>Visa-menyn
Använd hello **visa** menyn toocreate en anpassad vy av hello **resultat** fönstret innehållet. Hej **visa** menyn innehåller **Lägg till/ta bort kolumner** och **anpassa** alternativ.

#### <a name="menu-access"></a>Menyåtkomst
Du kan komma åt hello **visa** menyn hello menyraden eller hello **åtgärder** fönstret.

![StorSimple Snapshot Manager Visa-menyn](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Menyn Beskrivning
hello följande tabell beskrivs objekt som visas på hello **visa** menyn.

| Menyalternativ | Beskrivning |
|:--- |:--- |
| Lägg till/ta bort kolumner |Klicka på **Lägg till/ta bort kolumner** tooadd eller ta bort kolumner i hello **resultat** fönstret. |
| Anpassa |Klicka på **anpassa** tooshow eller Dölj objekt i konsolfönstret för hello StorSimple Snapshot Manager. |

### <a name="favorites-menu"></a>Favoriter-menyn
Använd hello **Favoriter** menyn tooadd, ta bort och ordna sidan vyer och uppgifter som du använder ofta. 

#### <a name="menu-access"></a>Menyåtkomst
Du kan komma åt hello **Favoriter** menyn på hello menyraden.

![StorSimple Snapshot Manager Favoriter-menyn](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Menyn Beskrivning
hello följande tabell beskrivs objekt som visas på hello **Favoriter** menyn.

| Menyalternativ | Beskrivning |
|:--- |:--- |
| Lägg till tooFavorites |Klicka på **lägga till tooFavorites** tooadd hello aktuella visa tooyour lista över favoriter. |
| Ordna Favoriter |Klicka på **Ordna Favoriter** tooorganize hello innehållet i mappen Favoriter. |

### <a name="window-menu"></a>Fönster-menyn
Använd hello **fönstret** menyn tooadd och ordna om StorSimple Snapshot Manager konsolfönster.

#### <a name="menu-access"></a>Menyåtkomst
Du kan komma åt hello **fönstret** menyn på hello menyraden.

![StorSimple Snapshot Manager fönster-menyn](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

hello numrerad lista längst hello hello menyn visar hello windows som för tillfället är öppna. Klicka på ett fönster i listan toobring hello fönstret i förgrunden hello. 

#### <a name="menu-description"></a>Menyn Beskrivning
hello följande tabell beskrivs hello-objekt som visas på hello Fönster-menyn.

| Menyalternativ | Beskrivning |
|:--- |:--- |
| Nytt fönster |Klicka på **nytt fönster** tooopen ett nytt konsolfönster (i tillägget toohello befintliga fönster). |
| Överlappande |Klicka på **Cascade** toodisplay hello öppna konsolfönstret i cascading style. |
| Sida vid sida |Klicka på **panelen vågrätt** toodisplay hello öppna konsolfönstret i formatet sida vid sida (eller rutnät). |
| Ordna ikoner |Om du har flera konsolen windows öppna och utspridda över skrivbordet, minimera dem och klickar sedan på **Ordna ikoner** tooarrange dem i en vågrät rad på hello längst ned på skärmen. |

### <a name="help-menu"></a>Hjälp-menyn
Använd hello **hjälp** menyn tooview tillgängliga onlinehjälpen för StorSimple Snapshot Manager och hello MMC. Du kan också visa information om hello MMC och StorSimple Snapshot Manager programvaruversioner som är installerade på datorn. 

Du kan komma åt hello **hjälp** menyn på hello menyraden. Du kan också komma åt hjälpavsnitt för StorSimple Snapshot Manager hello **åtgärder** fönstret.

![StorSimple Snapshot Manager hjälp-menyn](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Menyn Beskrivning
hello i den följande tabellen beskrivs objekt som visas på hello Hjälp-menyn.

| Menyalternativ | Beskrivning |
|:--- |:--- |
| Hjälp om StorSimple Snapshot Manager |Klicka på **hjälp på StorSimple Snapshot Manager** tooopen StorSimple Snapshot Manager hjälp i ett separat fönster. |
| Hjälpavsnitt |Klicka på **hjälpavsnitt** tooopen MMC onlinehjälpen i ett separat fönster. |
| TechCenter-webbplats |Klicka på **TechCenter webbplats** tooopen hello Microsoft TechNet Tech Center startsidan i ett separat fönster. |
| Om Microsoft Management Console |Klicka på **om Microsoft Management Console** toosee vilken version av hello Microsoft Management Console har installerats på datorn. |
| Om StorSimple Snapshot Manager |Klicka på **om StorSimple Snapshot Manager** toosee vilken version av hello snapin-modulen är installerad på datorn. |

## <a name="tool-bar"></a>Verktygsfältet
hello verktygsfältet under hello menyraden innehåller navigering och ikonerna. Varje ikon är en genväg tooa aktivitet.

### <a name="icon-descriptions"></a>Ikonen beskrivningar
hello följande tabell beskrivs hello ikoner som visas på hello-verktygsfältet. 

| Ikon | Beskrivning |
|:--- |:--- |
| ![VÄNSTERPIL](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) |Klicka på hello VÄNSTERPIL ikonen tooreturn toohello föregående sida. |
| ![HÖGERPIL](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) |Klicka på nästa sida om hello högerpilen toogo toohello (om hello pilen är grå, hello åtgärden är inte tillgänglig). |
| ![Upp-ikon](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) |Klicka på hello in ikonen toogo upp en nivå i konsolträdet för hello (hello **omfång** fönstret). |
| ![Visa/dölj konsolträd](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) |Klicka på hello Visa/Dölj konsolen trädet ikonen tooshow eller dölja hello **omfång** fönstret. |
| ![Exportera lista](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) |Klicka på hello exportera listan ikonen tooexport en lista över tooa CSV-fil som du anger. |
| ![Hjälpikonen](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png) |Klicka på hello hjälp ikonen tooopen en MMC-hjälpavsnittet. |
| ![Visa/Dölj åtgärdsfönstret](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) |Klicka på hello Visa/dölj **åtgärder** fönstret ikonen tooshow eller dölja hello **åtgärder** fönstret. |

## <a name="scope-pane"></a>Omfattning
Hej **omfång** är hello längst till vänster i hello StorSimple Snapshot Manager UI. Det innehåller hello-konsolen (eller noden) träd och hello primära navigering mekanism för StorSimple Snapshot Manager. 

### <a name="scope-pane-structure"></a>Struktur för scope-fönstret
Hej **omfång** rutan innehåller en serie klickbara objekt (noder) i en trädstruktur. 

![Omfattning](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

* tooexpand eller komprimera en nod, klickar du på hello pilen ikonen nästa toohello nodnamn.
* Klicka på hello nodnamnet tooview hello status eller innehållet i en nod. hello information visas i hello **resultat** fönstret. 

Hej **omfång** rutan innehåller hello följande noder: 

* [Enhetsnoden](#devices-node) 
* [Volymer nod](#volumes-node) 
* [Volymen Gruppnod](#volume-groups-node) 
* [Säkerhetskopiera noden policyer](#backup-policies-node) 
* [Säkerhetskopiera katalogen nod](#backup-catalog-node) 
* [Jobb nod](#jobs-node) 

### <a name="scope-pane-tasks"></a>Omfång rutan uppgifter
Du kan använda hello **omfång** fönstret toocomplete en åtgärd på en viss nod. tooselect en aktivitet, gör något av följande hello:

* Högerklicka på noden hello och välj sedan hello uppgiften hello-menyn som visas.
* Klicka på hello noden och klicka sedan på **åtgärd** hello menyraden. Välj hello uppgift hello-menyn som visas.
* Klicka på hello noden och välj sedan hello-åtgärden i hello **åtgärder** fönstret.

När du väljer en nod och använder någon av dessa metoder toosee uppgiftslistor visas bara de åtgärder som kan utföras på noden.

### <a name="devices-node"></a>Enhetsnoden
Hej **enheter** nod representerar hello StorSimple-enheter och virtuella StorSimple-enheter som är anslutna tooStorSimple Snapshot Manager. Välj den här noden tooconnect och konfigurera en enhet och importera dess associerade volymer, volymer grupper och befintliga säkerhetskopior. Flera enheter kan vara anslutna tooa värddator.

* tooexpand hello-noden, klicka på pilikonen hello bredvid för**enheter**.
* toosee en meny med tillgängliga åtgärder, högerklicka på hello **enheter** nod eller högerklicka på någon av noderna i hello som visas i hello Utvidgad vy.
* toosee en lista över konfigurerade enheter klickar du på **enheter** i hello **omfång** fönstret. hello lista över enheter, tillsammans med information om varje enhet visas i hello **resultat** fönstret.

### <a name="volumes-node"></a>Volymer nod
Hej **volymer** noden representerar hello-enheter som motsvarar toohello volymer som monterats av hello värden, inklusive de som identifierats via iSCSI och de identifieras via en enhet. Använd den här noden tooview hello listan över tillgängliga volymer och tilldela enskilda volymerna toovolume grupper.

* tooexpand hello-noden, klicka på pilikonen hello bredvid för**volymer**.
* toosee en meny med tillgängliga åtgärder, högerklicka på hello **volymer** nod eller högerklicka på någon av noderna i hello som visas i hello Utvidgad vy.
* toosee en lista över volymer, klickar du på **volymer** i hello **omfång** fönstret. hello lista över volymer, tillsammans med information om varje volym visas i hello **resultat** fönstret.

### <a name="volume-groups-node"></a>Volymen Gruppnod
Volymen grupper kallas även konsekvenskontroll grupper. Varje volym är en pool med programrelaterad volymer som hjälper tooensure programkonsekvens under säkerhetskopiering. Använd hello **volym grupper** nod tooconfigure dessa grupper och tootake interaktiva säkerhetskopior eller skapa scheman för säkerhetskopiering. 

* tooexpand hello-noden, klicka på pilikonen hello bredvid för**volym grupper**.
* toosee en meny med tillgängliga åtgärder, högerklicka på hello **volym grupper** nod eller högerklicka på någon av noderna i hello som visas i hello Utvidgad vy.
* toosee en lista över volymen grupper klickar du på **volym grupper** i hello **omfång** fönstret. hello volym grupper, tillsammans med information om varje volym gruppen visas i hello **resultat** fönstret.

### <a name="backup-policies-node"></a>Säkerhetskopiera noden policyer
Principer för säkerhetskopiering är jobbscheman för lokala och molnbaserade ögonblicksbilder. Använd hello **Säkerhetskopieringsprinciper** nod toospecify hur ofta en säkerhetskopia skapas och hur länge en säkerhetskopiering ska vara kvar. 

* tooexpand hello-noden, klicka på pilikonen hello nästa för**Säkerhetskopieringsprinciper**.
* toosee en meny med tillgängliga åtgärder, högerklicka på hello **Säkerhetskopieringsprinciper** nod eller högerklicka på någon av noderna i hello som visas i hello Utvidgad vy.
* toosee en lista över principer för säkerhetskopiering, klickar du på **Säkerhetskopieringsprinciper** i hello **omfång** fönstret. hello lista över principer för säkerhetskopiering, tillsammans med information om varje princip visas i hello **resultat** fönstret.

> [!NOTE]
> Du kan lagra högst 64 säkerhetskopieringar.


### <a name="backup-catalog-node"></a>Säkerhetskopiera katalogen nod
Hej **säkerhetskopieringskatalog** nod innehåller listor över på plats och extern säkerhetskopieringar av Azure StorSimple-volymer. Den här noden är ordnad efter volym grupp, och varje grupp volymbehållare innehåller separata strukturer för lokala ögonblicksbilder (hello **lokal ögonblicksbild**s nod) och molnbaserade ögonblicksbilder (hello **Molnögonblicksbilder** nod ). När expanderats visas varje grupp volymbehållare alla lyckade hello-säkerhetskopior som gjorts interaktivt eller av en konfigurerade principen.

* tooexpand hello-noden, klicka på pilikonen hello bredvid för**säkerhetskopieringskatalog**.
* toosee en meny med tillgängliga åtgärder, högerklicka på hello **säkerhetskopieringskatalog** nod eller högerklicka på någon av noderna i hello som visas i hello Utvidgad vy.
* toosee en lista över ögonblicksbilder av säkerhetskopior, klickar du på **säkerhetskopieringskatalogen** i hello **omfång** fönstret. hello ögonblicksbilder, tillsammans med information om varje ögonblicksbild visas i hello **resultat** fönstret.

### <a name="local-snapshots-node"></a>Lokala ögonblicksbilder noden
Hej **lokala ögonblicksbilder** innehåller en lista med lokala ögonblicksbilder för en viss volym-grupp. hello nod finns under hello **säkerhetskopieringskatalog** nod i hello **omfång** fönstret. Lokala ögonblicksbilder är point-in-time-kopior av volymens data som lagras på hello Azure StorSimple-enhet. Normalt kan den här typen av säkerhetskopiering skapas och återställs snabbt. Du kan använda en lokal ögonblicksbild som en lokal säkerhetskopia.

* tooexpand hello-noden, klicka på pilikonen hello bredvid för**lokala ögonblicksbilder**.
* toosee en meny med tillgängliga åtgärder, högerklicka på hello **lokala ögonblicksbilder** nod eller högerklicka på någon av noderna i hello som visas i hello Utvidgad vy.
* toosee en lista över lokala ögonblicksbilder klickar du på **lokala ögonblicksbilder** i hello **omfång** fönstret. hello ögonblicksbilder, tillsammans med information om varje ögonblicksbild visas i hello **resultat** fönstret.

### <a name="cloud-snapshots-node"></a>Molnet ögonblicksbilder nod
Hej **Molnögonblicksbilder** innehåller en lista med molnögonblicksbilder för en viss volym-grupp. hello nod finns under hello **säkerhetskopieringskatalog** nod i hello **omfång** fönstret. Molnögonblicksbilder är i tidpunkt kopior av volymens data som lagras i hello molnet. En ögonblicksbild i molnet motsvarar tooa ögonblicksbild replikeras på ett annat, externt lagringssystem. Molnögonblicksbilder är särskilt användbart i katastrofåterställning.

* tooexpand hello-noden, klicka på pilikonen hello bredvid för**Molnögonblicksbilder**.
* toosee en meny med tillgängliga åtgärder, högerklicka på hello **Molnögonblicksbilder** nod eller högerklicka på någon av noderna i hello som visas i hello Utvidgad vy.
* toosee en lista över molnögonblicksbilder klickar du på **Molnögonblicksbilder** i hello **omfång** fönstret. hello ögonblicksbilder, tillsammans med information om varje ögonblicksbild visas i hello **resultat** fönstret.

### <a name="jobs-node"></a>Jobb nod
Hej **jobb** nod innehåller information om schemalagda körs och nyligen utförda säkerhetskopieringsjobb. 

* tooexpand hello-noden, klicka på pilikonen hello bredvid för**jobb**.
* toosee en meny med tillgängliga åtgärder, högerklicka på hello **jobb** nod eller högerklicka på någon av noderna i hello som visas i hello Utvidgad vy.
* toosee en lista över schemalagda jobb Expandera hello **jobb** noden och klicka sedan på **schemalagda**. hello tidigare konfigurerade jobb och information om varje jobb visas i hello **resultat** fönstret. 
* toosee en lista över nyligen slutförda jobb Expandera hello **jobb** noden och klicka sedan på **senaste 24 timmarna**. En lista över jobb som har slutförts i hello senaste 24 timmarna visas i hello **resultat** fönstret. Hej **resultat** fönstret innehåller även information om varje slutförda jobb.
* toosee en lista över jobb som körs för närvarande expanderar hello **jobb** noden och klicka sedan på **kör**. hello lista över jobb och information om varje jobb som körs visas i hello **resultat** fönstret.

## <a name="results-pane"></a>Resultatfönstret
Hej **resultat** är hello mittrutan i hello StorSimple Snapshot Manager UI. Den innehåller listor och detaljerad statusinformation för hello-nod som du valde i hello **omfång** fönstret.

### <a name="example"></a>Exempel
toosee hello följande exempel, klicka på hello **volym grupper** nod i hello **omfång** fönstret. Hej **resultat** rutan visar en lista över volymen grupper med information om varje grupp.

![Resultatfönstret](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

Du kan konfigurera hello information som visas i hello **resultat** rutan: Högerklicka på en nod i hello **omfång** rutan klickar du på **visa**, och klicka sedan på **Lägg till/ta bort Kolumner**.

## <a name="actions-pane"></a>Åtgärdsfönstret
Hej **åtgärder** är hello högra fönstret i hello StorSimple Snapshot Manager UI. Den innehåller en meny med åtgärder som du kan utföra på hello nod, visa eller data som du väljer i hello **omfång** rutan eller **resultat** fönstret. Hej **åtgärder** rutan innehåller hello samma kommandon som hello **åtgärd** menyer som är tillgängliga för objekt i hello **omfång** rutan och **resultat**fönstret. En beskrivning av varje åtgärd finns hello tabellen i hello **åtgärd** menyn avsnitt.

### <a name="examples"></a>Exempel
toosee hello följande exempel i hello **omfång** rutan Expandera hello **jobb** och klickar på **schemalagda**. Hej **åtgärder** fönstret hello tillgängliga åtgärder för hello **schemalagda** nod.

![Åtgärder exempel i schemalagda jobb](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

toosee fler alternativ, i hello **omfång** rutan Expandera hello **jobb** nod, klickar du på **schemalagda**, och klicka sedan på ett schemalagt jobb i hello **resultat**fönstret. Hej **åtgärder** fönstret hello tillgängliga åtgärder för hello schemalagt jobb, som visas i följande exempel hello.

![Åtgärdsfönstret jobbåtgärder exempel](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Tangentbordsnavigering och genvägar
StorSimple Snapshot Manager gör hello hjälpmedelsfunktionerna i hello Windows-operativsystem och hello Microsoft Management Console (MMC). Den omfattar också vissa tangentbord navigeringsfunktionerna och genvägar som är specifika toohello StorSimple Snapshot Manager, som beskrivs i följande avsnitt hello.

* [Tangenter för navigering](#keyboard-navigation-keys) 
* [Menyraden kortkommandon](#menu-bar-shortcut-keys) 
* [Kortkommandon för scope-fönstret](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Tangenter för navigering
hello beskrivs följande tabell hello nycklar som du kan använda användargränssnittet för toonavigate hello StorSimple Snapshot Manager. 

| Navigering nyckel | Åtgärd |
|:--- |:--- |
| Nedåtpil |Använd hello ned pilen viktiga toomove lodrätt toohello nästa objekt i en meny eller fönstret. |
| Ange |Tryck på hello RETUR viktiga toocomplete en åtgärd och fortsätt sedan toohello nästa steg. Du kan till exempel trycka på RETUR tooselect **nästa**, **OK**, eller **skapa**, och sedan gå toohello nästa steg i guiden. |
| ESC |Tryck på hello Esc viktiga tooclose en meny eller toocancel och stänga en sida. |
| F1 |Tryck på hello F1 viktiga tooview ett hjälpavsnitt för hello aktiva fönstret. |
| F5 |Tryck på hello F5 viktiga toorefresh en nod. |
| F6 |Tryck på hello F6 viktiga toomove från hello **omfång** fönstret toohello **resultat** fönstret. |
| F10 |Tryck på menyraden för hello F10 viktiga toogo toohello. |
| VÄNSTERPIL |Använd hello vänster pil viktiga toomove vågrätt från en stapel alternativet toohello tidigare menyalternativet. När du flyttar toohello föregående visas objekt på hello menyn, hello-åtgärd (eller kontext) för hello föregående objekt. |
| HÖGERPIL |Använd hello HÖGERPIL viktiga toomove vågrätt från en menyraden alternativet toohello nästa. När du flyttar visas toohello nästa objekt på hello menyn, hello-åtgärd (eller kontext) för hello nytt objekt. |
| TABB-tangenten |Använd hello fliken viktiga toomove toohello nästa ruta i hello-konsolen eller toohello nästa val eller textrutan på en sida. |
| UPPIL |Använd hello in pilen viktiga toomove lodrätt toohello föregående objekt på en meny eller fönstret. |

### <a name="menu-bar-shortcut-keys"></a>Menyraden kortkommandon
hello beskrivs följande tabell hello kortkommandon för hello menyraden. När du trycker på hello kortkommandona och hello-menyn öppnas, kan du använda menyn kortkommandon (hello understrukna nycklar på hello-menyn). Mer information om hello menyraden gå för[menyraden](#menu-bar).

| Genväg | Resultat | Menyn kortkommando | Resultat |
|:--- |:--- |:--- |:--- |
| ALT + F |Öppnas hello **filen** menyn. |N |Öppnar en ny instans i konsolen. |
|  |O |Öppnas hello **Administrationsverktyg** sidan. | |
|  |S |Sparar hello StorSimple Snapshot Manager-konsolen. | |
|  |A |Öppnas hello **Spara som** sidan. | |
|  |M |Öppnas hello **Lägg till/ta bort snapin-modulen** sidan. | |
|  |P |Öppnas hello **alternativ** sidan. | |
|  |H |Öppnar Hjälp online. | |
| ALT + A |Öppnas hello **åtgärd** menyn. |I |Aktiverar hello Visningsalternativ för import och inaktivera. |
|  |W |Öppnar en ny StorSimple Snapshot Manager-konsolen. | |
|  |F |Uppdaterar hello StorSimple Snapshot Manager-konsolen. | |
|  |L |Öppnas hello **exportera listan** sidan. | |
|  |H |Öppnar Hjälp online. | |
| ALT + V |Öppnas hello **visa** menyn. |A |Öppnas hello **Lägg till/ta bort kolumner** sidan. |
|  |U |Öppnas hello **Anpassa vy** sidan. | |
| ALT + O |Öppnas hello **Favoriter** menyn. |A |Öppnas hello **lägga till tooFavorites** sidan. |
|  |O |Öppnas hello **Ordna Favoriter** sidan. | |
| ALT + W |Öppnas hello **fönstret** menyn. |N |Öppnar ett annat fönster för StorSimple Snapshot Manager. |
|  |C |Visar alla öppna konsolfönstret i en övergripande formatmall. | |
|  |T |Visar alla öppna konsolfönstret i ett rutnät. | |
|  |I |Ordnar ikonerna i en vågrät rad på hello längst ned på skärmen. | |
| ALT + H |Öppnas hello **hjälp** menyn. |H |Öppnar Hjälp online. |
|  |T |Öppnar hello Microsoft TechNet Tech Center webbsidan. | |
|  |A |Öppnas hello **om Microsoft Management Console** sidan. | |

### <a name="scope-pane-shortcut-keys"></a>Kortkommandon för scope-fönstret
hello följande tabeller visar hello genvägen för varje nod-tangentkombinationer i hello **omfång** fönstret. 

* [Kortkommandon för enheter nod](#devices-node-shortcut-keys)
* [Kortkommandon för volymer nod](#volumes-node-shortcut-keys)
* [Kortkommandon för volymen grupper nod](#volume-groups-node-shortcut-keys)
* [Kortkommandon för säkerhetskopiering principer nod](#backup-policies-node-shortcut-keys)
* [Kortkommandon för säkerhetskopiering katalog nod](#backup-catalog-node-shortcut-keys)
* [Kortkommandon för jobb nod](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Kortkommandon för enheter nod
| Genväg till meny | Resultat |
|:--- |:--- |
| C |Öppnas hello **konfigurera en enhet** sidan. |
| D |Uppdaterar hello listan med enheter och enhetsinformation. |
| V |Öppnas hello **visa** menyn. |
| W |Öppnar en ny konsol för StorSimple Snapshot Manager fokuserar på hello **information** nod. |
| F |Uppdaterar hello StorSimple Snapshot Manager-konsolen. |
| L |Öppnas hello **exportera listan** sidan. |
| H |Öppnar Hjälp online. |

#### <a name="volumes-node-shortcut-keys"></a>Kortkommandon för volymer nod
| Genväg till meny | Resultat |
|:--- |:--- |
| V |Uppdateringar hello lista över volymer. |
| V (tryck två gånger på) |Öppnas hello **visa** menyn. |
| W |Öppnar en ny konsol för StorSimple Snapshot Manager fokuserar på hello **volymer** nod. |
| F |Uppdaterar hello StorSimple Snapshot Manager-konsolen. |
| L |Öppnas hello **exportera listan** sidan. |
| H |Öppnar Hjälp online. |

#### <a name="volume-groups-node-shortcut-keys"></a>Kortkommandon för volymen grupper nod
| Genväg till meny | Resultat |
|:--- |:--- |
| G |Öppnas hello **skapa en volym grupp** sidan. |
| V |Öppnas hello **visa** menyn. |
| W |Öppnar en ny konsol för StorSimple Snapshot Manager fokuserar på hello **volym grupper** nod. |
| F |Uppdaterar hello StorSimple Snapshot Manager-konsolen. |
| L |Öppnas hello **exportera listan** sidan. |
| H |Öppnar Hjälp online. |

#### <a name="backup-policies-node-shortcut-keys"></a>Kortkommandon för säkerhetskopiering principer nod
| Genväg till meny | Resultat |
|:--- |:--- |
| B |Öppnas hello **skapa en princip** sidan. |
| V |Öppnas hello **visa** menyn. |
| W |Öppnar en ny konsol för StorSimple Snapshot Manager fokuserar på hello **volym grupper** nod. |
| F |Uppdaterar hello StorSimple Snapshot Manager-konsolen. |
| L |Öppnas hello ** exportera listan ** sidan. |
| H |Öppnar Hjälp online. |

#### <a name="backup-catalog-node-shortcut-keys"></a>Kortkommandon för säkerhetskopiering katalog nod
| Genväg till meny | Resultat |
|:--- |:--- |
| W |Öppnar en ny konsol för StorSimple Snapshot Manager fokuserar på hello **volym grupper** nod. |
| F |Uppdaterar hello StorSimple Snapshot Manager-konsolen. |
| H |Öppnar Hjälp online. |

#### <a name="jobs-node-shortcut-keys"></a>Kortkommandon för jobb nod
| Genväg till meny | Resultat |
|:--- |:--- |
| V |Öppnas hello **visa** menyn. |
| W |Öppnar en ny konsol för StorSimple Snapshot Manager fokuserar på hello **jobb** nod. |
| F |Uppdaterar hello StorSimple Snapshot Manager-konsolen. |
| L |Öppnas hello **exportera listan** sidan. |
| H |Öppnar Hjälp online |

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[med StorSimple Snapshot Manager tooadminister din StorSimple-lösning](storsimple-snapshot-manager-admin.md).
* Lär dig hur för[använder StorSimple Snapshot Manager tooconnect och hantera enheter](storsimple-snapshot-manager-manage-devices.md).

