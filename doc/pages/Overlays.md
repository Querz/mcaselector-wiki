To have a better overview of what's happening in your world, MCA Selector gives the option to enable customizable
overlays. Overlays can display single values per chunk in the form of a color gradient with a minimum and maximum value.

<p align="center">
  <img src="images/Overlays/overlays.png" alt="MCA Selector window showing an overlay for InhabitedTime">
</p>

## Configuration

Overlays can be configured using `Tools --> Edit overlays`. By pressing the `N` key, it will switch to displaying
the next overlay type. When pressing `O` while displaying an overlay, it will rotate all overlays of this type. Only
when all values of an overlay are valid (e.g. minimum < maximum) and if it is set to active it will be displayed in the
main window.

<p align="center">
  <img src="images/Overlays/overlay_editor.png" alt="MCA Selector 'Edit overlays' dialog with examples">
</p>

Information about the currently rendered overlay is shown in the status bar at the bottom of the window. The status bar
will also display the actual parsed value of the currently hovered chunk.
