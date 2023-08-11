# DatapackTutorial
An example datapack

Knowing how to create a datapack is very useful. Knowing how to do so can let you fix recipes, or inconsistent behaviour in your world.
This tutorial was originally created with Alloy Forgery in mind. If you already know how to create a basic datapack you can read up on [how to make Alloy Forgery recipes.](https://github.com/LordDeatHunter/Alloy-Forgery/wiki/Adding-recipes-and-fuels)  

# Creating a Datapack
Creating a basic datapack is fairly simple. To start out you need a folder or a zip file that contains the datapack.  
The main components for a datapack:
* A `pack.mcmeta` file, which tells Minecraft what versions your datapack supports.
* A data folder, which contains all the data of your pack.  

These are required for any data pack. If you look at any example you will find these folders. 
This tutorial will teach you how to format these.  

At any point where JSON is used, using a JSON checker like [JSONLint](https://jsonlint.com/) is useful for catching bad formatting, especially comma errors.  


## The `pack.mcmeta` file
A `.mcmeta` file is just a JSON file with a custom extension. The file usually looks something like this:
```JSON
{
  "pack": {
    "pack_format": 15,
    "description": "Put your description here"
  }
} 
```  
The `pack_format` file determines what versions of Minecraft your datapack supports. 
You can still load outdated packs, but the game warns you about loading them.  
 

To create this file you can simply save/edit a text file with the .mcmeta extension.  

Currently the pack format versions are the following:
- `7` for versions 1.17.x
- `8` for versions 1.18.x
- `9` for versions 1.19 – 1.19.2
- `11` for version 22w42a – 22w44a
- `12` for version 1.19.3
- `13` for version 1.19.4
- `14` for version 23w14a – 23w16a
- `15` for version 1.20.x
- `16` for version 23w31a
- `17` for version 23w32a

For future versions of Minecraft this can change, and the best page to find the latest number is on the [Minecraft Wiki.](https://minecraft.fandom.com/wiki/Tutorials/Creating_a_resource_pack#Formatting_pack.mcmeta)  

*If you are struggling with creating the file, you can simply copy a mcmeta from ANY datapack. It's not stealing.* 


## The `data` folder
The data folder frankly contains all your data files. When putting data inside you separate it using folders. The first folder you create is a `namespace`.  

A `namespace` is a core/root folder, which prevents conflicts with data from other places.  
The standard structure for your pack would be `data/namespace`, and in this folder you put your data.  

Having limitless possibility here allows modders to create new folders to store their own data in, although Minecraft itself only loads specific things from specific folders.
An example is the fact that recipes will only be loaded under a `recipes` folder. When putting files inside these main folders you can create subfolders to sort it as you please, and this will be reflected in-game.

Example: a recipe under `data/my_cool_datapack/recipes/awesome_stuff/test.json` will show up under the `/recipe` command as `my_cool_datapack:awesome_stuff/test`

Here are some of the folder names you can use to modifiy different things in the game: 
* `advancements`
* `damage_type`
* `dimension_type`
* `functions`
* `loot_tables`
* `predicates`
* `recipes` <--- Covered in this tutorial
* `structures`
* `tags` <--- Covered in this tutorial
* `worldgen`

**NOTE!** Every file and folder inside the data folder **needs to be lowercase**. If not, you might see errors in your logs or the pack won't load at all.   


## Packing the datapack  
Once you are done with creating your datapack it is likely you want to share it around. The simplest way to do this is to create a zip file out of it.  
For this tutorial I will be using 7zip, and I will create a zip file of the folder.  
To do this I open up 7zip, find the datapack I have created, and create a new zip file from it.  
![An image showing the 7zip GUI, and using the 7zip -> add to Testpack.zip](https://github.com/Noaaan/DatapackTutorial/blob/main/images/using_7zip.png?raw=true)

**NOTE!** By zipping in this particular way you will have to extract the folder out of zip file in order to use the datapack.  
If you instead create a zip file which *only* contains the data itself (`data` folder and `.mcmeta` file), you can import/install it directly.

You can send this zip file to your friends, who can install it by extracting the folder into their datapacks folder for the world they want to install it in.  

**TODO -** update this example to show a better way of packing this. 

# Example Datapack
Now to try to create a datapack!  
To get started we will do the following:
* Create a new single player world (with cheats enabled)
* Exit back to the menu
* Navigate to the world we created
* Click "Edit World"
* Finally, click "Open World Folder".  

Then we will head into the `datapacks` folder, where we can create our datapack. This is also the location you would install datapacks into your world. 

First we will create the pack folder, which can be named anything you please. For this example we will call it "TestPack".  
Inside we create a `pack.mcmeta` file, and an empty `data` folder. Since I am playing on 1.17.1 I set the `pack_format` to 7 and give it a description.  
 
Now we open the world again, and as long as the datapack was properly formatted it will be loaded automatically.  
We can verify this by running the command `/datapack list`. If done correctly it should load!  

![Running "/datapack list" was successful, and shows the datapack in the chat.](https://github.com/Noaaan/DatapackTutorial/blob/main/images/testpack.png)  

Now we can add something to the pack which could be useful. I want to make a recipe later, so adding a tag which we can use in our recipes would be useful. I want to be able to smelt certain stone blocks into deepslate, so lets do that!  

**NOTE! -** At this point you do not need to go back to the main menu to update the datapack. Just run the `/reload` command to reload the datapack. If the recipe fails to load, it will write an error to the logs, so keep an eye on them.   
**TODO -** Example on how to open the log viewer in the vanilla launcher, as thats much better than going into the `.minecraft/logs/latest.log` file and looking at it constantly.

## Creating a tag
We start by creating a new folder inside `data`, which determines the namespace for the pack. I will choose `based_tutorial` for this tutorial.  
Then we create a `tags` folder. Minecraft separates block and items, and since our recipes uses items, we will need to create an `items` folder.  

**NOTE!** Blocks usually have an associated block and block item. Because of this mixing blocks and items in an item tag is fine, but doing so in a block tag will not work.

Inside it we create a new tag file called "rocks_and_stones.json". Tags has to be JSON files (Most files in datapacks are .json files).  
The structure of the file looks like this:
```JSON
{
  "replace": false,
  "values": [
    "minecraft:andesite",
    "minecraft:diorite",
    "minecraft:granite"
  ]
}
```
The `replace` boolean here decides if the tag should be overwritten. If you create a tag with the *same* namespace and path of another tag (I.E: `data/minecraft/items/gold_ores.json`) you can add to it. If you want to overwrite it, you can do so by setting `replace` to true.   
By default this should be false. The reason being that other mods/datapacks may want to be able to add new values to this tag.  

The `values` field is an array (list), and contains all the blocks or items you want in the tag.  

We can now test to see if this tag exists in the game. We will use `/reload` to reload the datapack, and then we will use the `/clear` command, which supports removing tags from your inventory!  

If done correctly we can now remove andesite, diorite, and granite, with the tag, from the inventory!

![Running "/clear @s #based_tutorial:rocks_and_stones 10" removed exactly 10 granite, diorite, and andesite from the inventory. It was successful!](https://github.com/Noaaan/DatapackTutorial/blob/main/images/tag_test.png?raw=true)

## Creating a recipe  

We will now use the tag we made to make a recipe!  
To start out we need to create a new folder under our namespace called `recipes`.  

For our pack we will simply create a new JSON file inside the recipe folder called `deepslate_from_stones.json`.  
Inside it we write our new recipe. A regular smelting recipe looks something like this:
```JSON
{
  "type": "minecraft:smelting",
  "ingredient": {
    "tag": "based_tutorial:rocks_and_stones"
  },
  "result": "minecraft:deepslate",
  "experience": 0.2,
  "cookingtime": 200
}
```
All of these objects shape a smelting recipe. Here is what each object means:
* `type` decides what is using your recipe, like smelting or shapeless crafting. You can find a list of vanilla recipe types [here.](https://minecraft.fandom.com/wiki/Recipe#List_of_recipe_types)
* `ingredient` is an object that contains either an `item` or a `tag`, which is used in the recipe. The object inside contains the identifier of either the tag or the item, depending on what you picked.
* `result` is the item output of the recipe. Note for vanilla this only accepts single items.
* `experience` is the amount of experience dropped when completing a smelting recipe.
* `cookingtime` is how long it takes for the item to smelt.  

The `type` is used in all recipes, but the other objects can vary depending on what you are making.   
For more in-depth info [check out the Minecraft Wiki.](https://minecraft.fandom.com/wiki/Recipe)  

After running `/reload` again, and using `/recipe give @s *`, which gives us all the recipes, we can now see that the recipe appears in-game!  

![An animated image that shows the recipe in a furnace using the recipe book.](https://github.com/Noaaan/DatapackTutorial/blob/main/images/smelting_recipe.gif?raw=true)

Feel free to use sub-folders here to sort your recipes into categories, making it easier to use later. As an example; here is what the `recipe` folder from my mod Mythic Metals looks like inside: 
**NOTE!** The identifier of a recipe will include folders in the name. For Mythic Metals a recipe for an *Adamantite Sword* has the identifier `mythicmetals:swords/adamantite_sword`. Note the slash.

![An image showing multiple folders with different names, all containing recipes](https://github.com/Noaaan/DatapackTutorial/blob/main/images/recipe_folder_content_example.png?raw=true)   

## Further reading

If you feel like you want to learn more about creating datapacks, then I recommend looking up guides online for further reading.
The Minecraft Wiki has a ton of technical details on how to format different kinds of data. 

The https://misode.github.io/ site is also a fantastic resource for creating datapacks, with a helpful web-based GUI, allowing you to dig deeper and more easily create, import, and export datapacks. 
