---
elm:
  dependencies:
    gicentre/elm-vegalite: latest

narrative-schemas:
  - coursework
---

# Distribution of structures over time

Author: Alistair Grom 964398

```elm {l=hidden}
import VegaLite exposing (..)
```

```elm {l=hidden}
names : Data
names =
    dataFromUrl "./pleiades-names-latest.csv" []


locs : Data
locs =
    dataFromUrl "./pleiades-locations-latest.csv" []


places : Data
places =
    dataFromUrl "./pleiades-places-latest.csv" []
```

## Getting started

{(aim|}

The Aim of this visualisation is to show how the distribution of structures has changed over the time periods from present to -10000 BC (or the start of the dataset). This can provide us with insights into what period we are in (roman, greek, egypytian).

From this visualisation we can see how the frequency of settlements has grown up until 500 AD.
These graphs also show how the respective empires constructed varous structures. The egyptians showing pyramids constructed until 2000 BC, we see that technology has improved in the 500 BC - 2000 BC period, with the introduction of bridges and nuraghes. A general increase in the amount of settlements also.
From 500 BC to AD 500 is where we see a great rise in productivity, likely due to the roman and greek empires being active during these times. A increase in forts, bridges, theatres, temples and so on.

From 500 AD the dataset indicates the main focus of society was to create settlements and churches with less emphasis on innovation.

{|aim)}

```elm {v}
barChart1 : Spec
barChart1 =
    let
        h =
            500

        w =
            180

        trans =
            transform
                --could be better to find the top 10 types for each filter
                << filter (fiExpr """datum.featureType == 'fort' ||
                                         datum.featureType == 'villa' ||
                                         datum.featureType == 'tomb' ||
                                         datum.featureType == 'cemetery' ||
                                         datum.featureType == 'pyramid' ||
                                         datum.featureType == 'cairn' ||
                                         datum.featureType == 'theatre' ||
                                         datum.featureType == 'church' ||
                                         datum.featureType == 'nuraghe' ||
                                         datum.featureType == 'temple'||
                                         datum.featureType == 'bridge' ||
                                         datum.featureType == 'bath' ||
                                         datum.featureType == 'monument' ||
                                         datum.featureType == 'settlement'
                                         """)

        trans1 =
            transform
                << filter (fiExpr """ (datum.maxDate <= 2100 && datum.minDate > 500) """)

        trans2 =
            transform
                << filter (fiExpr """ (datum.maxDate <= 500 && datum.minDate > -500) """)

        trans3 =
            transform
                << filter (fiExpr """ (datum.maxDate <= -500 && datum.minDate >= -2000) """)

        trans4 =
            transform
                << filter (fiExpr """ (datum.maxDate < -2000 && datum.minDate >= -10000) """)

        enc1 =
            encoding
                << position X [ pName "featureType", pTitle "" ]
                << position Y [ pAggregate opCount, pQuant ]

        spec1 =
            asSpec [ title "Present - AD 500" [], width w, height h, trans1 [], enc1 [], bar [ maColor "red", maOpacity 1 ] ]

        spec2 =
            asSpec [ title "AD 500 - 500 BC" [], width w, height h, trans2 [], enc1 [], bar [ maColor "blue", maOpacity 1 ] ]

        spec3 =
            asSpec [ title "500 BC - 2000 BC" [], width w, height h, trans3 [], enc1 [], bar [ maColor "green", maOpacity 1 ] ]

        spec4 =
            asSpec [ title "2000 BC - 10000 BC" [], width w, height h, trans4 [], enc1 [], bar [ maColor "orange", maOpacity 1 ] ]
    in
    toVegaLite
        [ locs
        , trans []
        , hConcat [ spec1, spec2, spec3, spec4 ]
        ]
```

{(vistype|}

Bar chart

{|vistype)}

{(vismapping|}

Each bar chart has a w of 180 and h of 500.

y position
: featureType category

length
: count of location

{|vismapping)}

{(dataprep|}

For the records used here, replaced names that have trailing commas ('bath,' to 'bath' and so on)
Filtered out for the feature types I wanted
aggregated to get the count of these variables

{|dataprep)}

{(limitations|}

The scale is not the same on every graph, it would increase the sense of growth of structures and trends but the dataset doesnt contain the same amount of records for each time period.
This may be better as a toggle on or off but I was unable to do that.

{|limitations)}
