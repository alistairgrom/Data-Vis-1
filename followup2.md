---
follows: coursework
---

# Religious Sites location distribution across the World

{(aim|}

This visualisation looks at buildings that have religous significance and their locations across the map. The purpose of this was to determine patterns in religion across the world.

From this visualisation we learn that the dominant site is a temple, this is probably due to the Egyptians, Greeks and Romans all constructing buildings of this type for religious reasons.

The two zoomed in visualisations show both Greece and Italy/Rome being the most populus areas for these type of structure both with high concentrations of temples and Rome with a large amount of cemeterys.

This also allows us to see there are a small cluster of Abbeys in the Caucuses, which was a surprise to me. (possibly mislabelled in the dataset)

{|aim)}

```elm {v}
settlementEnc =
    encoding
        << position Latitude [ pName "reprLat" ]
        << position Longitude [ pName "reprLong" ]
        << color [ mName "featureType" ]


churchTrans =
    transform
        << filter (fiExpr """datum.featureType == 'church' ||
                                datum.featureType == 'abbey'
                                """)


templeTrans =
    transform
        << filter (fiExpr """datum.featureType == 'temple' ||
                               datum.featureType == 'temple,' """)


mosqueTrans =
    transform
        << filter (fiExpr """datum.featureType == 'mosque' ||
                               datum.featureType == 'mosque,' """)


burialTrans =
    transform
        << filter (fiExpr """datum.featureType == 'tomb' ||
                                datum.featureType == 'cemetery'""")


transTwo =
    transform
        << filter (fiExpr """datum.featureType == 'church' ||
                                datum.featureType == 'temple' ||
                                datum.featureType == 'tomb' ||
                                datum.featureType == 'cemetery' ||
                                datum.featureType == 'mosque' ||
                                datum.featureType == 'abbey'
                                    """)


mapData =
    dataFromUrl "https://vega.github.io/vega/data/world-110m.json"
        [ topojsonFeature "countries" ]


mapSpec : Spec
mapSpec =
    asSpec
        [ mapData, geoshape [ maFill "#BBBBBB" ] ]


mapProj =
    projection [ prScale 500, prCenter 50 45, prType naturalEarth1 ]


map : Spec
map =
    let
        churchSpec =
            asSpec
                [ locs
                , settlementEnc []
                , churchTrans []
                , point [ maFilled True, maSize 20, maOpacity 0.6 ]
                ]

        templeSpec =
            asSpec
                [ locs
                , settlementEnc []
                , templeTrans []
                , point [ maFilled True, maSize 20, maOpacity 0.7 ]
                ]

        mosqueSpec =
            asSpec
                [ locs
                , settlementEnc []
                , mosqueTrans []
                , point [ maFilled True, maSize 20, maOpacity 1 ]
                ]

        burialSpec =
            asSpec
                [ locs
                , settlementEnc []
                , burialTrans []
                , point [ maFilled True, maSize 20, maOpacity 1 ]
                ]
    in
    toVegaLite [ width 900, height 700, mapProj, layer [ mapSpec, mosqueSpec, burialSpec, templeSpec, churchSpec ] ]
```

{(vistype|}

Map

{|vistype)}

{(vismapping|}

x position
latitude of location

y position
longitude of location

Points are religous sites, they are colour coded by featureType also.

{|vismapping)}

```elm {v}
italyProj =
    projection [ prScale 1550, prCenter 12.4964 41.9028 ]


mapItaly : Spec
mapItaly =
    let
        italySpec =
            asSpec
                [ locs
                , settlementEnc []
                , transTwo []
                , point [ maFilled True, maSize 20, maOpacity 1 ]
                ]
    in
    toVegaLite [ width 400, height 400, italyProj, layer [ mapSpec, italySpec ] ]


greeceProj =
    projection [ prScale 1550, prCenter 25.8243 39.0742 ]


mapGreece : Spec
mapGreece =
    let
        italySpec =
            asSpec
                [ locs
                , settlementEnc []
                , transTwo []
                , point [ maFilled True, maSize 20, maOpacity 1 ]
                ]
    in
    toVegaLite [ width 400, height 400, greeceProj, layer [ mapSpec, italySpec ] ]
```

{(dataprep|}

Filtering out to only get the `featureTypes` of religous sites.
Aggregating and counting the amounts of each feature
Changing opacity to increase readability and size of some points
Multiple views for dense parts, greece and rome.

Removal of some trailing punctuation to include instances that we not matched by filters

{|dataprep)}

{(limitations|}

The manual zooms provide the features I would like fomr a zoom feature but I struggled to implement this in elm vegalite.
I would have liked to filter by date aswell as I think the rise of churches and drop off of newly constructed temples would be interesting.

{|limitations)}
