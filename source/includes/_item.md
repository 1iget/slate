

# Item

Operations with items: tasks, events, projects, notes, files.



|Field        |Type         |Example         |Description|
|-------------|-------------|----------------|-----------|
|id        |long        |123000        |Globally unique item Identifier    |
|guid        |String (UDID)        |123456abcdef        |Globally unique item Identifier used for public sharing    |
|user_id        |long        |123000        |user id of owner, user who created this item    |  
|title        |String (512)        |My Task        |title    |  
|completed        |int        |0        |Indication if this item is completed    |  
|color        |int        |0        |Color tag (index of predefined colors) used for items (tasks, events, notes, ..). Onde of these colors: [no color, '#FB7E6E', '#F8B957', '#F3DF5B', '#C2D95B', '#6CB4FF', '#CAA4DF', '#B8B8B8']    | 
|color_value        |Strng        |#5e93c3        |Color value used for projects. Default: #5e93c3<br/>One of these colors: ['#5e93c3', '#fc2f6a', '#fd9426', '#fecb2e', '#55ce2e', '#cb77df', '#a18460']    | 
|category        |int        |1        |Category (or type) of item: 0:project 1: task 2:event 4: note 5:file    | 
|message        |String (5000)        |Hello        |Item description text    | 
|parent        |long        |0        |Unique id of parent item. Default: 0. 0 means item is not child of another item and not inside of project    | 
|recurring        |int        |0        |0: not recurring, 1: daily, 2: weekly, 3: monthly, 4: yearly    | 
|assigneeId        |long        |0        |Assignee, primary responsible person. User id of user whom item is assigned to    | 
|participants        |comma separated list of user identifiers        |322,413        |array of user id for this item participants    |    
|start_date        |Date        |2012-03-15T19:00:00.000+10:00        |Start date and time for this item. Default:null.    | |<code>end_date        |Date        |2012-03-15T19:00:00.000+10:00        |End date and time for this item. Default:null.    |
|is_all_day        |int        |0        |Indicator that this item is "all day" event. Only date without time should be used from start/end date field..    |
|alerts        |JSON array        |  See "Alerts" below    | |    
|shared        |int        |0        |0: Item is private. 1: item is shared, visile to team memebrs (Business account feature)    |
|time_last_update        |Date        |2012-03-15T19:00:00.000+10:00        |Timestamp when this item was last changed (updated).    |
|time_create        |Date        |2012-03-15T19:00:00.000+10:00        |Timestamp when this item was created.    |
|starred        |int        |0        |If item is marked as starred, 1 or 0    |
|time_track        |int        |0        |1/0 flag to specify if time tracking is enabled for htis item. Default:0.    |
|time_est        |float        |0        |Time estimated for this item, in hours. Used in time tracking calculations.Optional.    |
|time_spent        |float        |0        |Total time spent recorded for this item.    |
|priority        |int        |20000        |Arbitrary priority. 10000-19999:low, 20000-29999:normal, 30000:high. Default: 20000 (normal)    |
|tags        |comma separated list of tags        |"work","home"        |array or tags added to item    |
|instances        |array[item_instance]        |[]        |array of instances for recurring item. at PUT/POST requests this field is ignored by the server.    |
|location        |object        |```{"longitude":123.1234567,"latitude":-4.0987654321,"address":"qwe"}```      |Map that contains long/lat coordinates and address    |
| unread | number or null | 0 | Item is "unread" for this user. It was created or modified by someone else |
| permissions | JSON array | See "Permissions" below |

### Alerts structure

```js
    {
        "id":900, // optional: if id is not specified then new alert will be created, or existing updated otherwise
        "timeType":4, // mandatory; possible values: 1 - specified time, 2 - days before, 3 - hours before, 4 - minutes before
        "time":0, // mandatory if timeType in [2, 3, 4]
        "timeSpecified":null, // mandatory if timeType=1, format is same as for start_date and end_date of item object
        "repeat":0, // optional: how many times reminder should be repeated
        "repeatInterval":0, // optional: repeat interval in minutes
        "sound":true, // optional: play sound
        "alert":true, // optional: display notification in desktop/browser version of application
        "email":true, // optional: send email
        "push":true // optional: send push notification to mobile device
    }
```

To Create/Update/Delete reminders:

```javascript
[
    {"timeType":4,"time":0,"timeSpecified":null,"repeat":0,
    "repeatInterval":0,"sound":true,"alert":true,
    "email":true,"push":true},
    {"timeType":4,"time":5,"timeSpecified":null,"repeat":0,
    "repeatInterval":0,"sound":true,"alert":true,"email":true,
    "push":true},
    {"timeType":3,"time":1,"timeSpecified":null,"repeat":0,
    "repeatInterval":0,"sound":true,"alert":true,"email":true,
    "push":true},
    {"timeType":1,"time":0,"timeSpecified":"2016-08-31T11:00:00.000+04:00",
    "repeat":0,"repeatInterval":0,"sound":true,"alert":true,
    "email":true,"push":true}
]
```

* Create: simply add new alert to array and put data to server
* Update: simply update existing alert in array (id must be specified in alert object) and put data to server
* Delete: simply remove alert from array and put data to server

Alert object description:




### Permissions

Permission is defined by a level integer value.

| Level | Description | Title displayed in client | Sub-title (description) displayed in client app |
|--------|------|-------|------|
| 0/none | Not visible to user. Server does not return this item in user's items | _Not visible_ | |
| 10 | User can see item in list. Item sub-items and file attachments. Item history. | View | |
| 20 | User can add comments to item | *View and Comment* | See in their lists, View details, Add comments |
| 30 | User can add attachments. Can add sub-items, If project- can create new items in project | *Add sub-tasks and attachments* , "Create tasks in project" | |
| 40 | User can assign/reassign | "Assign" | |
| 50 | User can Complete, Log time. Transition to another status (future) | *Complete and Assign* | Complete, reassign to another Team member, Attach documents, Add sub-tasks, Log time. |
| 60 | User can modify all properties (except Sharing permissions) | *Modify* | Modify item except its Sharing permissions |
| 70 | Can change sharing permissions (can add level 70 but not remove) | "Share with others" | |
| 80 | User can archive item |  |
| 100 | User can delete item | *Everything* | Modify, Change sharing, Delete, Archive |


### Unread value

| Unread | What it means | Relationship to item |
|--------- | ------- | -----------|
| 0 or null | Item is read (default) | |
| 1 | Comment added | User is participant or assignee or creator. Comment added by someone else |
| 2 | Modified, sub-item added | Assignee, participant, creator |
| 3 | New item | Assignee, participant, creator |



## 1. Get array of items


* `GET    /item` 

This method will return array of all items: tasks, events, files including projects

```javascript
//TODO: example
```

### 1.2 Get delta update of items

* `GET  	/item/since/{timestamp}`

Returns list of items that were changed or added since specified date


## 2. Create Item

* `POST    /item` will create new item


## 3. Update Item

* `PUT    /item` will update (modify) item

### Response codes for Create and Update

HTTP code | API Error code | Description
------------ | ------------- | ------------
200 |  | Created/updated
400 | 73 | Unable to add {principal X} because project/parent item is not shared with {principal X}.
400 | 74 | {principal X} permission can not be {modified/added/removed} because it will conflict with parent {permission X} permissions.
400 | 75 | {user X} can not be {assignee/participant} because they don't have access to project {project name}.
400 | 76 | Owner {owner name} is not allowed to change own permission.
400 | 77 | Permission {principal X} downgrade to {permission X} is not allowed because parent has {permission Y} for this principal.
400 | 78 | Cannot remove {assignee/participant} {user name} permission.


## 4. Delete

* `DELETE    /item/{id}` will delete item

Parameters:
* {id}: unique id of item.

User performing delete request must have sufficient rights to delete this object

Possible HTTP return responses:

* 200 operation successful
* 404 item not found. It may be already deleted or not visible to current user.
* 403 not authorised

## 5. History

### 5.1 Retrieve item history including comments:

* `GET    /item/history?id={item id}` 

### 5.2 Add comment

* `POST    /item/comment` will add a new comment to the item

Parameters: id: item id, message: comment body text

### 5.3 Delete comment

* `DELETE    /item/comment` will delete comment



## 6. Archive 

### 6.1 Archive item

Moves item to Archive

* `POST    /item/archive` will archive item

Parameters:
* id or guid: id or guid of item to archive.

User performing request must have sufficient rights to archive specified item.

Possible HTTP return responses:

* 200 operation successful
* 403 not authorised
* 404 item not found; it may be already archived or not visible to current user

### 6.2. Restore item from archive

* `GET    /archive/restore` will restore item from archive

Parameters:
* id or guid: id or guid of item to restore.

User performing request must have sufficient rights to restore specified item.

Possible HTTP return responses:

* 200 operation successful
* 403 not authorised
* 404 item not found
* 507 limit of items exceeded for current account

### 6.3. Restore copy of item from archive

* `GET    /archive/restorecopy` will restore copy of item from archive

Parameters:
* id or guid: id or guid of item which copy to restore.

User performing request must have sufficient rights to restore specified item.

Possible HTTP return responses:

* 200 operation successful
* 403 not authorised
* 404 item not found
* 507 limit of items exceeded for current account

## 7. Multiple items operations

### 7.1 Delete multiple items

* `POST    /item/deletemultiple` will delete items

Parameters:
* id: comma separated list of item identifiers (example: id=1,2,3).

User performing request must have sufficient rights to delete specified items.
No error will be rised and item delete request simply ignored if user does not have rights to delete specific item.

Possible HTTP return responses:

* 200 operation successful
* 400 invalid format of list of identifiers

### 7.2 Archive multiple items

* `POST    /item/archivemultiple` will archive multiple items

Parameters:
* id: comma separated list of item identifiers (example: id=1,2,3).

User performing request must have sufficient rights to archive specified items.
No error will be rised and item archive request simply ignored if user does not have rights to archive specific item.

Possible HTTP return responses:

* 200 operation successful
* 400 invalid format of list of identifiers