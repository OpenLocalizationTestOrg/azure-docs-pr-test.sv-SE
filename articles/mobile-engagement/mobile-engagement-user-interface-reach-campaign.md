---
title: "aaaAzure användargränssnitt för Mobile Engagement - nå kampanj"
description: Laern hur toocreate och hantera push notification kampanjer med Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2fe124a2-a86f-4136-81ba-a9d298ec798a
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 825e550ace63a34d1a90b10fa976a61eb15a6d04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-push-notification-campaigns"></a>Hur toocreate och hantera kampanjer för push-meddelande
Du kan använda hello Reach-avsnittet i hello UI toocreate en ny Push-kampanj med en formel genom att tillhandahålla alla hello information som du behöver toosend ett push-meddelande. hello alternativen för en Push-kampanj varierar något beroende på hello fyra kampanjtyper: meddelanden, avsökningar, Data-push både och paneler (endast Windows Phone).

### <a name="option-applies-to"></a>Alternativet gäller för:
* Språk: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)
* Kampanj: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)
* Meddelande: Meddelanden, undersökningar
* Innehåll: Unikt för varje Kampanjtyp av
* Målgrupp: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)
* Tidsintervall: meddelanden, undersökningar, paneler
* Test: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)

![Reach-Campaign1][20]

## <a name="languages"></a>Språk
Du kan använda hello språk nedrullningsbara menyn toosend en annan version av din Push-toodevices som ställs toouse olika språk. Som standard får alla enheter hello samma Push oavsett vilket språk de inställda toouse. Användare med sina enheter ange tooa olika språk får hello standard språkversion av hello Push. Många av hello push-kampanj alternativ tillåta toospecify alternativa innehåll för varje hello fler språk du väljer. 

![Reach-Campaign2][21]

### <a name="language-differences-apply-to"></a>Språkskillnader gäller för:
* Språk: Unika språk kan väljas i tillägg toohello standardspråk
* Kampanj: Samma för alla språk
* Meddelande: Unika för varje språk dessutom toohello standardspråk
* Innehåll: Unika för varje språk dessutom toohello standardspråk
* Målgrupp: Filtreras efter ett villkor för olika språk
* Tidsintervall: samma för alla språk
* Test: Skickas tooeach språk i taget

### <a name="supported-languages"></a>Språk som stöds:
* Arabiska (ar) 
* Bulgariska (bg) 
* Katalanska (ca) 
* Kinesiska (zh) 
* Kroatiska (hr) 
* Czech (CS) 
* Danska (da) 
* Nederländska (nl) 
* Engelska (en) 
* Finska (fi) 
* Franska (fr) 
* Tyska (de) 
* Grekiska (el) 
* Hebreiska (han) 
* Hindi (hög) 
* Ungerska (hu) 
* Indonesiska (id) 
* Italienska (it) 
* Japanska (ja) 
* Koreanska (ko) 
* Lettiska (lv) 
* Litauiska (lt) 
* Malajiska (macrolanguage) (ms) 
* Norska Bokmål (nb) 
* Polska (pl) 
* Portugisiska (pt) 
* Rumänska (ro) 
* Ryska (ru) 
* Serbiska (sr) 
* Slovakiska (sk) 
* Slovenska (sl) 
* Spanska (es) 
* Svenska (sv) 
* Tagalog (tl) 
* Thailändska (th) 
* Turkiska (tr) 
* Ukrainska (Storbritannien) 
* Vietnamesiska (vi) 

## <a name="campaign"></a>Kampanj
Du kan använda hello kampanj avsnittet tooset hello namn och kategori för kampanjen samt som om du planerar tooignore hello målgrupp för en Push-kampanj och skicka den här kampanjen via hello nå API (och vissa element med hello låg nivå Push-API) i stället. Kategorier kan användas med ett anpassat meddelande mallen toocontrol i appen meddelanden baserat på fördefinierade inställningar. Du kan hämta en lista över dina befintliga ”kategorier” via hello nå API.

> [!WARNING]
> Om alternativet hello ”Ignorera målgrupp, push skickas toousers via hello API” hello ”kampanj” avsnittet av en Reach-kampanj hello kampanj skickas inte automatiskt, måste toosend den manuellt via hello nå API.

![Reach-Campaign3][22]

### <a name="option-applies-to"></a>Alternativet gäller för:
* Namn: alla
* Kategori: Meddelanden, undersökningar
* Ignorera målgrupp, push skickas toousers via hello API: alla

## <a name="notification"></a>Avisering
Du kan använda hello Notification avsnittet tooset grundläggande inställningar för din push inklusive: hello titeln på hello Push hello-meddelande, en bild i appen, eller om det är stängas. Många Meddelandeinställningar är särskilda toohello plattform på enheten. Du kan välja om din push skickas ”app” eller ”out of app” eller båda. (Kom ihåg att användare kan ”välja” eller ”CEIP” ”out-of-app” push-meddelanden på hello operativsystemet nivå på sina enheter och Azure Mobile Engagement kommer inte att kunna toooverride den här inställningen. Kom ihåg att hello Reach-API som hanterar ”i app” och ”out-of-app” push-meddelanden. hello Push-API kan vara används toohandle ”out-of-app” push-meddelanden för.) Push-meddelanden kan anpassas med bilder eller HTML-innehåll, inklusive djuplänkar för att länka utanför din App eller tooanother plats i din App (Android SDK 2.1.0 eller senare avsiktshantering kategorier krävs). Du kan ändra hello ikonen eller iOS-skylt och skicka text eller web innehållet (ett popup-fönster med HTML-innehåll, URL: en länk tooanother plats i eller utanför hello app). Du kan också göra Android-enheter ring eller vibrerar med hello Push. (Kom ihåg att du kommer måste hello rätt SDK behörigheter i din Android manifest filen tooring eller vibrerar en enhet.) Det finns för närvarande inga branschen standard för Android ”översikt” storlekar eftersom skärmstorlek skiljer sig på varje enhet, men 400 × 100 bilder fungerar på nästan alla skärmstorlek.

### <a name="delivery-types"></a>Leverans typer:
* Endast utanför appen: hello-meddelande levereras när hello användare inte använda hello programmet.
* hello utanför appavisering som endast kräver ett certifikat från Apple eller Google (APN eller GCM-certifikat).
* I appen bara: hello-meddelande visas endast när hello program körs.
* hello-meddelande använder hello Capptain leverans tooreach hello systemanvändaren. Du kan anpassa hello layout/visning av din push.
* När som helst: Det här alternativet innebär att du skickar ett meddelande som antingen hello-program körs eller inte.

![Reach-Campaign4][23]

### <a name="option-applies-to"></a>Alternativet gäller för:
* Meddelande: Meddelanden, undersökningar

## <a name="content"></a>Innehåll
Du kan använda hello innehållsavsnitt toomodify hello innehållet i dina meddelanden, avsökningar, Data-push både och paneler (endast Windows Phone). hello innehåll inställningen för Push-kampanjer är särskilda toohello kampanj. 

### <a name="see-also"></a>Se även
* [UI-dokumentation – nå - Push-innehåll][Link 29]

![Reach-Campaign5][24]

## <a name="audience"></a>Målgrupp
Du kan använda hello målgruppen avsnittet toodefine en standard lista över objekt toolimit din kampanj eller gränser din kampanj baserat på egna villkor. hello standarduppsättning alternativ tooLimit din målgrupp kan du toopush tooeither nya eller gamla användare eller endast interna push-användare. Du kan också ange en kvot toolimit hello antalet användare som tar emot hello push. Du kan redigera hello-uttrycket för hur din kampanj är filtrerade tooinclude manuellt en eller flera kriterium tootarget användare. Du kan skriva en målgruppsuttrycket manuellt. Ett sådant uttryck måste uttryckligen definiera hello relationen mellan kriterierna. Ett kriterium beskrivs av en identifierare som måste börja med en bokstav och får inte innehålla blanksteg. hello relationen mellan hello kriterierna kan beskrivas med 'och', 'eller', 'not-operatorer som '(',')'. Exempel: ”Criterion1 or (Criterion1 och inte Criterion2)”.

> [!NOTE]
> Har många ingår i kampanjer, hello serversidan riktad genomsökning kan ta lång tid, särskilt om du försöker toostart hello flera kampanjer på samma gång.

* Om det är möjligt endast starta en kampanj i taget.
* Vid hello mest endast start fyra kampanjer i taget.
* Push-bara tooyour aktiva användare (kryssrutan ”engagera bara användare som kan nås med hjälp av interna Push” och ”engagera bara aktiva användare”) så att dina användare som fortfarande har hello appen har installerats och använda den måste toobe genomsöks.
  När användarna har definierats kan du använda hello simulera knappen toofind ut hur många användare får den här Push. Detta kommer att beräkna hello antal kända användare som kan vara mål för den här målgruppen (detta är en uppskattning baserad på ett slumpvist urval av användare). Tänk på att användare som har avinstallerat programmet hello ingår också i företagsmiljöer, men kan inte nås.

### <a name="see-also"></a>Se även
* [UI-dokumentationen – Reach - nytt Push kriterium][Link 28]

![Reach-Campaign6][25]

### <a name="edit-expression"></a>Redigera uttryck
![Reach-Campaign7][26]

### <a name="limit-your-audience-option-applies-to"></a>Gräns för din målgrupp alternativet gäller för:
* Engagera bara en delmängd av användare: alla (meddelanden, avsökningar, skickar Data, paneler)
* Engagera bara gamla användare: alla (meddelanden, avsökningar, skickar Data, paneler)
* Engagera bara nya användare: alla (meddelanden, avsökningar, skickar Data, paneler)
* Engagera bara inaktiva användare: meddelanden, undersökningar, paneler
* Engagera bara aktiva användare: alla (meddelanden, avsökningar, skickar Data, paneler)
* Engagera bara användare som kan nås med systemspecifika push-aviseringar: meddelanden, avsökningar

## <a name="time-frame"></a>Tidsintervall
Du kan använda hello tidsram avsnittet tooset när hello push skickas eller lämna hello tidsram tomt toostart hello kampanj omedelbart. Kom ihåg att tidszon hello slutanvändare kan starta med hello kampanj en dag tidigare än vad du förväntar dig för dina slutanvändare i Asien och skicka små batchar av push-meddelanden i taget tills alla tidszoner i hello world matchar hello tidsram som angetts för din kampanj. Med hjälp av hello slutanvändarens tidszon kan också orsaka fördröjningar i kampanjer eftersom den har toorequest hello tid från hello phone innan du startar hello push.

> [!NOTE]
> Kampanjer utan ett slutdatum kan cachelagra push-meddelanden lokalt och fortfarande visa dem efter att du manuellt fullständig kampanjer. tooavoid detta beteende, specifik sluttid för kampanjer.

### <a name="see-also"></a>Se även
* [Nå - hur Tos-planering][Link 3] 

![Reach-Campaign8][27]

### <a name="settings-apply-to"></a>Inställningar som gäller för:
* Tidsintervall: meddelanden, undersökningar, paneler

## <a name="test"></a>Testa
Du kan använda hello Test avsnittet toosend push tooyour egna test enheten innan du sparar hello kampanj. Om du har konfigurerat en anpassad språk för den här kampanjen kan testa du hello push på varje språk. Du kan konfigurera en testenhet från ”mitt konto”.

> [!NOTE]
> Inga data loggas när du använder hello-knappen på serversidan för ”test” push-meddelanden, data loggas endast för verkliga push-kampanjer.

### <a name="see-also"></a>Se även
* [UI-dokumentation – mitt konto][Link 14]

![Reach-Campaign9][28]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

