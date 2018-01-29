<!--author=alkohli last changed: 08/29/17-->

## <a name="troubleshooting-update-failures"></a>Felsökning av misslyckade uppdateringar
**Vad händer om du ser en avisering om att kontrollerna före uppgradering har misslyckats?**

Om en förkontroll misslyckas kontrollerar du att du har tittat i det detaljerade meddelandefältet längst ned på sidan. Detta ger vägledning om vilken förkontroll som misslyckades. Exempelvis kan du få ett meddelande som domänkontrollant hälsokontrollen och maskinvara komponenten hälsokontroll har misslyckats. Gå till **övervakaren > maskinvara hälsa**. Du måste se till att både domänkontrollanter är felfri och online. Du måste också se till att alla maskinvarukomponenter i StorSimple-enhet visas vara felfria i det här bladet. Du kan sedan försöka installera uppdateringarna. Om du inte kan åtgärda problemen med maskinvarukomponenterna måste du kontakta Microsoft Support angående nästa steg.

**Vad händer om du ser felmeddelandet "Could not install updates" (Det gick inte att installera uppdateringarna), och rekommendationen är att läsa felsökningsguiden för uppdateringen för att fastställa orsaken till felet?**

En trolig orsak till detta är att du inte har någon anslutning till Microsoft Update-servrarna. Det här är en manuell kontroll som behöver göras. Om du förlorar anslutningen till uppdateringsservern misslyckas uppdateringen. Du kan kontrollera anslutningen genom att köra följande cmdlet från Windows PowerShell-gränssnittet för StorSimple-enheten:

 `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter>`

Kör cmdlet:en på båda styrenheterna.

Om du har verifierat att anslutningen finns och du fortsätter att se det här problemet kontaktar du Microsoft Support angående nästa steg.

**Vad händer om du ser ett uppdateringsfel när du uppdaterar enheten till uppdatering 4 och båda styrenheterna kör uppdatering 4?**

Om båda styrenheterna kör samma programvaruversion och om det finns ett uppdateringsfel när uppdatering 4 startas försätts inte styrenheterna i återställningsläge. Den här situationen kan uppstå om snabbkorrigeringarna i enhetens programvara (1:a ordningen) tillämpas på båda styrenheterna, men andra snabbkorrigeringar (2:a och 3:e ordningen) ännu inte har tillämpats. När uppdatering 4 startar försätts styrenheterna i återställningsläge endast om de två styrenheterna kör olika programvaruversioner. 

Om användaren ser ett uppdateringsfel när båda styrenheterna kör uppdatering 4 rekommenderar vi att du väntar i några minuter och sedan försöker uppdatera igen. Om åtgärden misslyckas bör de kontakta Microsoft Support.
