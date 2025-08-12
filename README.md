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
#import Item_Parent
#import BP_FirstPersonCharacter

public class extends BlueprintActor implements Interact_Interface {
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


