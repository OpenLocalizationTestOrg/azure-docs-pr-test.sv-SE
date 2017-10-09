Nu när du har lagt till en utlösare, dess tid toodo något intressant med hello data som genereras av hello-utlösare. Följ dessa steg tooadd en hello **SFTP - extrahera mappen** åtgärd. Den här åtgärden extraherar hello innehållet i en fil om hello villkoren är uppfyllda. 

tooconfigure hello den här åtgärden måste du tooprovide hello följande information. Du kommer att märka att det är enkelt toouse data som genereras av hello utlösare som indata för några av hello egenskaper för ny hello-fil:

| SFTP - extrahera mappen egenskapen | Beskrivning |
| --- | --- |
| Sökvägen till källfilen Arkiv |Det här är hello sökväg för hello-fil som extraheras. Du kan välja en av hello token från en tidigare åtgärd eller bläddra hello SFTP server toofind hello sökväg. |
| Målmappens sökväg |Det här är hello sökväg där hello extraherade filerna ska placeras. Du kan välja ett av hello token från en tidigare åtgärd som hello målsökväg eller bläddra hello SFTP-servern och välj en sökväg. |
| Vill du ersätta? |Anger om en fil med hello samma namn som du extraherade hello-fil hittas i hello målmappens sökväg om hello befintlig fil ska skrivas över eller inte. |

Vi börjar med att lägga till hello åtgärd tooextract hello filer om hello villkoret som definierats tidigare utvärderas för*SANT*. 

1. Välj **lägga till en åtgärd**.        
   ![SFTP åtgärd villkoret bild 6](./media/connectors-create-api-sftp/condition-6.png)   
2. Välj hello **SFTP - extrahera mappen** åtgärd      
   ![Bild 7 till SFTP åtgärd villkor](./media/connectors-create-api-sftp/condition-7.png)   
3. Välj **sökvägen till källfilen Arkiv**              
   ![SFTP åtgärd villkoret bild 9](./media/connectors-create-api-sftp/condition-9.png)   
4. Välj hello **filsökväg** token. Detta anger att du kommer att använda hello filsökvägen till hello-fil som hello utlösare som hello Arkiv sökvägen till källfilen.           
   ![SFTP åtgärd villkoret bild 10](./media/connectors-create-api-sftp/condition-10.png)   
5. Välj **målmappens sökväg**           
   ![Bild av SFTP åtgärder för villkoret 11](./media/connectors-create-api-sftp/condition-11.png)   
6. Välj hello **filsökväg** token. Detta anger att du kommer att använda hello filsökvägen till hello-fil som hello utlösaren påträffades som hello målsökvägen för hello extraherade filer.   
7. Ange *\ExtractedFile* i hello **målmappsökvägen** kontroll. Gör det bara efter hello filen sökväg token i hello mål mappen sökväg kontroll.         
   ![Bild av SFTP åtgärder för villkoret 12](./media/connectors-create-api-sftp/condition-12.png)   
8. Ange *SANT* i hello **överskrivning?* kontrollen tooindicate som befintliga filer ska skrivas över om de har samma namn som hello hello extraherade filer.      
   ![Bild av SFTP åtgärder för villkoret 13](./media/connectors-create-api-sftp/condition-13.png)   
9. Spara hello ändringar tooyour arbetsflöde  

