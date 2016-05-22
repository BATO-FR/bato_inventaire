# Extraction des points d'arrêts transport en France depuis OSM

## Méthode manuelle en ligne de commande pour réaliser un export csv des arrêts de transport

/!\ les tags à considérer restent à discuter.
Seuls les noeuds sont extraits.

extraction des arrêts bien taggés d'après le nouveau schéma de transport public
```bash
  ./bin/osmosis --read-pbf file="chemin/vers/le/fichier/osm/france.osm.pbf" --nkv keyValueList="public_transport.platform" --write-pbf platform.osm.pbf
```

extraction des arrêts de bus taggés à l'ancienne
```bash
  ./bin/osmosis --read-pbf file="chemin/vers/le/fichier/osm/france.osm.pbf" --nkv keyValueList="highway.bus_stop" --write-pbf bus.osm.pbf
```

extraction des arrêts de tramway taggés à l'ancienne
```bash
  ./bin/osmosis --read-pbf file="chemin/vers/le/fichier/osm/france.osm.pbf" --nkv keyValueList="railway.tram_stop" --write-pbf tram.osm.pbf
```

fusion des trois extractions précédents
```bash
  ./bin/osmosis --read-pbf platform.osm.pbf --read-pbf tram.osm.pbf --read-pbf bus.osm.pbf --merge --merge --write-pbf public_transport_with_legacy.osm.pbf
```

transformation en csv
```bash
  osmconvert public_transport_with_legacy.osm.pbf --csv="@oname @id @lat @lon name railway highway public_transport" --csv-headline --csv-separator="," -o=arrets_transports_openstreetmap.csv
```
