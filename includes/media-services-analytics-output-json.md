hello jobbet producerar en JSON-utdata-fil som innehåller metadata om identifierade och spårade ytor. hello metadata innehåller koordinater som visar hello platsen för ytor samt en ansikte ID-nummer som anger hello spårning av som enskilda. Ansikts-ID-nummer är felbenägna tooreset under omständigheter när hello främre yta tappas bort eller överlappas i hello ram ledde till vissa individer komma tilldelade flera ID: N.

hello utdata JSON innehåller hello följande attribut:

| Element | Beskrivning |
| --- | --- |
| Version |Detta refererar toohello hello Video API-version. |
| Index | (Gäller endast tooAzure Media Redactor) definierar hello ram index för hello händelsen. |
| tidsrymd |”Tick” per sekund av hello video. |
| förskjutning |Detta är hello tidsförskjutningen för tidsstämplar. I version 1.0 av API: er för Video att detta alltid vara 0. I framtiden scenarier som vi stöder det här värdet kan ändras. |
| ramhastighet |Bildrutor per sekund av hello video. |
| fragment |hello metadata chunked upp till olika segment som kallas fragment. Varje avsnitt innehåller en start, varaktighet, antalet och händelse(r). |
| start |hello starttid för hello första händelsen i 'tick'. |
| Varaktighet |hello längd hello fragment i ”tick”. |
| interval |hello-intervall för varje händelsepost inom hello fragment i ”tick”. |
| evenemang |Varje händelse innehåller hello ytor identifieras och spåras inom den varaktigheten. Det är en matris med matris av händelser. hello yttre matrisen representerar ett tidsintervall. hello inre array består av 0 eller fler händelser som skedde vid den punkten i tid. En tom hakparentes [] innebär att inga ytor upptäcktes. |
| id |hello-ID för hello yta som spåras. Det här antalet kan oavsiktligt ändra om ett ansikte blir oupptäckt. En viss person ska ha samma ID i hela hello övergripande video hello, men det går inte att garantera på grund av toolimitations i hello identifieringsalgoritm (är spärrat, etc.) |
| x, y |hello övre vänstra hörnet X och Y-koordinaterna för hello inför avgränsningsram i en normaliserade skala 0.0 too1.0. <br/>-X och Y koordinater är relativa toolandscape alltid, så om du har en stående video (eller upp och ned, hello gäller iOS), har du tootranspose hello koordinater i enlighet med detta. |
| bredd, höjd |hello bredd och höjd hello inför avgränsningsram i en normaliserade skala 0.0 too1.0. |
| facesDetected |Detta hittas hello slutet av hello JSON-resultaten och sammanfattar hello antal ytor hello algoritm upptäcktes när hello video. Eftersom hello ID: N kan du återställa oavsiktligt om ett ansikte blir oupptäckt (t.ex. yta går utanför skärmen ser ut direkt) kan det här antalet kan inte alltid lika med hello true antal ytor i hello videon. |

