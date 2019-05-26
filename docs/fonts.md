* XXX Explain the various ways in which custom fonts can be used in the entire UI while still being compatible with non-western languages (there are 3, but I can't remember them all now)
* XXX explain outlinewidth
* XXX multiple level inheritance
* XXX cancel flags from parent with !
* XXX various ways of writing colors

>
[4:46 PM] folk: you cancel out styleflags of a parent template with !, like styleflags="!Outline"  
[4:47 PM] folk: any color field can be "100, 100, 100", or "100, 100, 100,255" (alpha), ffffff, ffffffaa(alpha), or a range, combining colors with a dash  
[4:47 PM] folk: ffffff-ffffff  
[4:47 PM] folk: 10,10,10-20,20,20  
[4:47 PM] folk: or with alphas  
>

The way you override the fonts for the whole interface is to define new FontGroups and change the constant, instead of modifying the FontGroup directly, so
```xml
<Constant name="MetroBold" val="Fonts\MetronicforBlizzard-Bold.otf"/>
<FontGroup name="CPHeader">
	<CodepointRange font="#BlizzardGlobal"/>
	<CodepointRange font="UI\Fonts\Eurostile-Bol.otf"/>
	<CodepointRange font="#MetroBold"/>
</FontGroup>
<Constant name="FontHeader" val="CPHeader"/>
<Constant name="FontHeaderExtended" val="CPHeader"/>
```

And do the same for FontStandard and FontStandardExtended. This allows all non-ascii locales to work perfectly, no matter which font you use. Randomly making styles and assigning random .ttf/otf files to them will not work for koKR/zhTW/etc.

>
[1:21 PM] folk: also, re-templating works perfectly, which is probably worth noting  
[1:22 PM] folk: so you can take for example a style defined in core.sc2mod like  
>

```xml
<Style name="StandardTooltip" template="StandardTemplate" height="18" vjustify="Middle" hjustify="Left" styleflags="Shadow" textcolor="#TooltipBody" highlightcolor="#ColorWhite" disabledcolor="7a59ac" hotkeycolor="#ColorWhite" />
```
And add this to your font styles
```xml
<Style name="OptionLabel" font="#FontStandard" height="16" styleflags="Shadow" textcolor="#CpColorAqua" hotkeycolor="#ColorWhite" shadowoffset="1"/>
<Style name="CpTooltipText" template="OptionLabel" styleflags="InlineJustification" />
<Style name="StandardTooltip" template="CpTooltipText" />
```
