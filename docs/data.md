# Data
* XXX what are the most often asked data questions?
* XXX what concepts do we need to explain here?
* XXX a small section about how to vscode-search for examples in sc2gamedata?

XXX add in all pins from data discord channels at least

----
I feel we should link or port a set of already good data tutorials that seem to get lost noawayas, like all [prozaicmuze](https://www.sc2mapster.com/members/ProzaicMuze/threads) tuts

## Data Spaces

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

## Fake Abilities

If your make has a hostile player in it, you might want to use this system instead of actual Abilities. These fake abilities are much more performant, and they do not interrupt the units order queues, because the unit is Stunned while casting.

Tya uses this for his Direct Strike map and others, and folk uses it for Crap Patrol 2/CP2 Official.

>
[8:54 PM] folk: I'm still having problems creating a channeled uninterruptible AOE ability without losing the current waypoint attack-move order  
[8:55 PM] folk: if I cast it on a "target" (but all the effects build from Caster), then the waypoint remains intact after the channel, but the channel can be interrupted by the target dying or some other condition  
[8:55 PM] folk: if I cast it on self, triggered by enemies being nearby, no matter what flags I tweak on the ability, the unit always loses their current waypoint  
[8:56 PM] Tya: Probably don't want to hear this because it almost certainly requires a redesign of the ability  
[8:56 PM] Tya: But I'd use "stun" behaviors  
[8:57 PM] Tya: Stunned units preserve their current order until the stun wears off  
[8:57 PM] Tya: Which makes it excellent for channeling and casting animations  
[8:58 PM] folk: so you dont really use an ability at all for that then, you just do everything from the behaviors periodic for example?  
[8:58 PM] Tya: Pretty much  
[8:59 PM] Tya: It allows for much more fluid gameplay than the default channeling/cast mechanics because a) it doesn't interrupt orders and b) it lets units that are currently casting or channeling be given a new order to execute once the behavior ends  
[8:59 PM] folk: holy smokes this is much better indeed  
[9:00 PM] folk: so I don't have the editor up now, is it just a buff behavior with a State Flag: Stun set?  
[9:00 PM] folk: or whatever it's called  
[9:01 PM] folk: yeah, that's what the wiki says Stun: disables the unit from moving, attacking or using abilities without affecting the order queue  
>

```xml
<CBehaviorBuff id="CastOmni">
    <InfoFlags index="Hidden" value="1"/>
    <InfoIcon value="Assets\Textures\btn-ability-neutral-buildundead.dds"/>
    <Modification>
        <StateFlags index="Stun" value="1"/>
        <StateFlags index="SuppressFidgeting" value="1"/>
    </Modification>
</CBehaviorBuff>
```

>
[9:01 PM] folk: god damn, seems like I'm redesigning all the abilities  
[9:01 PM] Tya: I stole the idea from WoW and really haven't had any complaints with it  
[9:02 PM] folk: can you show the progress info in the infopanel for the "cast" or "channel"?  
[9:02 PM] folk: doesn't really matter it would just be nice  
[9:02 PM] folk: I'm already sold :P  
[9:02 PM] folk: fuck abilities  
[9:03 PM] Tya: I've never tried to be honest  
[9:03 PM] Tya: Having a dummy arm magazine ability may or may not show up  
[9:04 PM] folk: this was so inspiring it fixes everything in one swoop, thank you  
...  
[10:44 PM] folk: I got tyas fake ability system working on the first try in mere minutes, with custom generic cooldowns, generic channeling bar
>

### Generic fake ability XML
```xml
<CBehaviorBuff id="ChannelAbility1">
    <InfoFlags index="Hidden" value="1"/>
    <DisplayDuration index="Self" value="1"/>
    <DisplayDuration index="Ally" value="1"/>
    <DisplayDuration index="Neutral" value="1"/>
    <DisplayDuration index="Enemy" value="1"/>
    <BuffFlags index="Countdown" value="0"/>
</CBehaviorBuff>
<CBehaviorBuff id="ChannelAbility2">
    <InfoFlags index="Hidden" value="1"/>
    <DisplayDuration index="Self" value="1"/>
    <DisplayDuration index="Ally" value="1"/>
    <DisplayDuration index="Neutral" value="1"/>
    <DisplayDuration index="Enemy" value="1"/>
    <BuffFlags index="Countdown" value="0"/>
</CBehaviorBuff>
<CBehaviorBuff id="CooldownAbility1">
    <InfoFlags index="Hidden" value="1"/>
</CBehaviorBuff>
<CBehaviorBuff id="CooldownAbility2">
    <InfoFlags index="Hidden" value="1"/>
</CBehaviorBuff>
<CBehaviorBuff id="StunAbilityChannel">
    <InfoFlags index="Hidden" value="1"/>
    <InfoIcon value="Assets\Textures\btn-behavior-incapacitated.dds"/>
    <Modification>
        <StateFlags index="Stun" value="1"/>
        <StateFlags index="SuppressFidgeting" value="1"/>
    </Modification>
</CBehaviorBuff>
<CEffectApplyBehavior id="AbilityStunApply">
    <WhichUnit Value="Caster"/>
    <Behavior value="StunAbilityChannel"/>
</CEffectApplyBehavior>
<CEffectRemoveBehavior id="AbilityStunRemove">
    <WhichUnit Value="Caster"/>
    <BehaviorLink value="StunAbilityChannel"/>
</CEffectRemoveBehavior>
<CValidatorUnitCompareBehaviorCount id="IsNotChanneling">
    <WhichUnit Value="Caster"/>
    <Behavior value="StunAbilityChannel"/>
</CValidatorUnitCompareBehaviorCount>
<CValidatorUnitCompareBehaviorCount id="IsNotOnCooldownAbility1">
    <Behavior value="CooldownAbility1"/>
</CValidatorUnitCompareBehaviorCount>
<CValidatorUnitCompareBehaviorCount id="IsNotOnCooldownAbility2">
    <Behavior value="CooldownAbility2"/>
</CValidatorUnitCompareBehaviorCount>
```

### Example fake ability
```xml
<CBehaviorBuff id="ApocalypseSpawnSurgelings">
    <BehaviorFlags index="Permanent" value="1"/>
    <InfoFlags index="Hidden" value="1"/>
    <DisableValidatorArray value="IsNotChanneling"/>
    <DisableValidatorArray value="IsNotOnCooldownAbility2"/>
    <Period value="8"/>
    <PeriodicEffect value="SpawnSurgelingsCast"/>
</CBehaviorBuff>
<CEffectSet id="SpawnSurgelingsCast">
    <ValidatorArray value="EnemiesWithin12"/>
    <EffectArray value="AbilityStunApply"/>
    <EffectArray value="SpawnSurgelingsApplyChannelBar"/>
    <EffectArray value="SpawnSurgelingsApplyCooldown"/>
    <EffectArray value="SpawnSurgelingsChannel"/>
</CEffectSet>
<CEffectApplyBehavior id="SpawnSurgelingsApplyChannelBar">
    <ValidatorArray index="0" removed="1"/>
    <EditorCategories value=""/>
    <Behavior value="ChannelAbility2"/>
    <Flags index="UseDuration" value="1"/>
    <Duration value="7"/>
</CEffectApplyBehavior>
<CEffectApplyBehavior id="SpawnSurgelingsApplyCooldown">
    <ValidatorArray index="0" removed="1"/>
    <Behavior value="CooldownAbility2"/>
    <Flags index="UseDuration" value="1"/>
    <Duration value="23"/>
</CEffectApplyBehavior>
```

## Fake Burrow/Unburrow

### Actor XML
```xml
<On Terms="Behavior.FakeBurrowInstant.On" Send="AnimBracketStart Cover Burrow Burrow,Stand Unburrow OpeningPlayForever,Instant"/>
<On Terms="Behavior.FakeBurrowInstant.On" Send="Create FakeBurrowedSplat"/>
<On Terms="Behavior.FakeBurrowInstant.Off" Send="AnimBracketStop Cover"/>
<On Terms="Behavior.FakeBurrowInstant.Off" Send="TimerSet 4.000000,2.000000 DestroySplat"/>
<On Terms="Behavior.FakeBurrowInstant.Off" Send="Create MediumBurrowUpEffects"/>
<On Terms="Behavior.FakeBurrowInstant.Off" Send="Create BurrowUpMedium"/>
<On Terms="Behavior.FakeBurrowed.On" Send="AnimBracketStart Cover Burrow {} Unburrow OpeningPlayForever"/>
<On Terms="Behavior.FakeBurrowed.Off" Send="AnimBracketStop Cover"/>
<On Terms="Behavior.FakeBurrowed.On" Send="Create FakeBurrowedSplat"/>
<On Terms="Behavior.FakeBurrowed.Off" Send="TimerSet 4.000000,2.000000 DestroySplat"/>
<On Terms="TimerExpired; TimerName DestroySplat" Target="FakeBurrowedSplat" Send="Destroy"/>
<On Terms="Behavior.FakeBurrowed.On" Send="Create MediumBurrowDownEffects"/>
<On Terms="Behavior.FakeBurrowed.Off" Send="Create MediumBurrowUpEffects"/>
<On Terms="Behavior.FakeBurrowed.On" Send="Create BurrowDownMedium"/>
<On Terms="Behavior.FakeBurrowed.Off" Send="Create BurrowUpMedium"/>
```

### Unit XML
```xml
<AbilArray Link="FakeBurrow"/>
<AbilArray Link="FakeUnburrow"/>
```

### Rest of the XML
```xml
<CActorSplat id="FakeBurrowedSplat" parent="BurrowedSplatBase"/>
<CValidatorUnitCompareBehaviorCount id="IsFakeBurrowedInstant">
    <WhichUnit Value="Caster"/>
    <Compare value="GT"/>
    <Behavior value="FakeBurrowInstant"/>
</CValidatorUnitCompareBehaviorCount>
<CValidatorUnitCompareBehaviorCount id="IsFakeBurrowed">
    <WhichUnit Value="Caster"/>
    <Compare value="GT"/>
    <Behavior value="FakeBurrowed"/>
</CValidatorUnitCompareBehaviorCount>
<CEffectRemoveBehavior id="FakeBurrowRemoveInstant">
    <WhichUnit Value="Caster"/>
    <BehaviorLink value="FakeBurrowInstant"/>
    <Count value="1"/>
</CEffectRemoveBehavior>
<CEffectSwitch id="FakeBurrow">
    <CaseArray Validator="IsFakeBurrowed" Effect="FakeBurrowRemove"/>
    <CaseArray Validator="IsFakeBurrowedInstant" Effect="FakeBurrowRemoveInstant"/>
    <CaseDefault value="FakeBurrowApply"/>
</CEffectSwitch>
<CEffectApplyBehavior id="FakeBurrowApply">
    <WhichUnit Value="Caster"/>
    <Behavior value="FakeBurrowed"/>
</CEffectApplyBehavior>
<CEffectRemoveBehavior id="FakeBurrowRemove">
    <WhichUnit Value="Caster"/>
    <BehaviorLink value="FakeBurrowed"/>
    <Count value="1"/>
</CEffectRemoveBehavior>
<CAbilEffectInstant id="FakeUnburrow">
    <AbilSetId value="BrwU"/>
    <Effect index="0" value="FakeBurrow"/>
    <CmdButtonArray index="Execute" DefaultButtonFace="BurrowDown">
        <Flags index="ToSelection" value="1"/>
    </CmdButtonArray>
</CAbilEffectInstant>
<CAbilEffectInstant id="FakeBurrow">
    <CmdButtonArray index="Execute" DefaultButtonFace="BurrowDown">
        <Flags index="ToSelection" value="1"/>
    </CmdButtonArray>
    <AbilSetId value="BrwD"/>
</CAbilEffectInstant>
<CBehaviorBuff id="FakeBurrowInstant">
    <InfoFlags index="Hidden" value="1"/>
    <Modification>
        <ModifyFlags index="DisableAbils" value="1"/>
        <ModifyFlags index="DisableWeapons" value="1"/>
        <ModifyFlags index="SuppressMoving" value="1"/>
        <ModifyFlags index="SuppressTurning" value="1"/>
        <StateFlags index="Bury" value="1"/>
        <StateFlags index="Cloak" value="1"/>
        <StateFlags index="SuppressAttack" value="1"/>
        <StateFlags index="SuppressCollision" value="1"/>
        <StateFlags index="SuppressFidgeting" value="1"/>
        <AbilLinkEnableArray value="FakeBurrow"/>
        <AbilLinkEnableArray value="FakeUnburrow"/>
    </Modification>
</CBehaviorBuff>
<CBehaviorBuff id="FakeBurrowed">
    <InfoFlags index="Hidden" value="1"/>
    <Modification>
        <ModifyFlags index="DisableAbils" value="1"/>
        <ModifyFlags index="DisableWeapons" value="1"/>
        <ModifyFlags index="SuppressMoving" value="1"/>
        <ModifyFlags index="SuppressTurning" value="1"/>
        <StateFlags index="Bury" value="1"/>
        <StateFlags index="Cloak" value="1"/>
        <StateFlags index="SuppressAttack" value="1"/>
        <StateFlags index="SuppressCollision" value="1"/>
        <StateFlags index="SuppressFidgeting" value="1"/>
        <AbilLinkEnableArray value="FakeBurrow"/>
        <AbilLinkEnableArray value="FakeUnburrow"/>
    </Modification>
</CBehaviorBuff>
```

## Picking random points in a radius

### Radius = 0.5
```xml
<PeriodicOffsetArray value="0.08,0.05,0"/>
<PeriodicOffsetArray value="0.0,0.10,0"/>
<PeriodicOffsetArray value="-0.08,0.05,0"/>
<PeriodicOffsetArray value="-0.08,-0.05,0"/>
<PeriodicOffsetArray value="-0.0,-0.10,0"/>
<PeriodicOffsetArray value="0.08,-0.05,0"/>
<PeriodicOffsetArray value="0.20,0.0,0"/>
<PeriodicOffsetArray value="0.10,0.17,0"/>
<PeriodicOffsetArray value="-0.10,0.17,0"/>
<PeriodicOffsetArray value="-0.20,0.0,0"/>
<PeriodicOffsetArray value="-0.10,-0.17,0"/>
<PeriodicOffsetArray value="0.10,-0.17,0"/>
<PeriodicOffsetArray value="0.26,0.15,0"/>
<PeriodicOffsetArray value="0.0,0.30,0"/>
<PeriodicOffsetArray value="-0.26,0.15,0"/>
<PeriodicOffsetArray value="-0.26,-0.15,0"/>
<PeriodicOffsetArray value="-0.0,-0.30,0"/>
<PeriodicOffsetArray value="0.26,-0.15,0"/>
<PeriodicOffsetArray value="0.40,0.0,0"/>
<PeriodicOffsetArray value="0.20,0.35,0"/>
<PeriodicOffsetArray value="-0.20,0.35,0"/>
<PeriodicOffsetArray value="-0.40,0.0,0"/>
<PeriodicOffsetArray value="-0.20,-0.35,0"/>
<PeriodicOffsetArray value="0.20,-0.35,0"/>
<PeriodicOffsetArray value="0.44,0.25,0"/>
<PeriodicOffsetArray value="0.0,0.51,0"/>
<PeriodicOffsetArray value="-0.44,0.25,0"/>
<PeriodicOffsetArray value="-0.44,-0.25,0"/>
<PeriodicOffsetArray value="-0.0,-0.51,0"/>
<PeriodicOffsetArray value="0.44,-0.25,0"/>
```

### Radius = 1.0
```xml
<PeriodicOffsetArray value="0.17,0.09,0"/>
<PeriodicOffsetArray value="0.0,0.19,0"/>
<PeriodicOffsetArray value="-0.17,0.09,0"/>
<PeriodicOffsetArray value="-0.17,-0.09,0"/>
<PeriodicOffsetArray value="-0.0,-0.19,0"/>
<PeriodicOffsetArray value="0.17,-0.09,0"/>
<PeriodicOffsetArray value="0.39,0.0,0"/>
<PeriodicOffsetArray value="0.19,0.34,0"/>
<PeriodicOffsetArray value="-0.19,0.34,0"/>
<PeriodicOffsetArray value="-0.39,0.0,0"/>
<PeriodicOffsetArray value="-0.19,-0.34,0"/>
<PeriodicOffsetArray value="0.19,-0.34,0"/>
<PeriodicOffsetArray value="0.51,0.29,0"/>
<PeriodicOffsetArray value="0.0,0.59,0"/>
<PeriodicOffsetArray value="-0.51,0.29,0"/>
<PeriodicOffsetArray value="-0.51,-0.29,0"/>
<PeriodicOffsetArray value="-0.0,-0.59,0"/>
<PeriodicOffsetArray value="0.51,-0.29,0"/>
<PeriodicOffsetArray value="0.79,0.0,0"/>
<PeriodicOffsetArray value="0.39,0.68,0"/>
<PeriodicOffsetArray value="-0.39,0.68,0"/>
<PeriodicOffsetArray value="-0.79,0.0,0"/>
<PeriodicOffsetArray value="-0.39,-0.68,0"/>
<PeriodicOffsetArray value="0.39,-0.68,0"/>
<PeriodicOffsetArray value="0.85,0.49,0"/>
<PeriodicOffsetArray value="0.0,0.99,0"/>
<PeriodicOffsetArray value="-0.85,0.49,0"/>
<PeriodicOffsetArray value="-0.85,-0.49,0"/>
<PeriodicOffsetArray value="-0.0,-0.99,0"/>
<PeriodicOffsetArray value="0.85,-0.49,0"/>
```

### Radius = 1.5
```xml
<PeriodicOffsetArray value="0.25,0.15,0"/>
<PeriodicOffsetArray value="0.0,0.30,0"/>
<PeriodicOffsetArray value="-0.25,0.15,0"/>
<PeriodicOffsetArray value="-0.25,-0.15,0"/>
<PeriodicOffsetArray value="-0.0,-0.30,0"/>
<PeriodicOffsetArray value="0.25,-0.15,0"/>
<PeriodicOffsetArray value="0.60,0.0,0"/>
<PeriodicOffsetArray value="0.30,0.51,0"/>
<PeriodicOffsetArray value="-0.30,0.51,0"/>
<PeriodicOffsetArray value="-0.60,0.0,0"/>
<PeriodicOffsetArray value="-0.30,-0.51,0"/>
<PeriodicOffsetArray value="0.30,-0.51,0"/>
<PeriodicOffsetArray value="0.77,0.45,0"/>
<PeriodicOffsetArray value="0.0,0.90,0"/>
<PeriodicOffsetArray value="-0.77,0.45,0"/>
<PeriodicOffsetArray value="-0.77,-0.45,0"/>
<PeriodicOffsetArray value="-0.0,-0.90,0"/>
<PeriodicOffsetArray value="0.77,-0.45,0"/>
<PeriodicOffsetArray value="1.20,0.0,0"/>
<PeriodicOffsetArray value="0.60,1.03,0"/>
<PeriodicOffsetArray value="-0.60,1.03,0"/>
<PeriodicOffsetArray value="-1.20,0.0,0"/>
<PeriodicOffsetArray value="-0.60,-1.03,0"/>
<PeriodicOffsetArray value="0.60,-1.03,0"/>
<PeriodicOffsetArray value="1.29,0.75,0"/>
<PeriodicOffsetArray value="0.0,1.50,0"/>
<PeriodicOffsetArray value="-1.29,0.75,0"/>
<PeriodicOffsetArray value="-1.29,-0.75,0"/>
<PeriodicOffsetArray value="-0.0,-1.50,0"/>
<PeriodicOffsetArray value="1.29,-0.75,0"/>
```

### Radius = 2.0
```xml
<PeriodicOffsetArray value="0.34,0.20,0"/>
<PeriodicOffsetArray value="0.0,0.40,0"/>
<PeriodicOffsetArray value="-0.34,0.20,0"/>
<PeriodicOffsetArray value="-0.34,-0.20,0"/>
<PeriodicOffsetArray value="-0.0,-0.40,0"/>
<PeriodicOffsetArray value="0.34,-0.20,0"/>
<PeriodicOffsetArray value="0.80,0.0,0"/>
<PeriodicOffsetArray value="0.40,0.69,0"/>
<PeriodicOffsetArray value="-0.40,0.69,0"/>
<PeriodicOffsetArray value="-0.80,0.0,0"/>
<PeriodicOffsetArray value="-0.40,-0.69,0"/>
<PeriodicOffsetArray value="0.40,-0.69,0"/>
<PeriodicOffsetArray value="1.03,0.60,0"/>
<PeriodicOffsetArray value="0.0,1.20,0"/>
<PeriodicOffsetArray value="-1.03,0.60,0"/>
<PeriodicOffsetArray value="-1.03,-0.60,0"/>
<PeriodicOffsetArray value="-0.0,-1.20,0"/>
<PeriodicOffsetArray value="1.03,-0.60,0"/>
<PeriodicOffsetArray value="1.60,0.0,0"/>
<PeriodicOffsetArray value="0.80,1.38,0"/>
<PeriodicOffsetArray value="-0.80,1.38,0"/>
<PeriodicOffsetArray value="-1.60,0.0,0"/>
<PeriodicOffsetArray value="-0.80,-1.38,0"/>
<PeriodicOffsetArray value="0.80,-1.38,0"/>
<PeriodicOffsetArray value="1.73,1.0,0"/>
<PeriodicOffsetArray value="0.0,2.0,0"/>
<PeriodicOffsetArray value="-1.73,1.0,0"/>
<PeriodicOffsetArray value="-1.73,-1.0,0"/>
<PeriodicOffsetArray value="-0.0,-2.0,0"/>
<PeriodicOffsetArray value="1.73,-1.0,0"/>
```

### Radius = 2.5
```xml
<PeriodicOffsetArray value="0.43,0.25,0"/>
<PeriodicOffsetArray value="0.0,0.50,0"/>
<PeriodicOffsetArray value="-0.43,0.25,0"/>
<PeriodicOffsetArray value="-0.43,-0.25,0"/>
<PeriodicOffsetArray value="-0.0,-0.50,0"/>
<PeriodicOffsetArray value="0.43,-0.25,0"/>
<PeriodicOffsetArray value="1.0,0.0,0"/>
<PeriodicOffsetArray value="0.50,0.86,0"/>
<PeriodicOffsetArray value="-0.50,0.86,0"/>
<PeriodicOffsetArray value="-1.0,0.0,0"/>
<PeriodicOffsetArray value="-0.50,-0.86,0"/>
<PeriodicOffsetArray value="0.50,-0.86,0"/>
<PeriodicOffsetArray value="1.29,0.75,0"/>
<PeriodicOffsetArray value="0.0,1.50,0"/>
<PeriodicOffsetArray value="-1.29,0.75,0"/>
<PeriodicOffsetArray value="-1.29,-0.75,0"/>
<PeriodicOffsetArray value="-0.0,-1.50,0"/>
<PeriodicOffsetArray value="1.29,-0.75,0"/>
<PeriodicOffsetArray value="2.0,0.0,0"/>
<PeriodicOffsetArray value="1.0,1.73,0"/>
<PeriodicOffsetArray value="-1.0,1.73,0"/>
<PeriodicOffsetArray value="-2.0,0.0,0"/>
<PeriodicOffsetArray value="-1.0,-1.73,0"/>
<PeriodicOffsetArray value="1.0,-1.73,0"/>
<PeriodicOffsetArray value="2.16,1.25,0"/>
<PeriodicOffsetArray value="0.0,2.50,0"/>
<PeriodicOffsetArray value="-2.16,1.25,0"/>
<PeriodicOffsetArray value="-2.16,-1.25,0"/>
<PeriodicOffsetArray value="-0.0,-2.50,0"/>
<PeriodicOffsetArray value="2.16,-1.25,0"/>
```

### Radius = 3.0
```xml
<PeriodicOffsetArray value="0.51,0.30,0"/>
<PeriodicOffsetArray value="0.0,0.60,0"/>
<PeriodicOffsetArray value="-0.51,0.30,0"/>
<PeriodicOffsetArray value="-0.51,-0.30,0"/>
<PeriodicOffsetArray value="-0.0,-0.60,0"/>
<PeriodicOffsetArray value="0.51,-0.30,0"/>
<PeriodicOffsetArray value="1.20,0.0,0"/>
<PeriodicOffsetArray value="0.60,1.03,0"/>
<PeriodicOffsetArray value="-0.60,1.03,0"/>
<PeriodicOffsetArray value="-1.20,0.0,0"/>
<PeriodicOffsetArray value="-0.60,-1.03,0"/>
<PeriodicOffsetArray value="0.60,-1.03,0"/>
<PeriodicOffsetArray value="1.55,0.90,0"/>
<PeriodicOffsetArray value="0.0,1.80,0"/>
<PeriodicOffsetArray value="-1.55,0.90,0"/>
<PeriodicOffsetArray value="-1.55,-0.90,0"/>
<PeriodicOffsetArray value="-0.0,-1.80,0"/>
<PeriodicOffsetArray value="1.55,-0.90,0"/>
<PeriodicOffsetArray value="2.40,0.0,0"/>
<PeriodicOffsetArray value="1.20,2.07,0"/>
<PeriodicOffsetArray value="-1.20,2.07,0"/>
<PeriodicOffsetArray value="-2.40,0.0,0"/>
<PeriodicOffsetArray value="-1.20,-2.07,0"/>
<PeriodicOffsetArray value="1.20,-2.07,0"/>
<PeriodicOffsetArray value="2.59,1.50,0"/>
<PeriodicOffsetArray value="0.0,3.0,0"/>
<PeriodicOffsetArray value="-2.59,1.50,0"/>
<PeriodicOffsetArray value="-2.59,-1.50,0"/>
<PeriodicOffsetArray value="-0.0,-3.0,0"/>
<PeriodicOffsetArray value="2.59,-1.50,0"/>
```

### Radius = 3.5
```xml
<PeriodicOffsetArray value="0.60,0.35,0"/>
<PeriodicOffsetArray value="0.0,0.70,0"/>
<PeriodicOffsetArray value="-0.60,0.35,0"/>
<PeriodicOffsetArray value="-0.60,-0.35,0"/>
<PeriodicOffsetArray value="-0.0,-0.70,0"/>
<PeriodicOffsetArray value="0.60,-0.35,0"/>
<PeriodicOffsetArray value="1.40,0.0,0"/>
<PeriodicOffsetArray value="0.70,1.21,0"/>
<PeriodicOffsetArray value="-0.70,1.21,0"/>
<PeriodicOffsetArray value="-1.40,0.0,0"/>
<PeriodicOffsetArray value="-0.70,-1.21,0"/>
<PeriodicOffsetArray value="0.70,-1.21,0"/>
<PeriodicOffsetArray value="1.81,1.04,0"/>
<PeriodicOffsetArray value="0.0,2.09,0"/>
<PeriodicOffsetArray value="-1.81,1.04,0"/>
<PeriodicOffsetArray value="-1.81,-1.04,0"/>
<PeriodicOffsetArray value="-0.0,-2.09,0"/>
<PeriodicOffsetArray value="1.81,-1.04,0"/>
<PeriodicOffsetArray value="2.80,0.0,0"/>
<PeriodicOffsetArray value="1.40,2.42,0"/>
<PeriodicOffsetArray value="-1.40,2.42,0"/>
<PeriodicOffsetArray value="-2.80,0.0,0"/>
<PeriodicOffsetArray value="-1.40,-2.42,0"/>
<PeriodicOffsetArray value="1.40,-2.42,0"/>
<PeriodicOffsetArray value="3.03,1.75,0"/>
<PeriodicOffsetArray value="0.0,3.50,0"/>
<PeriodicOffsetArray value="-3.03,1.75,0"/>
<PeriodicOffsetArray value="-3.03,-1.75,0"/>
<PeriodicOffsetArray value="-0.0,-3.50,0"/>
<PeriodicOffsetArray value="3.03,-1.75,0"/>
```

### Radius = 4.0
```xml
<PeriodicOffsetArray value="0.69,0.40,0"/>
<PeriodicOffsetArray value="0.0,0.80,0"/>
<PeriodicOffsetArray value="-0.69,0.40,0"/>
<PeriodicOffsetArray value="-0.69,-0.40,0"/>
<PeriodicOffsetArray value="-0.0,-0.80,0"/>
<PeriodicOffsetArray value="0.69,-0.40,0"/>
<PeriodicOffsetArray value="1.60,0.0,0"/>
<PeriodicOffsetArray value="0.80,1.38,0"/>
<PeriodicOffsetArray value="-0.80,1.38,0"/>
<PeriodicOffsetArray value="-1.60,0.0,0"/>
<PeriodicOffsetArray value="-0.80,-1.38,0"/>
<PeriodicOffsetArray value="0.80,-1.38,0"/>
<PeriodicOffsetArray value="2.07,1.20,0"/>
<PeriodicOffsetArray value="0.0,2.40,0"/>
<PeriodicOffsetArray value="-2.07,1.20,0"/>
<PeriodicOffsetArray value="-2.07,-1.20,0"/>
<PeriodicOffsetArray value="-0.0,-2.40,0"/>
<PeriodicOffsetArray value="2.07,-1.20,0"/>
<PeriodicOffsetArray value="3.20,0.0,0"/>
<PeriodicOffsetArray value="1.60,2.77,0"/>
<PeriodicOffsetArray value="-1.60,2.77,0"/>
<PeriodicOffsetArray value="-3.20,0.0,0"/>
<PeriodicOffsetArray value="-1.60,-2.77,0"/>
<PeriodicOffsetArray value="1.60,-2.77,0"/>
<PeriodicOffsetArray value="3.46,2.0,0"/>
<PeriodicOffsetArray value="0.0,4.0,0"/>
<PeriodicOffsetArray value="-3.46,2.0,0"/>
<PeriodicOffsetArray value="-3.46,-2.0,0"/>
<PeriodicOffsetArray value="-0.0,-4.0,0"/>
<PeriodicOffsetArray value="3.46,-2.0,0"/>
```

### Radius = 4.5
```xml
<PeriodicOffsetArray value="0.78,0.45,0"/>
<PeriodicOffsetArray value="0.0,0.90,0"/>
<PeriodicOffsetArray value="-0.78,0.45,0"/>
<PeriodicOffsetArray value="-0.78,-0.45,0"/>
<PeriodicOffsetArray value="-0.0,-0.90,0"/>
<PeriodicOffsetArray value="0.78,-0.45,0"/>
<PeriodicOffsetArray value="1.80,0.0,0"/>
<PeriodicOffsetArray value="0.90,1.56,0"/>
<PeriodicOffsetArray value="-0.90,1.56,0"/>
<PeriodicOffsetArray value="-1.80,0.0,0"/>
<PeriodicOffsetArray value="-0.90,-1.56,0"/>
<PeriodicOffsetArray value="0.90,-1.56,0"/>
<PeriodicOffsetArray value="2.34,1.35,0"/>
<PeriodicOffsetArray value="0.0,2.70,0"/>
<PeriodicOffsetArray value="-2.34,1.35,0"/>
<PeriodicOffsetArray value="-2.34,-1.35,0"/>
<PeriodicOffsetArray value="-0.0,-2.70,0"/>
<PeriodicOffsetArray value="2.34,-1.35,0"/>
<PeriodicOffsetArray value="3.60,0.0,0"/>
<PeriodicOffsetArray value="1.80,3.12,0"/>
<PeriodicOffsetArray value="-1.80,3.12,0"/>
<PeriodicOffsetArray value="-3.60,0.0,0"/>
<PeriodicOffsetArray value="-1.80,-3.12,0"/>
<PeriodicOffsetArray value="1.80,-3.12,0"/>
<PeriodicOffsetArray value="3.90,2.25,0"/>
<PeriodicOffsetArray value="0.0,4.51,0"/>
<PeriodicOffsetArray value="-3.90,2.25,0"/>
<PeriodicOffsetArray value="-3.90,-2.25,0"/>
<PeriodicOffsetArray value="-0.0,-4.51,0"/>
<PeriodicOffsetArray value="3.90,-2.25,0"/>
```

### Radius = 5.0
```xml
<PeriodicOffsetArray value="0.86,0.50,0"/>
<PeriodicOffsetArray value="0.0,1.0,0"/>
<PeriodicOffsetArray value="-0.86,0.50,0"/>
<PeriodicOffsetArray value="-0.86,-0.50,0"/>
<PeriodicOffsetArray value="-0.0,-1.0,0"/>
<PeriodicOffsetArray value="0.86,-0.50,0"/>
<PeriodicOffsetArray value="2.0,0.0,0"/>
<PeriodicOffsetArray value="1.0,1.73,0"/>
<PeriodicOffsetArray value="-1.0,1.73,0"/>
<PeriodicOffsetArray value="-2.0,0.0,0"/>
<PeriodicOffsetArray value="-1.0,-1.73,0"/>
<PeriodicOffsetArray value="1.0,-1.73,0"/>
<PeriodicOffsetArray value="2.60,1.50,0"/>
<PeriodicOffsetArray value="0.0,3.0,0"/>
<PeriodicOffsetArray value="-2.60,1.50,0"/>
<PeriodicOffsetArray value="-2.60,-1.50,0"/>
<PeriodicOffsetArray value="-0.0,-3.0,0"/>
<PeriodicOffsetArray value="2.60,-1.50,0"/>
<PeriodicOffsetArray value="4.0,0.0,0"/>
<PeriodicOffsetArray value="2.0,3.47,0"/>
<PeriodicOffsetArray value="-2.0,3.47,0"/>
<PeriodicOffsetArray value="-4.0,0.0,0"/>
<PeriodicOffsetArray value="-2.0,-3.47,0"/>
<PeriodicOffsetArray value="2.0,-3.47,0"/>
<PeriodicOffsetArray value="4.33,2.50,0"/>
<PeriodicOffsetArray value="0.0,5.01,0"/>
<PeriodicOffsetArray value="-4.33,2.50,0"/>
<PeriodicOffsetArray value="-4.33,-2.50,0"/>
<PeriodicOffsetArray value="-0.0,-5.01,0"/>
<PeriodicOffsetArray value="4.33,-2.50,0"/>
```

### Radius = 5.5
```xml
<PeriodicOffsetArray value="0.95,0.55,0"/>
<PeriodicOffsetArray value="0.0,1.10,0"/>
<PeriodicOffsetArray value="-0.95,0.55,0"/>
<PeriodicOffsetArray value="-0.95,-0.55,0"/>
<PeriodicOffsetArray value="-0.0,-1.10,0"/>
<PeriodicOffsetArray value="0.95,-0.55,0"/>
<PeriodicOffsetArray value="2.20,0.0,0"/>
<PeriodicOffsetArray value="1.10,1.90,0"/>
<PeriodicOffsetArray value="-1.10,1.90,0"/>
<PeriodicOffsetArray value="-2.20,0.0,0"/>
<PeriodicOffsetArray value="-1.10,-1.90,0"/>
<PeriodicOffsetArray value="1.10,-1.90,0"/>
<PeriodicOffsetArray value="2.86,1.65,0"/>
<PeriodicOffsetArray value="0.0,3.30,0"/>
<PeriodicOffsetArray value="-2.86,1.65,0"/>
<PeriodicOffsetArray value="-2.86,-1.65,0"/>
<PeriodicOffsetArray value="-0.0,-3.30,0"/>
<PeriodicOffsetArray value="2.86,-1.65,0"/>
<PeriodicOffsetArray value="4.40,0.0,0"/>
<PeriodicOffsetArray value="2.20,3.81,0"/>
<PeriodicOffsetArray value="-2.20,3.81,0"/>
<PeriodicOffsetArray value="-4.40,0.0,0"/>
<PeriodicOffsetArray value="-2.20,-3.81,0"/>
<PeriodicOffsetArray value="2.20,-3.81,0"/>
<PeriodicOffsetArray value="4.77,2.75,0"/>
<PeriodicOffsetArray value="0.0,5.51,0"/>
<PeriodicOffsetArray value="-4.77,2.75,0"/>
<PeriodicOffsetArray value="-4.77,-2.75,0"/>
<PeriodicOffsetArray value="-0.0,-5.51,0"/>
<PeriodicOffsetArray value="4.77,-2.75,0"/>
```

### Radius = 6.0
```xml
<PeriodicOffsetArray value="1.03,0.59,0"/>
<PeriodicOffsetArray value="0.0,1.19,0"/>
<PeriodicOffsetArray value="-1.03,0.59,0"/>
<PeriodicOffsetArray value="-1.03,-0.59,0"/>
<PeriodicOffsetArray value="-0.0,-1.19,0"/>
<PeriodicOffsetArray value="1.03,-0.59,0"/>
<PeriodicOffsetArray value="2.39,0.0,0"/>
<PeriodicOffsetArray value="1.19,2.07,0"/>
<PeriodicOffsetArray value="-1.19,2.07,0"/>
<PeriodicOffsetArray value="-2.39,0.0,0"/>
<PeriodicOffsetArray value="-1.19,-2.07,0"/>
<PeriodicOffsetArray value="1.19,-2.07,0"/>
<PeriodicOffsetArray value="3.11,1.79,0"/>
<PeriodicOffsetArray value="0.0,3.59,0"/>
<PeriodicOffsetArray value="-3.11,1.79,0"/>
<PeriodicOffsetArray value="-3.11,-1.79,0"/>
<PeriodicOffsetArray value="-0.0,-3.59,0"/>
<PeriodicOffsetArray value="3.11,-1.79,0"/>
<PeriodicOffsetArray value="4.79,0.0,0"/>
<PeriodicOffsetArray value="2.39,4.14,0"/>
<PeriodicOffsetArray value="-2.39,4.14,0"/>
<PeriodicOffsetArray value="-4.79,0.0,0"/>
<PeriodicOffsetArray value="-2.39,-4.14,0"/>
<PeriodicOffsetArray value="2.39,-4.14,0"/>
<PeriodicOffsetArray value="5.18,2.99,0"/>
<PeriodicOffsetArray value="0.0,5.99,0"/>
<PeriodicOffsetArray value="-5.18,2.99,0"/>
<PeriodicOffsetArray value="-5.18,-2.99,0"/>
<PeriodicOffsetArray value="-0.0,-5.99,0"/>
<PeriodicOffsetArray value="5.18,-2.99,0"/>
```

### Radius = 6.5
```xml
<PeriodicOffsetArray value="1.12,0.65,0"/>
<PeriodicOffsetArray value="0.0,1.30,0"/>
<PeriodicOffsetArray value="-1.12,0.65,0"/>
<PeriodicOffsetArray value="-1.12,-0.65,0"/>
<PeriodicOffsetArray value="-0.0,-1.30,0"/>
<PeriodicOffsetArray value="1.12,-0.65,0"/>
<PeriodicOffsetArray value="2.60,0.0,0"/>
<PeriodicOffsetArray value="1.30,2.25,0"/>
<PeriodicOffsetArray value="-1.30,2.25,0"/>
<PeriodicOffsetArray value="-2.60,0.0,0"/>
<PeriodicOffsetArray value="-1.30,-2.25,0"/>
<PeriodicOffsetArray value="1.30,-2.25,0"/>
<PeriodicOffsetArray value="3.38,1.95,0"/>
<PeriodicOffsetArray value="0.0,3.90,0"/>
<PeriodicOffsetArray value="-3.38,1.95,0"/>
<PeriodicOffsetArray value="-3.38,-1.95,0"/>
<PeriodicOffsetArray value="-0.0,-3.90,0"/>
<PeriodicOffsetArray value="3.38,-1.95,0"/>
<PeriodicOffsetArray value="5.20,0.0,0"/>
<PeriodicOffsetArray value="2.60,4.51,0"/>
<PeriodicOffsetArray value="-2.60,4.51,0"/>
<PeriodicOffsetArray value="-5.20,0.0,0"/>
<PeriodicOffsetArray value="-2.60,-4.51,0"/>
<PeriodicOffsetArray value="2.60,-4.51,0"/>
<PeriodicOffsetArray value="5.63,3.25,0"/>
<PeriodicOffsetArray value="0.0,6.51,0"/>
<PeriodicOffsetArray value="-5.63,3.25,0"/>
<PeriodicOffsetArray value="-5.63,-3.25,0"/>
<PeriodicOffsetArray value="-0.0,-6.51,0"/>
<PeriodicOffsetArray value="5.63,-3.25,0"/>
```

### Radius = 7.0
```xml
<PeriodicOffsetArray value="1.21,0.70,0"/>
<PeriodicOffsetArray value="0.0,1.40,0"/>
<PeriodicOffsetArray value="-1.21,0.70,0"/>
<PeriodicOffsetArray value="-1.21,-0.70,0"/>
<PeriodicOffsetArray value="-0.0,-1.40,0"/>
<PeriodicOffsetArray value="1.21,-0.70,0"/>
<PeriodicOffsetArray value="2.80,0.0,0"/>
<PeriodicOffsetArray value="1.40,2.42,0"/>
<PeriodicOffsetArray value="-1.40,2.42,0"/>
<PeriodicOffsetArray value="-2.80,0.0,0"/>
<PeriodicOffsetArray value="-1.40,-2.42,0"/>
<PeriodicOffsetArray value="1.40,-2.42,0"/>
<PeriodicOffsetArray value="3.64,2.10,0"/>
<PeriodicOffsetArray value="0.0,4.20,0"/>
<PeriodicOffsetArray value="-3.64,2.10,0"/>
<PeriodicOffsetArray value="-3.64,-2.10,0"/>
<PeriodicOffsetArray value="-0.0,-4.20,0"/>
<PeriodicOffsetArray value="3.64,-2.10,0"/>
<PeriodicOffsetArray value="5.60,0.0,0"/>
<PeriodicOffsetArray value="2.80,4.85,0"/>
<PeriodicOffsetArray value="-2.80,4.85,0"/>
<PeriodicOffsetArray value="-5.60,0.0,0"/>
<PeriodicOffsetArray value="-2.80,-4.85,0"/>
<PeriodicOffsetArray value="2.80,-4.85,0"/>
<PeriodicOffsetArray value="6.07,3.50,0"/>
<PeriodicOffsetArray value="0.0,7.01,0"/>
<PeriodicOffsetArray value="-6.07,3.50,0"/>
<PeriodicOffsetArray value="-6.07,-3.50,0"/>
<PeriodicOffsetArray value="-0.0,-7.01,0"/>
<PeriodicOffsetArray value="6.07,-3.50,0"/>
```

## Proper Aura

This aura is reapplied only when needed, so it doesn't trigger lots of behaviors on/off.

```xml
<CValidatorUnitCompareBehaviorCount id="ShouldApplyHailofLead">
    <Behavior value="HailofLead"/>
</CValidatorUnitCompareBehaviorCount>
<CEffectApplyBehavior id="HailofLeadApply">
    <ValidatorArray index="0" value="ShouldApplyHailofLead"/>
    <Behavior value="HailofLead"/>
</CEffectApplyBehavior>
<CEffectEnumArea id="HailofLeadSearch">
    <SearchFilters value="-;Self,Neutral,Enemy,Air,Robotic,Structure,User1,Worker,RawResource,HarvestableResource,Missile,Item,UnderConstruction,Dead,Hidden,Hallucination,Summoned"/>
    <AreaArray Radius="7" Effect="HailofLeadApply"/>
</CEffectEnumArea>
<CBehaviorBuff id="HailofLead">
    <Alignment value="Positive"/>
    <InfoIcon value="Assets\Textures\btn-upgrade-nova-shotgun.dds"/>
    <RemoveValidatorArray value="AuraRangeSwitch"/>
    <Modification AttackSpeedMultiplier="1.6"/>
</CBehaviorBuff>
<CBehaviorBuff id="HailofLeadSearcher">
    <InfoIcon value="Assets\Textures\btn-upgrade-nova-shotgun.dds"/>
    <Period value="1"/>
    <PeriodicEffect value="HailofLeadSearch"/>
    <InfoFlags index="Hidden" value="1"/>
</CBehaviorBuff>
<CValidatorCondition id="AuraRangeSwitch">
    <IfArray Test="MegamanUnlocked" Return="AuraRangeValidatorMegaman"/>
    <Else value="AuraRangeValidatorStandard"/>
</CValidatorCondition>
<CValidatorLocationCompareRange id="AuraRangeValidatorMegaman">
    <WhichLocation Value="TargetUnit"/>
    <Compare value="LE"/>
    <Range value="9.5"/>
</CValidatorLocationCompareRange>
<CValidatorLocationCompareRange id="AuraRangeValidatorStandard">
    <WhichLocation Value="TargetUnit"/>
    <Compare value="LE"/>
    <Range value="7.5"/>
</CValidatorLocationCompareRange>
```

If the source/bearer of the aura can die, the aura buff needs `<RemoveValidatorArray value="SourceNotDead"/>`, put that before `AuraRangeSwitch`, since it's faster to process.

```xml
<CValidatorPlayerRequirement id="MegamanUnlocked">
    <WhichPlayer Value="Caster"/>
    <Find value="1"/>
    <UnitSelectionNotRequired value="1"/>
    <Value value="MegamanLearned"/>
</CValidatorPlayerRequirement>
```


