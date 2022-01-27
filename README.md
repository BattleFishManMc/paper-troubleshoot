# Spigot/Paper Common Exploit Issues

This document is here to offer configuration suggestions for common problems people 
experience when playing on CraftBukkit forks.

## The "Problem"

Out of the box, Spigot can modify, or more specifically nerf, certain aspects of gameplay.
While these changes may not be noticeable to the purists of survival gameplay, you may find
things are not functioning as you expect them to when you expand you gameplay horizons.
Maybe you found a cool farm you want to build but after building it you noticed something
isn't quite right and it's not matching up with the farm's showcase.

Paper, a fork of Spigot, and more widely used, makes further modifications to the game.
It does fix a lot of Spigots issues, and it is highly recommended you use it over Spigot.
The modifications by Paper are less so nerfs and more just complete disabling of certain
things the team has identified as exploitative. Some of these exploits are configurable
but by default are disabled.

## The "Solution"

Search for your problem below and make the appropriate configuration changes.

## Table of contents
- [Duping](#duping)
  - [Sand/gravity block duping isn't working](#sand-dupe)
  - [TNT duping isn't working](#tnt-duping-isnt-working)
  - [TNT in world eater isn't falling](#tnt-in-world-eater-isnt-falling)
- [Items](#items)
  - [Item sorters don't always picking up items](#hopper-cooldown)
- [Spawning](#spawning)
  - [Mob switch isn't working](#mob-switch-isnt-working)
- [Misc.](#misc)
  - [Bedrock isn't breaking](#bedrock)
  - [Ender-porter doesn't work](#ender-porter)

### Duping

<a name="sand-dupe"/>

#### Sand/gravity block duping isn't working

Gravity block dupes are patched by Paper when they become aware of them. Paper does **not** 
offer a configuration option to re-enable duping. You can see discussion behind this in
[PaperMC@Paper#3724](https://github.com/PaperMC/Paper/issues/3724).

If you want gravity block duping, look into playing either Vanilla or Fabric with optimisation
mods, however, if you REALLY must run a Bukkit variant, you can replace Paper with a fork
which undoes, or modifies, the patch. A popular Paper fork that does this is
[Purpur](https://purpurmc.org).

If using Purpur, in `purpur.yml` set the following:
```yaml
blocks:
  sand:
    fix-duping: false
```

#### TNT duping isn't working

The form of duping that is most accepted (not by all). This exploit is patched by Paper but
can be configured.

In `paper.yml` set the following:
```yaml
unsupported-settings:
  allow-piston-duplication: true
```

_Please be aware that this will also enable other forms of piston-caused duping, 
such as carpet duping and rail duping._

#### TNT in world eater isn't falling

Spigot limits the amount of TNT entities that can be processed per game tick. World eaters use
a very large amount of TNT entities to blow up the world and will very easily hit this limit
where its physics and aging won't update. This is configurable.

In `spigot.yml` change the following setting:
```yaml
max-tnt-per-tick: 1000
```

You can try and calculate the amount of TNT active at a given time during your world eating and
set a max value slightly above this, or if you don't care, you can set this absurdly high.
Unfortunately Spigot does not offer a value that outright disables this.

### Items

<a name="hopper-cooldown"/>

#### Item sorters don't always picking up items

Paper makes optimizations to the logic of hoppers, however, included is a change in how
hoppers cooldown. In vanilla, a hopper only cools-down when an item is moved to or from
its inventory. In Paper, a hopper cools-down when item movement is attempted. 

This shouldn't be an issue when using a hopper chain because the chain can only move items 
at a rate of 8 game ticks, so if a item filter hopper does go in to cooldown, it's no big 
deal because by the time the next item is ready to be tested, the 8gt cooldown will have ended.

When feeding items into an item sorting system using a water stream, items move over the
hoppers at a rate greater than 8 game ticks. If an item flows over an item filter but fails
to get picked up because it is not the right item for that filter, the filter will still go
in to cooldown. If another item comes through the water stream in less than 8 game ticks,
the filter will still be in cooldown so even if its the right item type, it'll flow right over it.

A solution to this could be to alter your water stream to build up items, maybe with a trapdoor
and hopper clock, and then release all items at the same tick to flow over the hoppers together.

If you're too lazy to make this modification to your water stream, you can configure Paper
to not cooldown the hopper all the time.

In `paper.yml` set the following:
```yaml
hopper:
  cooldown-when-full: true
```

### Spawning

#### Mob switch isn't working

By default, Paper only counts natural spawns towards the mobcap. Your mobswitch might not
be working because the mobs in it are not natural spawns.

In `paper.yml` set the following:
```yaml
count-all-mobs-for-spawning: true
```

_Please be aware that CraftBukkit messes with the persistence code of mobs and can cause mobs
to be counted who otherwise wouldn't be in Vanilla. By enabling this config option, this issue
may occur and you might experience a slow down in mob farms. But you're enabling this because
you want to disable mob spawns with your mob switch, right?_

### Misc.

<a name="bedrock"/>

#### Bedrock isn't breaking

Bedrock breaking is largely understood to be an exploit and very unintended. 
Bedrock is, of course by design, intended to be unbreakable. Paper patches bedrock breaking
but offers a config option to re-enable this exploit.

In `paper.yml` set the following:
```yaml
unsupported-settings:
  allow-permanent-block-break-exploits: true
  allow-headless-pistons: true
```

`allow-permanent-block-break-exploits` allows the bedrock block to be broken  
`allow-headless-pistons` enables the common exploit used to break bedrock

<a name="ender-porter"/>

#### Ender-porter doesn't work

Did you build an ender-porter and did you find that you didn't teleport when the enderpearl broke?  
This is considered an exploit by Paper and has been patched, but a config option is offered
to disable this patch.

In `paper.yml` set the following:
```yaml
game-mechanics:
  disable-unloaded-chunk-enderpearl-exploit: true
```
