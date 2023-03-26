# Tiled-RRGA Technical Spec

## Coordinates

Each game object comes with a (x,y) pair whose coordinates are expressed in 
Tiled screen coordinates. Tiled does not report coordinates in game units. Before
we go into how to retrieve the game coordinates, all programmers must be aware
that:

> The logical y-screen-coordinate of a given coordinate (x,y) is given by 
tiled max y - given y

where tiled max y is map `height` times `tiledHeight`.

This is because the **top left corner** of the Tiled grid is the origin and 
the origin is located at the bottom left corner in our game.

To convert tiled (x,y) into game coordinate (gx, gy), we compute

`(x, y) => (x / tiledWidth, height - y / tiledHeight)`

## Rectangles

All rectangles come with properties such as width, height, x and y. Here, the
(x,y) coordinate pair is selected as the top left corner of the shape.

## Polygons

When you create a polygon (through polylines), the very _first_ point you create
is chosen to the representative point of the polygon. All subsequent points are
expressed relative to the representative. For example, if you created a 
rectangle polygon starting at (10,10) with width 30 and height 40 (in Tiled pixels), you should expect the JSON file to display something like

```json
{
    "other stuff..." : "...",
    "polygon": [
        {
            "x":0,
            "y":0
        }, 
        {
            "x":0,
            "y":40
        }, 
        {
            "x":30,
            "y":40
        }, 
        {
            "x":30,
            "y":0
        }, 
    ],
    "x" : 10,
    "y" : 10,
    "other stuff..." : "...",
}
```

pictorially, this is drawn in the order of top left (start point), bottom left, bottom right, top right then connecting back to starting point.

It appears that the first point in the list of polygon vertices is always `(0,0)`. This is not true in general, but this is true for all created shapes whose nodes are unchanged. All the vertices are labeled using a coordinate system whose **origin** is **centered** at the starting point `(x,y)`.

(**TODO**) If we want to retrieve the in-game polygon origin and the vertices, we first convert the representative (x,y) into game coordinates. Then we must first downscale all (x,y) in the vertices by the tile size (height, width) and the y-offset should be negated to adjust for the change in direction.

## Moving/Adding/Deleting Nodes in Polygons

### Moving placed nodes

As mentioned before, when you create a polygon, the first point you click defines the "origin" (x,y) of the polygon. Now, what if you move this point _relative to other points_ after creating the polygon? The result is that: you modify the vertex in the set of vertices. Under the assumption that you do not translate the entire polygon, the origin remains in-place.

### Translating the entire polygon

This will update the origin of the object but every other property should remain the same.

### Adding/Deleting Nodes

The order in which the vertices are inserted/deleted does not change the order in which edges are currently created. However, deleting nodes that define vertices will change the shape of the polygon and therefore, this is not recommended.

## Templates

Tiled comes with templates that are amazing because we can pack as many properties as we desire into a single entity class. The main technical concern is:

> instances of templates that are not modified do not have any data other than a representative (x,y) to denote a position. All other custom properties (including vertices of shapes) will not appear in the exported JSON.

Solution (**TODO**): This may solve this issue. Since templates are basically analogous to constants (before their created instance gets modified), we should also parse the templates which are saved in XML (tx) format. Alternatively, if templates are fixed, rewrite their data in JSON formatting and load them into the parser as default values for "missing" properties.

If a property of the object instance has been modified, that property will show up in the JSON value of the object. This should feel very similar to how we construct game entities where we use default values when we cannot find a requested JSON entry.

## Layers

Having separate layers for objects might be useful for displaying details in
isolation. There's little changes in the JSON parsing with separate layers.
There's a list of layers in the JSON which each layer corresponds to a set of
properties and data (objects/tile data).

## Textures

**TODO**

