hello steg toounregister en processerver skiljer sig beroende på dess anslutningsstatus med hello konfigurationsservern.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Avregistrera en processerver som är i kopplat läge

1. Fjärråtkomst till hello processervern som administratör.
2. Starta hello **Kontrollpanelen** och öppna **program > Avinstallera ett program**
3. Avinstallera ett program med namnet hello **Microsoft Azure Site Recovery konfigurationsprocessen/Server**
4. När steg 3 är slutfört kan du avinstallera **Microsoft Azure Site Recovery Configuration/Process Server Dependencies** (Beroenden för Microsoft Azure Site Recovery konfigurations-/processerver)

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Avregistrera en processerver som är i frånkopplat läge

> [!WARNING]
> Använd hello nedanstående steg ska användas om det finns inget sätt toorevive hello virtuell dator på vilken hello Processervern har installerats.

1. Logga in tooyour konfigurationsservern som administratör.
2. Öppna en administrativ kommandotolk och gå toohello directory `%ProgramData%\ASR\home\svsystems\bin`.
3. Kör nu hello-kommando.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. Hello-information för hello processerver rensas från hello system.
