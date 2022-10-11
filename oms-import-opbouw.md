# OMS Import opbouw

Een OMS import bestand heeft dezelde opbouw als het export formaat, minus het paging blok.
Daarnaast zijn er andere eisen:

- Een importbestand bevat alle benodigde informatie: er kan niet worden gepagineerd.
- Observations staan zoveel mogelijk in een bepaalde volgorde:
  - Ongekoppelde Observations eerst
  - Daarna Observations die gekoppeld zijn aan ongekoppelde Observations
  - Daarna de resterense Observations

Hiermee kunnen meervoudige updates worden voorkomen.
