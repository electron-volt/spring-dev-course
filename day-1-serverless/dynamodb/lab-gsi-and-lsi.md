# LAB: GSI and LSI

## Adding items via the CLI&#x20;

For this lab to be a little more interesting, you may want to add a few more items to the Dogs table so that there is more than just 1 dog of a certain breed. You can do this via the console like we did before (if you like clicking) or you might want to do it via the CLI.&#x20;

Below is a command that allows you to add several items at once to a table called Dogs. If using a shared account, just change the name of your table on line 3. In this example the dogs all have an ID and a breed. You could also add other attributes to some or all items.&#x20;

```
aws dynamodb batch-write-item \
--request-items '{
    "Dogs": [
        {
            "PutRequest": {
                "Item": {
                    "ID": {"N": "123"},
                    "Breed": {"S": "husky"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "ID": {"N": "234"},
                    "Breed": {"S": "belgian shepherd"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "ID": {"N": "345"},
                    "Breed": {"S": "husky"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "ID": {"N": "456"},
                    "Breed": {"S": "husky"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "ID": {"N": "567"},
                    "Breed": {"S": "husky"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "ID": {"N": "678"},
                    "Breed": {"S": "labrador"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "ID": {"N": "980"},
                    "Breed": {"S": "labrador"}
                }
            }
        }
    ]
}' \
$LOCAL
```

This is my example table that has two rottweilers and three huskies:

![example Dogs table](<../../.gitbook/assets/image (229).png>)

## Part 1: GSI&#x20;

Let's create a global secondary index for our Dogs table.

* [ ] Navigate to DynamoDB and the Dogs table
* [ ] Go to Indexes and press Create index
* [ ] Use breed as partition key and ID as sort key&#x20;
* [ ] The system will suggest breed-ID-index as the name, that's fine.&#x20;
* [ ] Leave all other settings as they are and hit "create index".&#x20;

The index creation takes a while. Just wait until the Status turns to Active.

### Query for a certain breed

Once the index has been successfully created, let's go back to View items. We want to run a query,  not on the Dogs table but the new breed-ID-index. I will query my table for all dogs whose breed is Husky.&#x20;

![](<../../.gitbook/assets/image (157).png>)

Running the same query for rottweiler:

![it works as advertised](<../../.gitbook/assets/image (352).png>)

## Part 2: LSI

We just created a GSI by pressing a button that said "create index". It didn't ask us to specify if we want a local or global index. Has AWS learned to read minds? Possibly. But also for a table that already exists you can **only** create a GSI. The only time when you are able to create an LSI is when the table itself is being created - so we must create a new table.&#x20;

### Create a new table

* [ ] Go to DynamoDB and Create a new table&#x20;
* [ ] Name of the table is YOURNAME-**Music**&#x20;
* [ ] Partition key is **artist** (String)
* [ ] _Sort_ key is **song** (String)
* [ ] Under settings select **Customize settings**
* [ ] Leave table class and all read/write settings as they are
* [ ] Under secondary indexes, click "**Create local index**"
  * [ ] Sort key is **album** (String)
  * [ ] Index name is **album-index**
  * [ ] Attribute projections: all
  * [ ] **Create index**
* [ ] **Create table**

### **Populate the table**

Once the table has been created, add some items there.

#### Via the console

1. Navigate to your table&#x20;
2. Click **Create item**&#x20;
3. On the right hand upper corner you can toggle between Form and JSON. Pick whichever you like.&#x20;
4. Add an item with three attributes:
   1. Artist
   2. Song
   3. Album

In the console there is no option to add several items at once.&#x20;

#### Via the CLI&#x20;

You can populate the table using the command below via the CLI or CloudShell. If the name of your table is not Music, then change the name on line 3.

```
aws dynamodb batch-write-item \
--request-items '{
    "Music": [
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Beethoven"},
                    "song": {"S": "Canon in D major"},
                    "album": {"S": "Greatest classical compositions"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Nightwish"},
                    "song": {"S": "Dark Chest of Wonders"},
                    "album": {"S": "Once"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Nightwish"},
                    "song": {"S": "Ghost Love Score"},
                    "album": {"S": "Once"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Nightwish"},
                    "song": {"S": "Nemo"},
                    "album": {"S": "Once"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Justin Bieber"},
                    "song": {"S": "Baby"},
                    "album": {"S": "CIA black site torture collection"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Led Zeppelin"},
                    "song": {"S": "Whole Lotta Love"},
                    "album": {"S": "II"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Led Zeppelin"},
                    "song": {"S": "What Is and What Should Never Be"},
                    "album": {"S": "II"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Led Zeppelin"},
                    "song": {"S": "When the Levee Breaks"},
                    "album": {"S": "II"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Led Zeppelin"},
                    "song": {"S": "Bring It On Home"},
                    "album": {"S": "II"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Led Zeppelin"},
                    "song": {"S": "Four Sticks"},
                    "album": {"S": "IV"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Led Zeppelin"},
                    "song": {"S": "Misty Mountain Hop"},
                    "album": {"S": "IV"}
                }
            }
        },
        {
            "PutRequest": {
                "Item": {
                    "artist": {"S": "Jimi Hendrix"},
                    "song": {"S": "All along the watchtower"},
                    "album": {"S": "some album"}
                }
            }
        }
    ]
}' \
$LOCAL
```

### Query the index

Here is an example of what the Music table might look like:

![example Music table](<../../.gitbook/assets/image (400).png>)

Let's try to find all the songs from Nightwish's album Once:

![All songs on the album Once](<../../.gitbook/assets/image (401).png>)

There you have it. In the actual base table Music we have a composite key of artist and song. So Artist-song is a key that uniquely defines one item. On the LSI however the composite key is Artist-album.&#x20;
