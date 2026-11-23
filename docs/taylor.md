# Taylor history

Taylor Wilderness Research Station ArcGIS Story Map: https://storymaps.arcgis.com/stories/ad15191228e2404cb5a5ddeb18b2de61

## Context 

From this Story Map, the aim is to download the content as structured data that will be possible to migrate into a CB-Essay project. 
The aim is not a copy of the styles and animations out of the Story Map, just extracting the data/content so that a human user can work on a new version starting from the original base.
The export will include 1) a folder of media files in "objects/", 2) a CSV listing the files in "_data/items.csv", 3) a folder of markdown files recording the content in "_essay/", 4) map data as geojson, as described below.

Every media file (image, pdf, video, audio, but not including externally embedded items such as YouTube videos) will be downloaded into the "objects/" directory. 

Every media file and embed/link (image, pdf, video, audio, and embeds or links such as YouTube) will be recorded in "_data/items.csv" following [CollectionBuilder-CSV metadata conventions](https://collectionbuilder.github.io/cb-docs/docs/metadata/csv_metadata/). 
The exact filename + extension will be recorded in the related CSV record in the "filename" field.
Each record will have a unique id assigned in the "objectid" column, following the pattern `twrs-001`.
Fill in the other columns with information provided in the Story Map such title from the image caption.
Leave unknown or non-relevant fields blank. 
Leave fields such as image_small and image_thumb that can be generated later blank.
If there is broken image links, provide a record and note the broken link.
Do not generate other metadata, just copy what is available in the Story Map.

Each section of content in the Story Map (broken up by the major headings or animation shifts), will be converted into Markdown and recorded into a ".md" file in the "_essay/" directory following the example "template.md".
Comments will note where images and other media features are located (approximately) in the content, using the "objectid" to refer to items in the CSV records.
Use comments to note any usual animations that should be considered. 

For each map feature, attempt to download a geojson version of the data if possible or provide a link to the embedded map layer in arcgis.

## Plan

1. **Inventory the source.** Retrieve the public ArcGIS StoryMaps item data and its resources for this story. Use the ordered story nodes as the source of truth; inventory section headings, text, media, captions/attributions, embeds, immersive transitions, and map/web-map references before producing files.
2. **Set up a traceable asset manifest.** Assign each media item, embed, and link a unique `twrs-###` object ID in story order. Track its source node and original URL during extraction so every exported file and every Markdown media comment can be checked against `_data/items.csv`.
3. **Export media and metadata.** Download available first-party media into `objects/`, retaining its filename and extension in `filename`. Add one CSV row per media item, embed, or link, copying only source-provided metadata. Preserve external URLs in the appropriate CSV field; leave generated fields blank. Record failed or inaccessible assets in the CSV and flag them for review rather than silently omitting them.
4. **Create the essay content.** Convert the story’s text nodes in source order into Markdown files under `_essay/`, using `template.md` front matter (`title` and sequential `order`). Split at major headings and meaningful immersive/animation shifts; retain subheadings and text formatting where practical. Add concise HTML comments at approximate media positions, referring to the corresponding object IDs and noting transitions or interactions that a new version may need to reconsider.
5. **Export map data where possible.** For each embedded web map, inspect its ArcGIS item and layer references. Download public feature layers as GeoJSON when available; otherwise preserve the map/layer URL and record why a local GeoJSON export was not possible. Link the map and layer references to the relevant essay location and asset record as appropriate.
6. **Validate and review the handoff.** Confirm CSV headers and unique IDs, that each downloaded file exists and matches its `filename`, that Markdown references resolve to CSV IDs, and that map exports are valid GeoJSON. Compare the output against the published story in order, and manually review broken links, missing assets, captions/attributions, and section boundaries before considering the migration complete.

## Export notes

- The export is in `_essay/`, `_data/items.csv`, `objects/`, and `maps/`. The essay contains the story sections and its bibliography; the CSV has a record for each exported media item, link, map, or map-data file.
- The national wilderness layers and 1950-present fire perimeters are large, so those local GeoJSON exports are limited to the Taylor map extent; the WesternStates layer is exported in full. Source ArcGIS services are retained in the essay comments and CSV. Raster climate layers remain links because they are not GeoJSON feature layers. Queries for the ID_Hydrology and Western Rivers services returned ArcGIS “Invalid URL” errors, so their service links are preserved without local exports.
- The Big Creek historical-image layer has 79 photo attachments. They are saved in `objects/big-creek-images/`, with each feature linked to its CSV record through `story_media_objectid` in `maps/big-creek-images.geojson`.
- Two downloads need human follow-up: `twrs-121` (the UIDAHO image returned HTTP 403) and `twrs-201` (the UMT PDF hostname could not be resolved). Both records preserve their source URLs and failure details in `_data/items.csv`.

