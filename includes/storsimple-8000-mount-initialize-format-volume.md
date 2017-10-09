<!--author=SharS last changed: 9/17/15-->

#### <a name="toomount-initialize-and-format-a-volume"></a>toomount, initiera och formatera en volym
1. Starta hello Microsoft iSCSI-initieraren.
2. I hello **iSCSI-Initieraregenskaper** fönstret på hello **identifiering** klickar du på **identifiera Portal**.
3. I hello **identifiera målportal** dialogrutan Ange hello IP-adressen för din iSCSI-aktiverade nätverksgränssnitt och klicka sedan på **OK**. 
4. I hello **iSCSI-Initieraregenskaper** fönstret på hello **mål** , letar du reda hello **upptäckta mål**. hello enhetens status borde visas som **inaktiv**.
5. Välj hello målenheten och klicka sedan på **Anslut**. När hello-enheten är ansluten hello bör statusen ändras för**ansluten**. (Mer information om hur du använder hello Microsoft iSCSI-initieraren finns [installera och konfigurera Microsoft iSCSI Initiator][1]).
6. Tryck på hello Windows-tangenten + X på din Windows-värd och klicka sedan på **kör**. 
7. I hello **kör** skriver **Diskmgmt.msc**. Klicka på **OK**, och hello **Diskhantering** visas dialogrutan. hello högra panelen visar hello volymer på värden.
8. I hello **Diskhantering** hello monterade volymer visas fönster, som visas i följande illustration hello. Högerklicka på hello identifierade volymen (klicka på hello diskens namn) och klicka sedan på **Online**.
   
     ![Initiera formatera volym](./media/storsimple-8000-mount-initialize-format-volume/step7initializeformatvolume.png) 
9. Högerklicka på hello volymen (klicka på disknamnet hello) igen, och klicka sedan på **initiera**.
10. tooformat en enkel volym, utför följande steg hello:
    
    1. Välj hello volym, högerklicka på den (klicka på hello rätt område) och klicka på **ny enkel volym**.
    2. Hello i guiden Ny enkel volym anger hello volym och enhetsbeteckning och konfigurerar hello volymen som NTFS-filsystemet.
    3. Ange en storlek på allokeringsenheterna på 64 KB. Den här storleken på allokeringsenhet fungerar bra med hello dedupliceringsalgoritmer som används i hello StorSimple-lösningen.
    4. Utför en snabbformatering.

<!--Link references-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx
