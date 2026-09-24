The NBT Changer modifies the world files directly by changing specific values.

<p align="center">
  <img src="images/NBT-Changer/change_nbt_query.png" alt="MCA Selector window showing the nbt changer dialog with an example query">
</p>

## Fields

The following values can be changed:

| Field                    | Type       | Description                                                                                                                                                                                                                                                                                                  |
|--------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DataVersion              | int        | Allows to change the DataVersion of the chunks. Should be used with extreme care.                                                                                                                                                                                                                            |
| InhabitedTime            | long       | This field stores the amount of game-ticks players have spent in a chunk. Impacts the local difficulty.                                                                                                                                                                                                      |
| Timestamp                | int        | The time in epoch seconds when the chunk was last saved.                                                                                                                                                                                                                                                     |
| Compression              | String/int | The compression method to use. Supported values: `GZIP`, `ZLIB`, `UNCOMPRESSED`, `LZ4`, and their .mcc equivalents (when saving a single chunk in an .mcc file, with the suffix `_EXT`. The raw signed byte values are also supported: `1`, `2`, `3`, `4`, `-127`, `-126`, `-125`, and `-125`.               |
| LastUpdate               | long       | The time in ticks since world creation when the chunk was last saved.                                                                                                                                                                                                                                        |
| Status                   | String     | The status of the chunk generation. Only recognized by Minecraft 1.13+ (DataVersion 1444+)                                                                                                                                                                                                                   |
| Biome                    | String/int | A biome name or ID. This sets all biomes of this chunk to a single biome. For a reference of biome names and IDs, have a look at the [Wiki](https://minecraft.wiki/Java_Edition_data_values#Biomes). Custom biomes can be specified by using single quotes (') around a biome ID.                            |
| ReplaceBlocks            | String     | A comma separated list of block replacements in the format `<block-name>=<block-name\|block-nbt>[;<tile-nbt>]`. Custom block names can be specified by surrounding them with single quotes (i.e. for non-vanilla blocks).                                                                                    |
| DeleteEntities           | boolean    | If set to `1` or `true`, all entities in that chunk will be deleted.                                                                                                                                                                                                                                         |
| DeleteSections           | boolean    | One or a range of section indices. A range has the format `<from>:<to>`, inclusive. Omitting `<from>` sets the lowest possible value, omitting `<to>` sets the highest possible value. `:` or `true` means _all_ sections. Multiple ranges or single indices can be defined by separating them with a comma. |
| FixStatus                | boolean    | Will look for chunks that have terrain data, but their `Status` is set to `empty` and sets their `Status` field to `full`. Having terrain data but the `Status` set to `empty` can sometimes happen when upgrading the world through a long upgrade path, e.g. from Minecraft 1.12 directly to 1.16.         |
| FixHeightmaps            | boolean    | Recalculate heightmaps.                                                                                                                                                                                                                                                                                      |
| DeleteStructureReference | String     | Deletes the specified structure references from a chunk. Multiple references can be chained with a `,` and non-vanilla references can be specified in single quotes (`'`).                                                                                                                                   |
| PreventRetrogen          | boolean    | Can be used after upgrading a world to 1.18 to prevent Minecraft from generating caves below Y=0.                                                                                                                                                                                                            |
| ForceBlend               | boolean    | Can be used to force chunks generated in 1.18 to blend with other chunks generated in 1.18.                                                                                                                                                                                                                  |
| RemoveLightingInfo       | boolean    | Removes lighting info from the chunks and (depending on the DataVersion) sets a value to force Minecraft to recalculate lights.                                                                                                                                                                              |
| LightPopulated           | byte       | Whether the light levels for the chunk have been calculated. If this is set to `0`, converting a world from 1.12.x to 1.13 will omit that chunk. Allowed values are `0` and `1`                                                                                                                              |
| Custom                   | String     | A custom one-line groovy script setting any values in the chunk's NBT data. For backwards compatibility. It is generally recommended to use the Script Editor instead.                                                                                                                                       |

Once the field is highlighted in green, the value is considered valid and will be changed. A gray field, no matter
its content, will be ignored.

## Field string

A string representation of the to-be-changed fields is printed in a text field below the editor. When entering a
valid string into this field directly, press `Enter` to parse it into the editor.

For more information about the fields have a look at the chunk format description on [Minecraft Wiki](https://minecraft.wiki/Chunk_format).

## Options

### Change

When this option is selected, the fields will only be changed if they already exist in the chunk.

### Force

When this option is selected, the fields will be forcefully added to the chunk. This only works if the chunk already exists
and it doesn't work for all fields.

### Apply to selection only

When this option is selected, only the fields in the current selection will be changed or set. If this is *not* set, it
will change / set the fields in *all* existing chunks.

---

## Script

<p align="center">
  <img src="images/NBT-Changer/change_nbt_script.png" alt="MCA Selector window showing the nbt changer dialog with an example script">
</p>

The second tab in the NBT Changer dialog is for custom scripts. Here, you can create your own Groovy script to change
any value with any condition. Scripts can be saved and loaded independently using the Script Editor's `File` menu.

The is the most basic script:

```Groovy
import net.querz.mcaselector.io.mca.ChunkData;
import net.querz.nbt.*;

void apply(ChunkData data) {
	
}
```

But more functions are supported. This is the extended script with all supported functions:

```Groovy
import net.querz.mcaselector.io.mca.ChunkData;
import net.querz.nbt.*;

void apply(ChunkData data) {
	
}

void before() {
	
}

void after() {
	
}

boolean matchesRegion(int x, int z) {
	return true;
}
```

### boolean apply(ChunkData data)

This function can be used to change any value in the chunk data. [ChunkData](https://github.com/Querz/mcaselector/blob/master/src/main/java/net/querz/mcaselector/io/mca/ChunkData.java)
contains references to the chunk data from the `region`-, `entities`-, and `poi`-files. For example, to change a chunk's
`DataVersion`, the following script can be used:

```Groovy
void apply(ChunkData data) {
	data.region()?.getData()?.putInt("DataVersion", 1234);
}
```

### void before()

This function runs *ONCE* before changing the first chunk's data. It can be used to e.g. load additional data from a file.

### void after()

This function runs *ONCE* after changing the last chunk's data. It can be used to e.g. save accumulated data to a file.

### boolean matchesRegion(int x, int z)

This function can be used to skip certain region files entirely, based on their region coordinates. For example, the
following script can be used to skip any regions where the x-coordinate is negative:

```Groovy
boolean matchesRegion(int x, int z) {
	return x >= 0;
}
```

## Existing scripts

[Here](https://github.com/arvitus/mcascripts) is an awesome source for existing scripts, such as a
[script to fill chests with items](https://github.com/arvitus/mcascripts/blob/main/modifiers/ChestFiller.groovy).