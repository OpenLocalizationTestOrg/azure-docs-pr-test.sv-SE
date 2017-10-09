## <a name="prepare-your-raspberry-pi"></a>Förbered din Raspberry Pi

### <a name="install-raspbian"></a>Installera Raspbian

Om detta är hello första gången du använder din hallon Pi måste tooinstall hello Raspbian operativsystem genom att använda NOOBS på hello SD-kort som ingår i hello kit. Hej [hallon Pi Software Guide] [ lnk-install-raspbian] beskriver hur tooinstall ett operativsystem på din hallon Pi. Den här självstudiekursen förutsätts att du har installerat hello Raspbian operativsystemet på din hallon Pi.

> [!NOTE]
> hello SD-kort som ingår i hello [Microsoft Azure IoT-startpaket för hallon Pi 3] [ lnk-starter-kits] NOOBS installerat finns redan. Du kan starta hello hallon Pi från det här kortet och välj tooinstall hello Raspbian OS.

toocomplete hello maskinvaruinställningar, måste du:

- Anslut din hallon Pi toohello strömförsörjning ingår i hello kit.
- Ansluta med hello Ethernet-kabel ingår i din kit hallon Pi tooyour nätverket. Du kan också ställa in [trådlös anslutning] [ lnk-pi-wireless] för din hallon Pi.

Du har nu slutfört hello maskinvaruinställningar hallon-pi.

### <a name="sign-in-and-access-hello-terminal"></a>Logga in och komma åt hello terminal

Du har två alternativ tooaccess en miljö med terminal på din hallon Pi:

- Om du har ett tangentbord och övervaka anslutna tooyour hallon Pi kan du använda hello Raspbian GUI tooaccess ett terminalfönster.

- Åtkomst hello kommandoraden på din hallon Pi använder SSH från den stationära datorn.

#### <a name="use-a-terminal-window-in-hello-gui"></a>Använd ett terminalfönster i hello GUI

Hej standardautentiseringsuppgifterna för Raspbian är användarnamn **pi** och lösenord **hallon**. I Aktivitetsfältet i hello GUI hello kan du starta hello **Terminal** verktyget med hello-ikonen som ser ut som en bildskärm.

#### <a name="sign-in-with-ssh"></a>Logga in med SSH

Du kan använda SSH för kommandoradsåtkomst tooyour hallon Pi. hello artikel [SSH (Secure Shell)] [ lnk-pi-ssh] beskriver hur tooconfigure SSH på din hallon Pi, och hur tooconnect från [Windows] [ lnk-ssh-windows] eller [Linux och Mac OS][lnk-ssh-linux].

Logga in med användarnamn **pi** och lösenord **hallon**.

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a>Valfritt: Dela en mapp på din hallon Pi

Alternativt kan kanske du tooshare en mapp på din hallon Pi med skrivbordsmiljön. Dela en mapp kan du toouse din önskade skrivbord textredigerare (exempelvis [Visual Studio Code](https://code.visualstudio.com/) eller [Sublime Text](http://www.sublimetext.com/)) tooedit filer på din hallon Pi istället för att använda `nano` eller `vi`.

tooshare en mapp med Windows, konfigurera en Samba-server på hello hallon Pi. Du kan också använda hello inbyggda [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server med en SFTP-klient på skrivbordet.

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/