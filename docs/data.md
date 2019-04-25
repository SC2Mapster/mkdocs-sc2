* XXX what are the most often asked data questions?
* XXX what concepts do we need to explain here?
* XXX a small section about how to vscode-search for examples in sc2gamedata?

XXX add in all pins from data discord channels at least

----
I feel we should link or port a set of already good data tutorials that seem to get lost noawayas, like all [prozaicmuze](https://www.sc2mapster.com/members/ProzaicMuze/threads) tuts

# Data Spaces

To add data spaces to your map, all you need to do (obviously always work with the map as .SC2Components) is create XML files and add them to Base.SC2Data/GameData.xml (if it doesn't exist, you need to create it with UTF8 and CRLF line endings) , like 
```xml
<?xml version="1.0" encoding="us-ascii"?>
<Includes>
  <Catalog path="GameData/Auras.xml"/>
  <Catalog path="GameData/Heroes/H-Tychus.xml"/>
  <Catalog path="GameData/Talents/T-Tychus.xml"/>
  <Catalog path="GameData/Zerg/Z-Birther.xml"/>
  <Catalog path="GameData/Specialists/S-Gar.xml"/>
  <Catalog path="GameData/Cleanse/C-Spawners.xml"/>
  <Catalog path="GameData/Evil/E-BrainBug.xml"/>
</Includes>
```
The reason you want to prefix the files with a unique letter per folder is to make it easier to sort in the flat representation in the data editor. Then, you can right-click any item in the data and select "Move to" to move it to any file you want. You can use dataspaces in many ways - for example; open it as a tab in the editor (like a data type), use it to focus your editing on a specific area once you've properly segregated your data, use it to more easily sort the data, and make it easier to copy and paste between maps.

The files should be created in Base.SC2Data/GameData, and you can create as many subfolders you want, and name them whatever you want. The folders are not exposed in the data editor except in the dropdown list where you select data space.

Then probably restart the editor, and you can move any data you want to any file

Here's what I put in my empty files when I create them 
<?xml version="1.0" encoding="us-ascii"?>
<Catalog>
</Catalog>
 and of course save them as UTF8, with CRLF
