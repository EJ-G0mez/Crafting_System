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

    public Dictionary<Parent_Item, int> inventory; // This dictionary will allow the player to store any kind or item and know how many of said item are available.

    .......
  }

  ```

<summary>Functions</summary>

  ### Functions

  This section will show every function that this class has

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

  
</details>
