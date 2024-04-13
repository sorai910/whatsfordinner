# whatsfordinner

## Table of Contents

1. [Overview](#Overview)
2. [Product Spec](#Product-Spec)
3. [Wireframes](#Wireframes)
4. [Schema](#Schema)

## Overview

### Description

Whats For Dinner is a mobile application designed to help users manage their kitchen inventory more efficiently. It tracks expiry dates, suggests recipes based on what users already have, and helps manage shopping lists to optimize grocery purchases. The app aims to reduce food waste and save money by providing smart, timely suggestions for meal planning and grocery shopping. 

### App Evaluation

[Evaluation of your app across the following attributes]
- **Category:** Lifestyle / Utility
- **Mobile:** This app is designed for mobile use, utilizing device-specific features such as the reduced size of mobile device to carry over to the kitchen and real-time notifications.
- **Story:** Makes kitchen management easier by automating inventory tracking and meal preparation guidance.
- **Market:** Targeted at individuals or families looking to streamline their cooking and shopping routines and reduce food waste.
- **Habit:** Users will interact with the app daily to update inventory, check meal suggestions, and manage their grocery lists.
- **Scope:** Initially a simple app to manage kitchen inventory, with potential to scale into meal planning, online grocery integration, and smart home connectivity.

## Product Spec

### 1. User Stories (Required and Optional)

**Required Must-have Stories**

* Users can scan barcodes or manually enter items to add them to their inventory.
* Users receive recipe suggestions based on the current items in their pantry.
* Users can view and manage a shopping list based on inventory needs.
* Users receive notifications when items are nearing their expiry date.

**Optional Nice-to-have Stories**

* Users can scan barcodes to add them to their inventory.
* Recipe sharing and community features where users can share their own recipes and modifications.
* Integration with online grocery stores for direct ordering from the app.

### 2. Screen Archetypes

- [ ] [Launch Screen(optional)]
* Users can view the logo and simple instruction on how to use the app
list second screen here]
- [ ] [Inventory Screen]
* Users can add, remove, or update items in their pantry.
- [ ] [Recipes Screen]
* Users receive recipe suggestions and can select favorites.
- [ ] [Shopping List Screen]
* Users can view and modify their shopping lists.

### 3. Navigation

**Tab Navigation** (Tab to Screen)

* Inventory
* Recipes
* Shopping List

**Flow Navigation** (Screen to Screen)

- [ ] [Launch Screen(optional)]
* Navigates to Inventory Screen after the instruction.
* ...
- [ ] [Inventory Screen]
* Opens the item entry form when adding a new item.
* Goes to Recipes Screen based on selected items.
- [ ] [Recipes Screen]
* Navigates back to Inventory or forward to Shopping List.
* Opens the recipe entry form when adding a new recipes
- [ ] [Shopping List Screen]
* Users can edit items directly and check them off as purchased.


## Wireframes

[Add picture of your hand sketched wireframes in this section]
<img src="YOUR_WIREFRAME_IMAGE_URL" width=600>

### [BONUS] Digital Wireframes & Mockups

### [BONUS] Interactive Prototype

## Schema 

[This section will be completed in Unit 9]

### Models

User
| Property | Type   | Description                       |
|----------|--------|-----------------------------------|
| objectId | String | unique id for the user (default)  |
| username | String | user's username                   |
| password | String | user's password                   |
| email    | String | user's email address              |

Item
| Property   | Type   | Description                         |
|------------|--------|-------------------------------------|
| objectId   | String | unique id for the item (default)    |
| user       | String | reference to user who owns the item |
| name       | String | name of the item                    |
| expiryDate | Date   | expiry date of the item             |
| quantity   | Int    | amount of the item available        |
| category   | String | category of the item (e.g., dairy)  |

Recipe
| Property     | Type     | Description                        |
|--------------|----------|------------------------------------|
| objectId     | String   | unique id for the recipe (default) |
| title        | String   | title of the recipe                |
| ingredients  | [String] | list of ingredients required       |
| instructions | String   | cooking instructions               |
| sourceUrl    | String   | link to the recipe source          |


### Networking

- Inventory Screen
* (Read/GET) Query all items where user is owner
```
let query = PFQuery(className:"Item")
query.whereKey("user", equalTo: PFUser.current()!)
query.findObjectsInBackground {
  (items: [PFObject]?, error: Error?) in
  if let error = error {
    print("Error: \(error.localizedDescription)")
  } else if let items = items {
    // items contains an array of items owned by the current user
  }
}
```
* (Create/POST) Add a new item
```
let newItem = PFObject(className:"Item")
newItem["name"] = "Milk"
newItem["expiryDate"] = NSDate()
newItem["quantity"] = 2
newItem["user"] = PFUser.current()

newItem.saveInBackground {
  (success: Bool, error: Error?) in
  if success {
    // Item saved successfully
  } else {
    print("Error: \(error?.localizedDescription)")
  }
}
```
* (Update/PUT) Update an item's quantity or expiry date
```
let query = PFQuery(className:"Item")
query.getObjectInBackground(withId: "objectIdOfItem") {
  (item: PFObject?, error: Error?) in
  if let item = item {
    item["quantity"] = 3
    item.saveInBackground()
  } else {
    print("Error: \(error?.localizedDescription)")
  }
}
```
- Recipes Screen
* (Read/GET) Query recipes based on selected ingredients
```
let query = PFQuery(className:"Recipe")
query.whereKey("ingredients", containsAllObjectsIn: ["Milk", "Eggs"])
query.findObjectsInBackground {
  (recipes: [PFObject]?, error: Error?) in
  if let recipes = recipes {
    // recipes containing Milk and Eggs
  } else {
    print("Error: \(error?.localizedDescription)")
  }
}
 ```

- Will use a recipe api as well but still trying to search for the right one
