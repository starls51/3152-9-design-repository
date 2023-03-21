Dev: 		Erik
MM/DD: 	03/19
Comments: This is a test level I made for use to develop our workflow for creating and importing levels.

>>>Files<<<
(What files are included in the test level)

.tmx: Working version of level, only included as reference or editable version of the level
	
	test_level.tmx: Working version of this level.

.tsx: Tilesets for Tiled

	clouds.tsx: Closer to final version of cloud set
	test_palette.tsx: Blocks made with our color palette, only placeholders

.json: Export version of a level

	test_level.json: Test level I exported

.png: Background, art files used in tilesets, reference image

	repeating_sky.png: Small texture to repeat in the level's background
	clouds.png & test_palette.png: May be needed by corresponding tsx file, not sure.
	map_guide.png: Reference to see specific names of each object (I used this convention: classname_#), this is for you to see what the level looks like without Tiled.


>>>Classes<<<
(Descriptions for each class name I assigned to objects)

platform: Polygon, Invisible, overlays in game art representing a walkable/hard surface. We may split this up into different classes later (platform_cloud, platform_branch) if we want different sound effects for each surface.

player_position: Point, invisible, starting coordinate of player

goal: Point, invisible, coordinate to place goal (I can change this to a polygon area if needed)

path_gbird/path_ugbird: A and B positions for a line that will be followed by a patrolling bird

lightning_dumb: Polygon representing the "dumb" (non-proximity activated) flashing lightning effect. This should fill with a repeating texture when active. I will make a unique transparent texture for this later.

wind: Polygon to be filled with wind texture

Edit 03/20:
Moved wind rotation in game. Added "simple" variant with all assets. Added super simple variant with no hazards and 8x8 size.


