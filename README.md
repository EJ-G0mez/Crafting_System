# Crafting_System

## This Project is a basic Crafting System built into Unreal Engine 5, meant to simulate the crafting system of an action adventure video game.

This project is meant to be a *learning and practical* execution of a crafting system for myself and a guide to others to implement something similar into their games/projects. The following project has these features:

  1. A basic inventory with its own UI to view items
  2. The ability to pickup items from the ground
  3. A crafting system that allows items to be created as long as the necessary components are acquired
  4. Basic items with their own categories that are separated in the inventory system UI

This README document is meant to record all of the features this code has, giving a thrurough explanation with the use of Diagrams, Images, and PseudoCode meant to resemble the Unreal Engine 5 block structure.

# Content

<details>

  <summary>Syntaxis, Diagrams and Media</summary>

  # Syntaxis, Diagram and Media

  ## How is this document organized and how should you read it?
  
  This section is to give an understanding of how the document will be formated and organized as well as explaining the diagrams that will be placed around this document.

  ### Syntax

  All of the added or edited functions, structures or events will be added to this document. Any default classes, structures, etc, that are in the base Unreal Engine default projects, will only be added if they are specifically mentioned in the context of the coding.

  The coding will be done in a pseudocode that will ressemble as much as possible a object oriented language

  Example:

  ```
    class newClass {

      public  bool attribute;
      public  int attribute2;
      private static  struct attribute3;

      public function1( int parameter, bool parameter){
        return returnType;
      }

      private functionEvent(){
        attribute += 1;
      }
    }
  ```
  
  ### Diagrams 
  
  There will be general UML Diagrams summarizing and showing the logic of every component, as Unreal has very specific color coding for their nodes (i.e, events, functions, and the specific color of the data structures) these diagrams will use the colors used in Unreal for the classes and data structures.
  
  Example:

  ![imagen](https://github.com/user-attachments/assets/c4eb07d7-86af-498c-9c1a-d473067e88b4) [^1]

  [^1]: Example of a diagram that will be used in this document

### Media

Media in this document will have foot notes describing what the media is showing. Media can include images, videos of the project to assist in the understanding of this project.

Example:

  ![dotboxspawnemitter](https://github.com/user-attachments/assets/a0b89f3c-2c57-42c8-b502-7a0211c958d8)[^2]

  [^2]: Example of an image showing an Unreal Project taken from the [Unreal Documentation Webpage](https://dev.epicgames.com/documentation/en-us/unreal-engine/nodes-in-unreal-engine). 

  <img width="4409" height="3366" alt="Clase UML" src="https://github.com/user-attachments/assets/35333426-fe2c-4501-9380-3439eba92ec8" /> [^3]

  [^3]: Full class diagram of the project.
  
</details>

<details>

  <summary>Project Settings</summary>

  # Project Settings

  For this class to function properly, it is Required to add some additional inputsinto your project settings to toggle the inventory to appear, and to pick up items. For this project "Tab" is used to open the invenotry, and "E" is used to pick up items on the ground.

  <img width="1007" height="552" alt="imagen" src="https://github.com/user-attachments/assets/629c5160-30b5-464d-af4d-ec71b6f22e48" /> [^4]

  [^4]: Project Settings showing the addtional inputs in Unreal Engine 5


</details>
  
<details>

  <summary>Inventory_Component</summary>
  
  # Inventory_Component

  ## How we contain items that are grabbed by the player.

  This is a blueprint actor component class that allows the player to add items, remove them, and search for items in a specific category.

  <img width="394" height="299" alt="imagen" src="https://github.com/user-attachments/assets/6d5a58ce-f46b-4e77-9fc5-aedf68d848bd" /> [^5]

  [^5]: Inventory_Component class diagram

  ```
  #import Parent_Item

  public Blueprint class extends ActorComponent Inventory_Component {

    public Dictionary<Item_Parent, int> inventory; // This dictionary will allow the player to store any kind or item and know how many of said item are available.

    .......
  }

  ```

<summary>Functions</summary>

  ### Functions

  This section will show every function that this class has.

  <ins>AddToInventory</ins>

  A simple functions that add an item to the "inventory" Dictonary,  it mkes the "Parent Item" the key, and adds an int value to the respective key.

  <img width="1308" height="549" alt="imagen" src="https://github.com/user-attachments/assets/d1b9a494-00be-4093-806f-519cfcf24f9d" /> [^6]

  [^6]: AddToInventory function in Unreal Engine 5.

  ```
  public boolean AddToInventory(Item_Parent itemAdded, int quantity){
    quantity += inventory.find(itemAdded); // This will check if the item already exists in the dictionary, if not, this operation will add 0.
    inventory.add(itemAdded, // This will add the key to the Dictionary
                  clamp(/*value*/ quantity, /*min*/ 0, /*max*/ 99)); // this clamp function makes sure that the maximum amount of the quantity is limited to 99 and minimum is 0
    return true;
  }
  
  ```

  <ins>RemoveFromInventory</ins>

  A simple function that removes a single item from the "inventory" Dictionary. It searches the Item_parent Map Key and reduces the count .1.

  <img width="1192" height="374" alt="imagen" src="https://github.com/user-attachments/assets/7190fbdb-75ca-43c0-9f80-e466242e102c" /> [^7]

  [^7]: RemoveFromInventory function in Unreal Engine 5

  ```
  public boolean RemoveFromInventory(Item_Parent itemToRemove, int quantity){
    if(QueryInventory(self, itemToRemove, quantity).success){ //search if the item is in the inventory
      inventory.add(itemToRemove, (QueryInventory(self, itemToRemove, quantity).outputQuantity - quantity)); //update the Map by removing the value of item quantity
      return true;
    }
    return false;
  }
  ```

<ins>QueryInventory</ins>

A simple function that searches a specific item in the "inventory" Map/Dictionary. It searches for the spefied item and amount in the Map and removes it from the Dictionary.

<img width="1197" height="373" alt="imagen" src="https://github.com/user-attachments/assets/7b2e71c9-907c-4f9b-a933-a9e04bdf05d3" /> [^8]

[^8]: QueryInventory function in Unreal Engine 5

```
public boolean, int QueryInventory(Item_Parent item, int quantity){
  boolean success = true; // we set a boolean to see if the item was found
  int outputquantity; // we set a number to see how many of said number of items was found
  if(inventory.find(item).value >= quantity AND inventory.find(item).success){ //if we found the item and it has a higher amount then the requested quantiry
    return success, outputQuantkty = inventory.find(item).value; //return that the item an amoun do exist
  }
  return !success, outputQuantkty = inventory.find(item).value; //else return false and a value of -1
}
```
</details>

<details>

<summary>Interact_Interface</summary>

# Interact_Interface

## An interface to interact with the world

This is an interface with the purpose to have to functions that allows items to be interactables. A function that knows when an item is looked and a function that allows the item to be interacted with.

<summary>Interface</summary>

### Interface

<img width="330" height="149" alt="imagen" src="https://github.com/user-attachments/assets/73d26258-cbfd-4335-a68f-d5b91babb51f" /> [^9]

[^9]: Interact_Interface class diagram

<img width="1131" height="582" alt="imagen" src="https://github.com/user-attachments/assets/cba4de42-f8e4-4530-a3f4-713dec8d483a" /> [^10]

[^10]: Interact_Interface in Unreal Engine 5

```

public interface Interact_interface{

  public lookAt(); //This will known when the object is being looked at

  public InteractWith(); //This will allows the item to be interactablke

}
```

</details>

<details>

<summary>Item_Categories</summary>

# Item_Categories

## Classifying item types

This is an Enumerator that gives the option to give a specific category for an item.

<summary>Enumerator</summary>

### Enumerator

<img width="289" height="134" alt="imagen" src="https://github.com/user-attachments/assets/9aa3e0a4-02f0-467d-a4f9-01698aba7cdd" /> [^11]

[^11]: Item_Categories Enumeraot class diagram

<img width="1912" height="407" alt="imagen" src="https://github.com/user-attachments/assets/a3841e6b-a90c-4592-86c4-bce19183eae3" /> [^12]

[^12]: Item_Categories Enumerator in Unreal Engine 5

```
public enumerator Item_Categories {
  Parts,
  Plants,
  Skins,
  Medicinal,
  Ammon,
  Throwable,
  Equipment
}
```

</details>

<details>

<summary>Item_Parent</summary>

# Item_Parent

## The class for all items

This actor blueprint class is a parent function to all items, it will have all basic parameters all items need, and Events that items will have.

<img width="765" height="477" alt="imagen" src="https://github.com/user-attachments/assets/eae71c52-cd99-4153-b7e3-a78caa64b8aa" /> [^13]

[^13]: Item_Parent class diagram

<img width="1916" height="992" alt="imagen" src="https://github.com/user-attachments/assets/82de365e-3d2c-45d2-aa4a-e93988fde70b" /> [^14]

[^14]: Item_Parent Viewport in unreal_Engine 5

```
#import Item_Categories
#import BP_FirstPersonCharacter

public BluePrintActor class  Item_Parent extends BlueprintActor implements Interact_Interface {
  public text name;
  public text description;
  public Item_Categories category;
  public Texture2D thumbnail;
  public Dictionary<Item_Parent, int> craftingRecipe;

  .........
}
```

<summary>Event Graph</summary>

### Event Graph

<ins>InteractWith</ins>

This is an event that tranforms the "InteractWithFunction" from the Interact Interface to an Event. It will allow the player to interact with an item, to add them to their inventory, and removes them from the level.

<img width="1682" height="295" alt="imagen" src="https://github.com/user-attachments/assets/93ec4a31-9378-4f82-bd8f-66ddb085d862" /> [^15]

[^15]: InteractWith Event in Unreal Engine 5

```
public eventr InteractWith(BP_FirstPersonCharacter playerCharacter){
  playerCharacter.Inventory_Component.AddToinventory(self.GetClass(), 1); // we add the item to the inventory
  try {
    My_Player_Controller cast = (My_Player_Controller) GetPlayerController(0); //then we attempt to cast the player controller into  aMy_Player_Controller class
    cast.Headup_Display.Crafting_Menu_W.RefreshInventory(); //then we update the UI to show this item in the inventory
    DestroyActor(self); // We destroy the item itself and remove from the level
  }
}
```
</details>

<details>

<summary>Item_Green_Leaf</summary>

# Item_Green_Leaf

## A basic crafting item

This is an Actor the inherits all featuers from the Item_Parent class. It is used for testing purposes.

<img width="268" height="251" alt="imagen" src="https://github.com/user-attachments/assets/b41dd6c8-ba30-48f3-b908-096eff9510a9" /> [^16]

[^16]: Item_Green_Leaf class diagram

<img width="1914" height="1008" alt="imagen" src="https://github.com/user-attachments/assets/989f0385-8bf9-4a54-9f7c-9e05f4813f35" /> [^17]

[^17]: Item_Green_Leaf Actor values in Unreal Engine 5

```
#import Item_Categories
#import BP_FirstPersonCharacter

public BluePrintActor class  Item_Green_Leaf extends Item_Parent  {
  super.name = "Green Leaf";
  super.description = "These Green Herbs are filled with healthy protein. They are used by the locals for medicine.";
  super.category = Item_Categories.Plants;
  super.thumbnail = "T_Bush_D";
  super.craftable = false;
  super.craftingRecipe = null;
  .........
}
```

</details>

<details>

<summary>Item_Green_Leaf</summary>

# Item_Healing_Flask

## A basic crafted item

This is an Actor the inherits all featuers from the Item_Parent class. It is used for testing purposes.

<img width="270" height="268" alt="imagen" src="https://github.com/user-attachments/assets/4f74fdcd-223a-4c89-90c6-3e55ce658fdb" /> [^18]

[^18]: Item_Healing_Flask class diagram

<img width="1915" height="1034" alt="imagen" src="https://github.com/user-attachments/assets/25f8844d-1aed-49b7-a2a6-db78ba0f0065" /> [^19]

[^19]: Item_Healing Flask Actor values in Unreal Engine 5

```
#import Item_Categories
#import BP_FirstPersonCharacter

public BluePrintActor class  Item_Healing_Flask extends Item_Parent  {
  super.name = "Healing Flask";
  super.description = "Can heal the drinker by a considerable amount, wanring: side-effects included";
  super.category = Item_Categories.Medicinal;
  super.thumbnail = "T_Tech_Dot_M";
  super.craftable = true;
  super.craftingRecipe = {Item_Green_Leaf: 3}
  .........
}
```

</details>

<details>

<summary>Crafting_Menu_W</summary>

# Crafting_Menu_W

## How we see our crafting menu.

This is a Widget Blueprint Object that gives our UI to the player. This UI has all the item categories, and will show all available items, and available craftable items.

<img width="769" height="649" alt="imagen" src="https://github.com/user-attachments/assets/7196f0b4-87e5-4dff-96b7-13ac73b28527" /> [^20]

[^20]: Crafting_Menu_W class diagram

<img width="1422" height="1003" alt="imagen" src="https://github.com/user-attachments/assets/bf807729-21cf-443a-a531-c23a69288f88" /> [^21]

[^21]: Crafting_Menu_W Desginer view in Unreal Engine 5

```
#import Inventory_Category_W
#import BP_FirstPersonCharacter
#import Inventory_Slot_W
#import Item_Parent

public BlueprintWidget class Crafting_Menu_W extends UserWidget {
  public Inventory_Category_W Ammo;
  public Inventory_Category_W Equipment;
  public Inventory_Category_W Medicinal;
  public Inventory_Category_W Parts;
  public Inventory_Category_W Skins;
  public Inventory_Category_W Throwables;
  public Inventory_Slot_W hoveredItem;
  public BP_FirstPersonCharacter playerCharacter;


...................
}
```

<summary>Event Graph</summary>

### Event Graph

<ins>Construct</ins>

This is an event that triggers when the item is created. It sets the playerCharacter variable value.

<img width="853" height="316" alt="imagen" src="https://github.com/user-attachments/assets/14eb641f-bfc1-4504-9ec7-de08f074bf0b" /> [^22]

[^22]: Contruct event in Unreal Engine 5

```
public event Contrcut() {
  playerCharacter = (BP_FirstPersonCharacter) GetPlayerCharaccter();
}
```

<ins>RefreshInventory</ins>

This event is called when there is a change in the inveotry of the player. It will refresh the UI to show changes.

<img width="940" height="465" alt="imagen" src="https://github.com/user-attachments/assets/fd3016b6-82e9-41a4-a0d1-484ff86c0fde" /> [^23]

[^23]: RefreshInventory event in Unreal Engine 5

```
public event RefreshInventory(){
  Inventory_Category_W array[] = [Ammo, Equipment, Medicinal, Parts, Plants, Skins, Throwables]; // an array that makes sure that all categories are refreshed
  for each x in array {
    OnRefresh(x); // Refresh the inventory
  }
}
```

<ins>Craft_Item</ins>

This Event triggers when an item is crafted. It gets all used items, the crafted item, the crafting recipe and adds and removes the items correspondingly.

<img width="1565" height="275" alt="imagen" src="https://github.com/user-attachments/assets/b3c349c4-b182-40b9-83dd-5a8bb5ff6908" /> [^24]

[^24]: Craft_Item Event in unreal Engine 5.

```
public event Craft_Item(){
  if(hoveredItem.isValid){ //check if there is a hovered item
    if(hoveredItem.item.craftable){ //check if the item craftable
      if(checkCraftRecipe(hoveredItem.item.craftingRecipe).validRecipe){ //check if the recipe is available to be crafted
        AddToInventory(playerCharacter.Inventory_Component, hoeveredItem.item); //add to inventory crafted item
        removeCraftedIngredient(hoveredItem.item.craftingRecipe); //remove crafting materials
        RefreshInventory();
      }
    }
  }
}
```
<summary>Functions</summary>

### Functions

<ins>checkCraftRecipe</ins>

A function that checks if the player has all the item for the selected recipe. It searches the recipe and checks if the player has all required items.

<img width="1417" height="449" alt="imagen" src="https://github.com/user-attachments/assets/c9cc006b-7804-4210-94c9-4c4e40bca1c8" /> [^25]

[^25]: checkCraftRecipe function in Unreal Engine 5

```
public boolean checkCraftRecipe(Dictionary<Item_Parent, int> recipe){
boolean isComplete = true;
  for each item in recipe.keys() { //check all the item map keys
    if(!QueryInventory(playerCharacter.Inventory_Component, item, recipe.find(item)).success){  //check if the item amount required is not found
      isComplete = false;
    }
  }
return isComplete;
}
```

<ins>removeCraftedIngredients</ins>

A function that removes all used items in a crafting recipe. It finds the in the inventory and removes them from itself.

<img width="1231" height="363" alt="imagen" src="https://github.com/user-attachments/assets/485f7620-7c7d-4036-95ba-be363bee471d" /> [^26]

[^26]: removeCraftedIngredients function in Unreal Engine 5

```
public removeCraftedIngredient(Dictionary<Item_Parent, int> ingredients){
  for each items in ingredients.keys(){
    RemoveFromInventory(playerCharacter.Inventory_Component, item, ingredients.find(item));
  }
}
```

</details>

<details>

<summary>Inventory_Category_W</summary>

# Inventory_Category_W

## Lets divide our inventory in the UI

This is a Widget Blueprint class that divides the inventory into categories. THey make sure the inventory is organized.

<img width="812" height="659" alt="imagen" src="https://github.com/user-attachments/assets/9435a51e-4236-4e47-9858-9d824aadfe23" /> [^27]

[^27]: Inventory_Category_W class diagram

<img width="1426" height="1008" alt="imagen" src="https://github.com/user-attachments/assets/80225dc7-ac8c-490d-bf50-adbb5fdbd925" /> [^28]

[^28]: Inventory_Category_W Designer view in Unreal Engine 5

```
#import Item_Categories
#import BP_FirstPersonCharacter
#import Item_Parent

public BlueprintWidget class Inventory_Category extends UserWidget {
  public WrapBox BOX_Category_Container;
  public TextBox TXT_Category_Title;
  public BP_FirstPersonCharacter playerCharacter;
  public boolean isCraftedCatgory;
  public Item_Parent[] craftedItems;

..........
}
```

<summary>Event Graph</summary>

### Event Graph

<ins>PreConstruct</ins>

The event that is active before the actor is constructed. It helps to add the slots to a specific category and adds any item that can be crafted.

<img width="1810" height="436" alt="imagen" src="https://github.com/user-attachments/assets/7b4d35db-b674-4033-b9e2-1f97ab7f2a84" /> [^29]

[^29]: PreConstruct Event in Unreal Engine 5

```
public event PreConstruct(){
  TXT_Category_Title.setText((Text) category.toString()); //set the category title bby casting the enum to a text
  playerCharacter = (BP_FirstPersonCharacter) GetPlayerCharacter(0);
  if(isCraftedCategory){
    for each item in craftedItems {
      AddChildToWrapBox(CreateWidget("Inventory_Slot_W", item, playerCharacter)); //Add the slots in the catgory if they are a craftable
    }
  } else {
    for each item in playerCharacter.Inventory_Component.inventory {
      if(item.category == category){
        AddChildToWrapBox(CreateWidget("Inventory_Slot_W", item, playerCharacter)); // Adds the players current inventory slots to the category
      }
    }
  }
}
```

<ins>OnRefresh</ins>

This event is active when there is a change on the inventory, it refreshes the slots of the categories by clearing and adding the WarpBox of the category

<img width="1566" height="785" alt="imagen" src="https://github.com/user-attachments/assets/992818a3-b26f-4ac7-8b24-21acb100b768" /> [^30]

[^30]: OnRefresh event in Unreal Engine 5

```
public event OnRefresh(){
  BOX_Category_Container.ClearChildren();
  if(isCraftedCategory){
    for each item in craftedItems {
      AddChildToWrapBox(CreateWidget("Inventory_Slot_W", item, playerCharacter)); //Add the slots in the catgory if they are a craftable
    }
  } else {
    for each item in playerCharacter.Inventory_Component.inventory {
      if(item.category == category){
        AddChildToWrapBox(CreateWidget("Inventory_Slot_W", item, playerCharacter)); // Adds the players current inventory slots to the category
      }
    }
  }
}
```

</details>

<details>

<summary>Inventory_Slot_W</summary>

# Inventory_Slot_W

## Viewing a singular item

This is a Widget Blueprint class that show an individual slot of an item. 

<img width="802" height="622" alt="imagen" src="https://github.com/user-attachments/assets/4fe55b6e-ebd6-4afd-9d77-6be833a35cf8" /> [^31]

[^31]: Inventory_Slot_W class diagram.

<img width="1422" height="1005" alt="imagen" src="https://github.com/user-attachments/assets/c5e9842d-686e-4cf4-990d-36da71171f08" /> [^32]

[^32]: Inventory_Slot_W Designer View in Unreal Engine 5

```
#import Item_Parent
#import BP_FirstPersonCharacter
#import Crafting_Menu_W

public WidgetBluerpint class Inventory_Slot_W extends UserWidget {
  public Image IMG_Item_Thumbnail
  public TextBox TXT_Quantity_Text;
  public Item_Parent item;
  public BP_FirstPersonCharacter playerCharacter;
  public Crafting_Menu_W crafting_Menu;

........... 
}
```

<summary>Event Graph</summary>

### Event Graph

<ins>PreConstruct</ins>

An event that occurs prior to when the object is created, it will set the slot to show the item thimbnail and show the exact amount.

<img width="1872" height="423" alt="imagen" src="https://github.com/user-attachments/assets/a43fbade-3590-4f7a-89c8-b7f4632dd85b" /> [^33]

[^33]: PreConstruct in unreal Engine 5

```
  public event PreConstruct(){
    IMG_Item_Thumbnail.SetBrushFromTexture(item.thumbnail); //set the thumbnail of the slot
    TXT_Quantity_Text-setText(playerCharacter.Invenotry_Component.inventory.find(item).toText()); //set the amount of the item from the amount in the players inventory
    if(playerCharacter.Invenotry_Component.inventory.find(item) > 0){ //If there are more than one item
      crafting_Menu = ((My_Player_Character) GetPlayerController(0)).Headup_Display.Crafting_Menu_W; //set the crafting menu to that of the players
    } else { //else reduce the opacity of the text and then set the crafting menu
      TXT_Quantity_Item.SetOpacity(0.4);
      crafting_Menu = ((My_Player_Character) GetPlayerController(0)).Headup_Display.Crafting_Menu_W; //set the crafting menu to that of the players
    }
  }
```

<ins>Tick</ins>

This Event is triggered on every tick, it is a check to see if an item is being hovered above with the mouse.

<img width="1507" height="606" alt="imagen" src="https://github.com/user-attachments/assets/7c40f931-218c-48c5-9869-942efb0ddf86" /> [^34]

[^34]: Tick event in Unreal Engine 5

```
public event Tick(){
  if(isHovered()){
    crafting_Menu.hoveredItem = self;
  } else {
    if(crafting_Menu.hoveredItem == self){
      crafting_Menu.hoveredItem = null;
    }
  }
}
```

</details>

<details>

<summary>BP_FirstPersonCharacter </summary>

# BP_FirstPersonCharacter

## The default first person character blueprint

This is the default First PErson Character Blueprint in the First Person Project in Unreal Engine 5. It was modified to have an inventory component and to check and interact with items.

<img width="528" height="287" alt="imagen" src="https://github.com/user-attachments/assets/7555ffee-06ce-4106-ac6a-f80d8622510c" /> [^35]

[^35]: BP_FirstPersonCharacter class diagram

```
#import Inventory_Component

public Blueprint class BP_FirstPersonCharacter extends Character {
  public Inventory_Component Inventory_Component
  public Actor lookAtTarget;

.......
}

```

<summary>Event Graph</summary>

### Event Graph

<ins>Tick</ins>

This event is triggered with every tick, it does a check to see if an item is being looked at.

<img width="521" height="204" alt="imagen" src="https://github.com/user-attachments/assets/7cd6a157-fa3e-4df7-a38e-d9242251be9f" /> [^36]

[^36]: Tick event in Unreal engine 5

```
public event Tick(){
  CheckLookAt(self);
}
```

<ins>InputAction_Interact</ins>

This event checks if the "E" key is pressed.

<img width="597" height="392" alt="imagen" src="https://github.com/user-attachments/assets/1bd14978-7aae-44b5-b035-85f8d34cd975" /> [^37]

[^37]: InputAction_Interact event in Unreal Engine 5.

```
public event InputAction_Interact(){
  lookAtTarget.InteractWith(self);
}
```

<summary>Functions</summary>

### Functions

<ins>CheckLookAt</ins>

This function calls the look at of the actor to see if the plkayer is looking at an item, it calculates the camera's position and sets the value of the looked at actor.

<img width="1670" height="432" alt="imagen" src="https://github.com/user-attachments/assets/52f661ae-f178-4d81-b20c-7cd6f8482bbd" /> [^38]

[^38]: CheckLookAt function in Unreal Engine 5

```
public CheckLookAt(){
  vector3 start = this.FirtPersonCamera.GetWorldLocation();
  vector3 end = GetForwardVector(this.FirtPersonCamera.GetWorldRotation()) * 300.0 + start;
  if(LineTraceByChannel(start, end, "interactive", /*Ingenor self*/ true).OutHitBlockingHit){ // Check if there is an item
    lookAtTarget = LineTraceByChannel(start, end, "interactive", /*Ingenor self*/ true).OutHitActor; // set the item hit
    lookAtTarget.LookAt(self);
  } else [
    lookAtTarget = null;
  ]
}
```
</details>

<details>

<summary>Headup_Display</summary>

# Headup_Display

## The main UI for the HUD

This widget Blueprint class is meant to keep all other widgets contained within. I has the crafting menu as a main widget.

<img width="519" height="273" alt="imagen" src="https://github.com/user-attachments/assets/09cd2551-eaa7-4b01-987f-974d22951700" /> [^39]

[^38]: Headup_Display class diagram

<img width="1425" height="1004" alt="imagen" src="https://github.com/user-attachments/assets/7182be33-2f14-4d55-a618-2fc8d54cc71b" /> [^39]

[^39]: Headup_Display desginer view in UNreal Engine 5

```
#import Crafting_Menu_W

public BlueprinWidget class Headup_Display extends UserWidget {
  public Crafting_Menu_W Crafting_Menu_W;
  public Canvas HUD_Canvas;

....................
}
```

<summary>Event Graph</summary>

### Event Graph

<ins>ToggleCraftingMenu</ins>

This event is triggered when the Crafting menu is opened. It checks if its already open and makes it visible or not depending on the visibiolity.

<img width="1683" height="675" alt="imagen" src="https://github.com/user-attachments/assets/2803f13c-c075-42b2-8df5-daf98bb148cf" /> [^40]

[^40]: ToggleCraftingMenu in Unreal Engine 5

```
public event ToggleCraftingMenu(){
  if(Crafting_Menu_W.isVisible()){
    Crafting_Menu_W.SetVisibility("hidden");
    SetInputModeGameOnly(GetPlayerController(0), false);
    GetPLayerController(0).showMouseCursor = false;
    GetPlayerCharacter(0).EnableInput(GetPlayerController(0));
  } else {
    Crafting_Menu_W.SetVisibility("visible");
    SetInputModeGameAndUI(GetPlayerController(0), Crafting_Menu_W, "Do Not Look", true, false);
    GetPlayerController(0).showMouseCursor = true;
    GetPlayerCharacter(0).DisableInput(GetPlayerController(0));
  }
}
```

</details>

<details>

<summary>My_Player_Controller</summary>

# My_Player_Controller

## A player controller for the character

This is PlayeController class that is meant to be added for the player to have access to certain controls. Like opening the inventory and interacting with items.

<img width="470" height="308" alt="imagen" src="https://github.com/user-attachments/assets/933245d4-b5d2-45f7-b712-c32836bc9067" /> [^41]

[41]: My_Player_Controller class function

```
#import Haedup_Display

public PlayerController class My_Player_Controller extends PlayerController {
  public Haedup_Display haedupDisplay;


.............
}
```

<summary>Event Graph</summary>

### Event Graph

<ins>BeginPlay</ins>

This event occurs when the project is started, it generates the HUD and adds it to the viewport.

<img width="1082" height="245" alt="imagen" src="https://github.com/user-attachments/assets/49196b23-7088-4063-ae24-5a0ef0c2c335" /> [^42]

[^42]: BeginPlay Event in Unreal Engine 5

```
public event BeginPlay(){
  headupDisplay = CreateWidget("Headup_Display");
  AddToViewport(headupDisplay);
}
```

<ins>ToggleCrafting</ins>

This event is triggered when pressing "tab". It opens the crafting menu.

<img width="592" height="354" alt="imagen" src="https://github.com/user-attachments/assets/e725cfc7-dd06-4567-882f-0654ee008a7b" /> [^43]

[^43]: ToggleCrafting event in Unreal Engine 5

```
  public event ToggleCrafting(){
    headupDisplay.ToggleCraftingMenu();
  }
```

<ins>Interact</ins>

This event is trigerred when pressing "e" in the cratfing menu. It crafts an item.

<img width="777" height="387" alt="imagen" src="https://github.com/user-attachments/assets/b7e0270b-9d6d-4701-99e3-8a8e0a0ebfed" /> [^44]

[^44]: Interact event in Unreal Engine 5

```
publice event Interact(){
  headupDiplay.Crafting_Menu_W.CraftItem():
}
```

</details>
