## Contributing translations

Translations are located in [resources/lang](https://github.com/Querz/mcaselector/tree/master/src/main/resources/lang),
and new language files can be added without updating the code. The name of the language file is a country-language code
with the ending `.txt`. The first argument is a valid ISO Language Code. These codes are the lower-case two-letter codes
as defined by ISO-639. The second argument to both constructors is a valid ISO Country Code. These codes are the
upper-case two-letter codes as defined by ISO-3166. A valid example would be `fr-CA.txt` for French Canadian.

The format of the language files is one translation per line, where each line is made of 2 parts. The first part is the
language key (e.g. `menu.file` for the "File" menu), the second part, separated by a semicolon `;`, is the translation
string. If a newline is required in the translation string, it can be encoded using `\n`. A semicolon doesn't need to be
escaped.

If a translation file is missing a translation, the UI will render the key instead, e.g. `[menu.file]`.

### Updating translations

If new keys were added, but a translation is missing, MCA Selector in CLI mode offers different functions to determine
which translations are missing.

| Parameter                         | Description                                                                                                                                                                                                                                                 |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--mode printMissingTranslations` | Searches all translation files and prints a list of all keys missing their translation to console.                                                                                                                                                          |
| `--mode printTranslationKeys`     | Prints all available translation keys to console.                                                                                                                                                                                                           |
| `--mode printTranslation`         | Requires the additional parameter `--locale`. `--locale` accepts an ISO country-language code as described above, OR the string `updateResources` *when run in an IDE*. The latter will add missing translation keys to the `resource/lang` files directly. |

### Pull request

The updated language files can then be added to the main project through a pull request.

---

## Updating color mappings

MCA Selector uses color pre-generated color mappings to render images of region files, where each Minecraft block (and
sometimes block state) uses the average color of the topmost texture of a block model. As long as there is no significant
change in how Minecraft handles textures, biomes and block models, these color mappings can be generated automatically.

For this, the locally installed JRE needs to support JavaFX. By running the following command, the Minecraft client and
server are automatically downloaded. The assets in the Minecraft client are parsed for the block models and textures, and
the Minecraft server generates reports. In addition, heightmap information is extracted from an automatically created
debug world, as well as structure and entity names.

```java -cp mcaselector-2.4.2.jar net.querz.mcaselector.version.mapping.generator.Main <version>```

Where `<version>` needs to be replaced by the desired Minecraft version or snapshot.

All generated mappings can then be found in `tmp/<version>/configs/`, in addition to accumulated mappings from previous
versions. Those are needed in MCA Selector's UI, when e.g. validating biome names (since older Minecraft versions may
have had different biome names). `tmp/<version>/configs/colors.json`
contains all color mappings.

Generating color mappings also relies on the file [color_properties.json](https://github.com/Querz/mcaselector/blob/master/src/main/resources/mapping/color_properties.json),
which contains specific rules for generating and rendering blocks and colors. It contains sets of block names that require
biome dependent tinting, static tinting or simply a static color, as well as information about which blocks are foliage
(important for rendering caves), water (for rendering water depth), air or transparent (ignored during rendering).

---

## Contributing code

If you would like to add a feature or fix a bug yourself, you can create a Pull Request. When adding or changing code,
please keep it readable (existing code may not be a good example) and keep the formatting generally similar to existing code.