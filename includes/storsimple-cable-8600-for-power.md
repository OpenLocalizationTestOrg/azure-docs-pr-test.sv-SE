<!--author=alkohli last changed: 9/16/15-->


#### <a name="toocable-your-device-for-power"></a>toocable för enheten
> [!NOTE]
> Båda höljen på StorSimple-enheten inkluderar redundant PCMs. För varje höljet hello PCMs måste vara installerad och anslutna toodifferent power källor tooensure hög tillgänglighet.
> 
> 

1. Kontrollera att hello strömbrytare på alla hello PCMs i hello OFF-läge.
2. Ansluta hello power sladdar tooboth PCMs på primära hello-hölje. hello strömkablar identifieras i rött i hello power kablar diagrammet nedan.
3. Kontrollera att som hello två PCMs på hello primära höljet Använd separata strömkällor.
4. Koppla hello power sladdar toohello slå på hello rack distribution enheter enligt hello power kablar diagram.
5. Upprepa steg 2 till 4 för hello EBOD hölje.
6. Aktivera hello EBOD hölje genom att vända hello strömbrytare på varje PCM toohello ON position.
7. Kontrollera att hello EBOD hölje är aktiverat genom att kontrollera att hello grön indikatorer på hello baksidan hello EBOD domänkontrollant är aktiverade.
8. Aktivera hello primära höljet genom att vända varje PCM växeln toohello ON position.
9. Kontrollera att hello system är på genom att säkerställa hello enhetskontroll indikatorer har aktiverat.
10. Se till att hello anslutningen mellan hello EBOD domänkontrollant och hello enhetskontroll är aktiva genom att verifiera att hello fyra led nästa toohello SAS-port på hello EBOD domänkontrollant är grön.
    
    > [!IMPORTANT]
    > tooensure hög tillgänglighet för ditt system, rekommenderar vi strikt följa toohello power kablar schemat visas i följande diagram hello.
    > 
    > 
    
    ![Kabelansluta den 4U för ström](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Kablar**
    
    | Etikett | Beskrivning |
    |:--- |:--- |
    | 1 |Primär enhet |
    | 2 |PCM 0 |
    | 3 |PCM 1 |
    | 4 |Styrenhet 0 |
    | 5 |Kontrollant 1 |
    | 6 |EBOD styrenhet 0 |
    | 7 |EBOD kontrollant 1 |
    | 8 |EBOD hölje |
    | 9 |PDU |

