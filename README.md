# JusticePC
---
---
---
## Data Set Description

Our data set is split to the training set, valid set and test set, models may not use all of them. these sets are sampled from some running Game of Netease Games within three individual weeks, each set is corresponding to some week. There're different types of data in each set, like portraits data, behavior sequence data, relation graph data, and labels for role_id showed in data before. All sensitive information of users has been removed or mapped to some other values.
Our data set will be available soon, and the structure of which has been shown below:
+ Portraits_data
    * ... 
+ Behavior_sequence_data
    * ... 
+ Relation_graph_data 
    * Chat
        * ... 
    * Friendship
        * ... 
    * Team
        * ... 
    * Device-sharing
        * ... 
    * Transaction
        * ... 
+ Labels
---
---
#### Portraits Data

For each player, there will be several values to describe his/her status in the game every day which are automatically generated by the game server, which is the portraits data. We will always combine one player's portrait data in a week for convenience. If portrait data for some players on some day is missing, we will fill zeros in the corresponding place.
There're three files in this Portraits_data folder, each file consists of all the portrait data in the corresponding set, the folder structure is:
+ Portraits_data
    * train.json
    * valid.json
    * test.json

In each file, portrait data is saved in the format as below:
```python
{
    "role_id_foo":[  # this list's length is always seven
        [...],  # portrait data of role_id_foo on first day
        [...],  # portrait data of role_id_foo on second day
        [...],  # portrait data of role_id_foo on third day
        [...],  # portrait data of role_id_foo on fourth day
        [...],  # portrait data of role_id_foo on fifth day
        [...],  # portrait data of role_id_foo on sixth day
        [...],  # portrait data of role_id_foo on seventh day
    ]，
    ...  # some other role_id
}
```
---
---
#### Behavior Sequence Data

Players' behavior sequences will always be recorded by the game server, but only some events defined by the game providers will be considered as behaviors that are worth recording. We rank the events by two levels, each event will have a level-1 event id and a level-2 event id, we will also record the grade when the player take any action, especially 000 in grade field means record missing, the truth grade can be inferred by grade field in the context. We save different players' data separately, role_id_foo.json file contains the behavior sequence data of role_id_foo, the folder structure is: 
+ Behavior_sequence_data
    * train
        * role_id_foo.json
        * role_id_bar.json
        * ...
    * valid
        * role_id_foo.json
        * role_id_bar.json
        * ...
    * test
        * role_id_foo.json
        * role_id_bar.json
        * ...

In each file, behavior sequence data is saved in the format as below:
```python
[
    "2018-11-15 01:07:48#000#400063#21080002",  # "timestamp#current grade#event id level1#event id level2"
    "2018-11-15 01:07:48#000#400063#21080004",  # "timestamp#current grade#event id level1#event id level2"
    ...
]
```
---
---
#### Relation graph data 

Relation graph data consists of five parts, chat graph, friendship graph, team graph, device-sharing graph, and transaction graph. All these graphs are generated by players' interactions with each other and well-formed. We save graph data in a standard way called "edge list", we will show details of each kind of graph data below.

---
##### Chat graph data

We treat chat graphs as directed graphs. In the chat graph, each vertex is corresponding to a player, one edge from foo to bar means that foo has sent some words to bar and vice versa. The weight of a directed edge means the times of the corresponding chat. There're three files in the Chat folder, each file consists of all the chat graph data in the corresponding set. The folder structure is:

+ Relation_graph_data 
    * Chat
        * train.csv
        * valid.csv
        * test.csv

In each file, chat graph data is saved in the format as below:
```python
role_id_foo, role_id_bar, weight  # role_id_foo sent some words to role_id_bar for weight times
role_id_foo_1, role_id_bar_1, weight  # role_id_foo_1 sent some words to role_id_bar_1 for weight times
role_id_foo_2, role_id_bar_2, weight  # role_id_foo_2 sent some words to role_id_bar_2 for weight times
......
```
---
##### Friendship graph data

We treat friendship graphs as directed graphs. In the friendship graph, each vertex is corresponding to a player, one edge from foo to bar means that bar is in foo's friend list and vice versa. The weight of a directed edge means how many days this week that the friend relation has maintained. Especially, if foo and bar have a friend relation for 3 days in a week, we won't record which day on this week they are friends or which day they are not. There're three files in the Friendship folder, each file consists of all the Friendship graph data in the corresponding set. The folder structure is:

+ Relation_graph_data 
    * Friendship
        * train.csv
        * valid.csv
        * test.csv

In each file, friendship graph data is saved in the format as below:
```python
role_id_foo, role_id_bar, weight  # role_id_bar has been in role_id_foo's friend list for weight days on this week
role_id_foo_1, role_id_bar_1, weight  # role_id_bar_1 has been in role_id_foo_2's friend list for weight days on this week
role_id_foo_2, role_id_bar_2, weight  # role_id_bar_1 has been in role_id_foo_2's friend list for weight days on this week
......
```
---
##### Team graph data

We treat team graphs as undirected graphs. In the team graph, each vertex is corresponding to a player, one edge between foo and bar means that foo and bar have been in one team. The weight of an undirected edge means the times of the team playing. There're three files in the Team folder, each file consists of all the team graph data in the corresponding set. The folder structure is:

+ Relation_graph_data 
    * Team
        * train.csv
        * valid.csv
        * test.csv

In each file, team graph data is saved in the format as below:
```python
role_id_foo, role_id_bar, weight  # role_id_foo teamd up with role_id_bar for weight times
role_id_foo_1, role_id_bar_1, weight  # role_id_foo_1 teamd up with role_id_bar_1 for weight times
role_id_foo_2, role_id_bar_2, weight  # role_id_foo_2 teamd up with role_id_bar_2 for weight times
......
```
---
##### Device-sharing graph data

We treat the device-sharing graphs as undirected graphs. In the device-sharing graph, each vertex is corresponding to a player, one edge between foo and bar means that foo and bar used the same hardware device. The weight of an undirected edge means how many days in this week that the device-sharing relation has maintained. There're three files in the Device-sharing folder, each file consists of all the device-sharing graph data in the corresponding set. The folder structure is:

+ Relation_graph_data 
    * Device-sharing
        * train.csv
        * valid.csv
        * test.csv

In each file, device-sharing graph data is saved in the format as below:
```python
role_id_foo, role_id_bar, weight  # role_id_foo and role_id_bar used same hardware device for weight days on this week
role_id_foo_1, role_id_bar_1, weight  # role_id_foo_1 and role_id_bar_1 used same hardware device for weight days on this week
role_id_foo_2, role_id_bar_2, weight  # role_id_foo_2 and role_id_bar_2 used same hardware device for weight days on this week
......
```
---
##### Transaction graph data

We treat the transaction graphs as directed graphs. In the transaction graph, each vertex is corresponding to a player, one edge between foo and bar means that foo gave money to bar within a transaction. The weight of a directed edge means the total amount of money foo gave to bar. Specially foo may give money to bar for many times on a week, we will consider these transactions as one edge and the weight is the sum of the amount of money in these transactions. There're three files in the transaction folder, each file consists of all the transaction graph data in the corresponding set. The folder structure is:

+ Relation_graph_data 
    * Transaction
        * train.csv
        * valid.csv
        * test.csv

In each file, transaction graph data is saved in the format as below:
```python
role_id_foo, role_id_bar, weight  # role_id_foo gived role_id_bar weight money
role_id_foo_1, role_id_bar_1, weight  # role_id_foo_1 gived role_id_bar_1 weight money
role_id_foo_2, role_id_bar_2, weight  # role_id_foo_2 gived role_id_bar_2 weight money
......
```
---
