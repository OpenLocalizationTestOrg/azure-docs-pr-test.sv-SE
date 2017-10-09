<!--author=alkohli last changed: 03/17/16-->

## <a name="troubleshooting-update-failures"></a>Felsökning av misslyckade uppdateringar
**Vad händer om du ser ett meddelande om att hello förberedande kontroller har misslyckats?**

Om en förkontroll misslyckas, kontrollerar du att du har tittat i hello detaljerad meddelandefältet på hello hello sidans nederkant. Detta ger vägledning som toowhich förkontroll misslyckades. hello visar följande bild en instans som ett sådant meddelande visas. I det här fallet har hello controller hälsokontrollen och maskinvara komponenten hälsokontroll misslyckats. Under hello **maskinvarustatus** avsnittet visas som båda **styrenhet 0** och **domänkontrollant 1** komponenter behöver åtgärdas.

  ![Fel vid förkontroll](./media/storsimple-install-troubleshooting/HCS_PreUpdateCheckFailed-include.png)

Du behöver toomake till att både domänkontrollanter är felfri och online. Du måste också till att alla hello maskinvarukomponenter i hello StorSimple-enhet visas toobe felfritt på sidan för underhåll av hello toomake. Sedan kan du försöka tooinstall uppdateringar. Om du inte kan toofix hello maskinvaruproblem komponent måste toocontact Microsoft-supporten för nästa steg.

**Vad händer om du får felmeddelandet ”Det gick inte att installera uppdateringar” och hello rekommendation är toorefer toohello uppdatering felsökning guiden toodetermine hello felorsaken med hello?**

En trolig orsak till detta kan vara att du inte har anslutning toohello Microsoft Update-servrar. Det här är en manuell kontroll som behöver toobe utförs. Om du förlorar anslutningen toohello uppdateringsservern misslyckas uppdateringsjobb. Du kan kontrollera hello anslutningen genom att köra följande cmdlet från hello Windows PowerShell-gränssnittet för din StorSimple-enhet hello:

 `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter>`

Kör hello cmdlet på båda domänkontrollanter.

Om du har kontrollerat hello anslutning finns, och du kan fortsätta toosee det här problemet kan du kontakta Microsoft Support för nästa steg.

**Vad händer om du ser ett update-fel när du uppdaterar din enhet tooUpdate 4 och båda hello domänkontrollanter kör Update 4?**

Startar uppdatera 4 om båda hello-domänkontrollanter finns körs hello samma programvaruversion och om det finns ett fel för uppdateringen, går hello domänkontrollanter inte i återställningsläge. Den här situationen kan uppstå om hello enheten programvara snabbkorrigering (1 uppdatering) är tillämpade tooboth hello domänkontrollanter har men andra snabbkorrigeringar (2 ordning och 3 ordning) har använts toobe. Startar uppdatering 4 går hello domänkontrollanter till återställningsläge endast om hello två domänkontrollanter kör olika versioner. 

Om hello användaren ser ett update-fel när båda domänkontrollanterna kör uppdatering 4, rekommenderar vi att de Vänta några minuter och försök sedan uppdatera. Om hello försök inte lyckas, bör de kontakta Microsoft Support.
