<!--author=alkohli last changed:02/22/16-->

#### <a name="tooattach-hello-sas-cables"></a>tooattach hello SAS-kablar
1. Identifiera primära hello och hello EBOD bilagor. hello två höljen kan identifieras genom att titta på sina respektive tillbaka plan. Se hello följande bild för vägledning. 
   
    ![Säkerhetskopiera plan av primära och EBOD bilagor](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Bakifrån primära och EBOD bilagor**
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |Primär enhet |
   | 2 |EBOD hölje |
2. Leta upp hello serienummer på hello primära och hello EBOD bilagor. hello serienummer etiketten är anbringad toohello tillbaka bort för varje enhet. hello serienummer måste vara identiska på båda höljen. [Kontakta Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) omedelbart om hello serienummer inte matchar. Se följande illustration toolocate hello serienummer hello.
   
    ![Bakifrån höljet visar serienummer](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Platsen för serienummer etikett**
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |Ta bort av hello-omslutning |
3. Använd hello ges SAS-kablar tooconnect hello EBOD hölje toohello primära höljet enligt följande:
   
   1. Identifiera hello fyra SAS-portar på hello primära höljet och hello EBOD hölje. hello SAS-portar är märkta som EBOD på hello primära höljet och motsvarar tooport A på hello EBOD hölje som hello SAS går bilden nedan.
   2. Använd hello tillhandahålls SAS-kablar tooconnect hello EBOD port tooport A.
   3. Hej EBOD port på styrenhet 0 ska vara anslutna toohello port A på EBOD styrenhet 0. Hej EBOD port på en domänkontrollant 1 ska vara anslutna toohello port A på EBOD kontrollant 1. Se följande illustration anvisningar hello. 
      
      ![SAS-kablar för din enhet](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **SAS-kablar**
      
      | Etikett | Beskrivning |
      |:--- |:--- |
      | A |Primär enhet |
      | B |EBOD hölje |
      | 1 |Styrenhet 0 |
      | 2 |Kontrollant 1 |
      | 3 |EBOD styrenhet 0 |
      | 4 |EBOD kontrollant 1 |
      | 5, 6 |SAS-portar på primära hölje (märkt EBOD) |
      | 7, 8 |SAS-portar på EBOD hölje (Port A) |

