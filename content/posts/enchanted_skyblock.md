+++
title = "Enchanted Skyblock"
date = "2022-03-01T00:00:00-05:00"
author = "Reeve"
authorTwitter = "" #do not include @
cover = ""
tags = ["coding-project", "kotlin", "minecraft", "plugin", "spigot", "freelancing"]
keywords = ["minecraft", "plugin"]
description = "A little view of what happened in some of my old freelance projects."
showFullContent = false
+++

# Enchanted Skyblock

This was a sever I worked with for a long while, so I have numerous projects I worked on for them. This is just a bit of what I did.

## XwyUtilities

This was a utility plugin I made for a number of things, but allowed me to reduce my code size significantly and reduce the amount of code I rewrote between projects.
Personally, my favorite part is the GUI system, which is pretty complicated to use, but allows for a lot of customization and less worrying about how it works.

There's a lot to this, so it might get its own post in the future if I end up adding more to it, or updating it for use in newer versions.

### GUI System

{{< figure src="/images/enchanted_skyblock/inventory-diagram.png" caption="The class diagram for the inventory system." >}}

Some sample code below from one of the plugins that wasn't finished can be seen below.
It takes a little getting used to, but it's much more readable than the alternative, which requires a lot of boilerplate code, keeping track of what is which slot, etc.

{{< code language="kotlin" >}}

class GeneratorMenu : GeneratorPlayerSpecificInventoryManager() {
private val config = EnchantedMoneyGenerators.instance.config.data
private val economy = EnchantedMoneyGenerators.instance.economy

	override fun getInventory(player: Player): PlayerSpecificInventory {
		return PlayerSpecificInventory(EnchantedMoneyGenerators.instance, config.menu.slots, config.menu.title, player.uniqueId)
	}
	
	override fun setInventory(player: Player, inventory: PlayerSpecificInventory, generator: Generator) {
		with(inventory) {
			if (config.menu.fill.enabled) {
				fill(CustomItem(config.menu.fill.material) {
					setName(config.menu.fill.name)
				})
			}
			
			val collectCoins = ClickableItem(config.menu.collectCoins.getCustomItem(), true) {
			
			}
			
			addItem(collectCoins, config.menu.collectCoins.slot)
			
			if (generator.data.tier == config.tiers.size - 1) {
				// max level
				val generatorMaxLevel = ClickableItem(config.menu.maxLevel.getCustomItem(), true) {
				
				}
				
				addItem(generatorMaxLevel, config.menu.upgradeTier.slot)
			} else {
				val cost = config.tiers[generator.data.tier + 1].cost
				
				val upgradeGenerator = ClickableItem(config.menu.upgradeTier.getCustomItem().also {
					it.applyPlaceholder("%cost%", NumberFormat.getNumberInstance().format(cost))
				}, true) {
					
					if (economy!!.getBalance(player) >= cost) {
						economy.withdrawPlayer(player, cost.toDouble())
						
						player.sendMessage(config.messages.generatorUpgraded.string.color().replace("%tier%", (generator.data.tier + 1).toString()))
						config.messages.generatorUpgraded.sound.playSound(player)
						
						generator.upgrade()
					} else {
						config.menu.cantAfford.sound.playSound(player)
						setTempItem(config.menu.cantAfford.getCustomItem(), 3 * 20, config.menu.upgradeTier.slot)
					}
				}
				
				addItem(upgradeGenerator, config.menu.upgradeTier.slot)
			}
			
		}
	}
}

{{< /code >}}

### CustomItem

This was the most used feature, and went hand in hand with the GUI system. It allowed me to create items with a Kotlin style DSL, and it was easier to manage with all the little nitpick details that go into items.
One cool thing I used it for was when I had to load in all the config.
The configs always were in YAML which I hated, but I had to use it because JSON was "too complicated" for the server owner.
I tried making my own config system like GSON, but it was too complicated and I didn't have time to make it work.

Below is a simple example of how I'd typically use the CustomItem for configs.
{{< code language="kotlin" >}}
private val material = ConfigMaterial(config.getString("material"), ":", -1)
private val displayName = config.getString("display-name").color()
private val lore = config.getStringList("lore").color()
private val glow = config.getBoolean("glow")
private val amount = ItemAmount(config.getConfigurationSection("amount"))

fun getItem(): ItemStack = CustomItem(material) {
    setName(displayName)
    setLore(lore)
    if (glow) {
        meta {
            addEnchant(Enchantment.DURABILITY, 1, true)
            addItemFlags(ItemFlag.HIDE_ENCHANTS)
        }
    }
    setAmount(amount.getRandom())
}.itemStack
{{< /code >}}

## EnchantedSkills

One of the plugins I had the *joy* of working with, except it was a complete mess. Whoever made it was either learning how to use Java or had never heard of technical debt, though I have a feeling it was both.
It was essentially any usual skills plugin, but since we wanted most things custom, it *had* to be made by a developer tied to the server.

### XMaterial

One of the worst parts of it was this specific file, called `XMaterial`, which was taken from a library called [`XSeries`](https://github.com/CryptoMorin/XSeries/), except that it didn't use the primary feature of the library, cross version support.
I wouldn't have minded if it was used well, or if they had actually imported the library through Maven or something, but no, they literally just copied the XMaterial class into the project.
This file caused a lot of issues, since the owner of the server didn't know the names these used, and I tried to give him the file to look through, but it is around ~1.1k lines long and is still code, so he didn't really know where to look.

### Duplicate Code

This one wasn't nearly as bad as EnchantedPets, but it was more workable, since it was method bodies that were duplicated, not instead of pasted in the middle of a giant method with slight alterations.
The main things I ended up removing were accessors for the config, which ended up being hundreds of lines of 1 line methods, with each one having similar 100-120 characters of code.

## EnchantedPets

### Duplicate Code

This is a problem in a couple of the codebases I had to work with, but this one specifically had a horrid amount of it.
I ended up reducing a good amount through refactoring, but there was still a lot of it.
Most of my improvements ended up being in the EnchantedSkills plugin, since I had to work with that the most.


## EnchantedDrops

This one was neat, and was kinda complicated in getting things to work, since it needed the highest priority on block drops, and in order to change a drop in Minecraft (with Spigot), you need to cancel the event.
This isn't that problematic for most things, but once you realize there's enchants like fortune that change the drops, and then the block events are no longer sent to other plugins that use them, it becomes a problem.

My solution wasn't great, but since we work with mostly custom plugins it worked for us.
What I ended up doing was adding a new event called, you guessed it, `EnchantedDropEvent`, which was called before the items were dropped, in case some other plugin wanted to change or cancel them.

In the end, I had to use NMS (implementation details from Craftbukkit) to get the fortune count, because I'd rather leave it to their official math than try to copy it myself.

The original task this was for, was so that we could change the drop for something like stone, just to call the drop `Enchanted Stone` instead.
This, too, was configurable.

## EnchantedElections

One of the last ones I did, but also one of the most fun.
This one was similar to what they do in Hypixel Skyblock, where players can vote for a mayor, and the mayor will give certain bonuses.
Everything is configurable, including the mayor's perks, how their information is displayed, and how many certain ranks will get.

