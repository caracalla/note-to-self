## General Knowledge
* Primitives (Hash, Array etc.) are evil! Build objects.
* Inheritance is evil! Use Composition.
* Conditions (if..else, switch..case) are evil! Use Polymorphism.

## Stuff to Build

What problems do I wish someone else would solve for me?

### ARCHITECTURE GAME
 - 2D flat color style
 - build multi-story houses out of walls, windows, doors
 - place furniture and decorations and define usage
 - NPCs mingling around, using spaces appropriately
 - Have different types of windows, doors, etc
 - Show 3D render of space?



## Reading List
* SICP
* Algorithm Design Manual
* Concrete Mathematics
* C Programming Language
* Design Patterns
* Programming Pearls



## lessons learned from shuddle/HSD:
* break problem into small changes, make small change, run specs/test, repeat
* rails makes it easy to make monstrous queries with associations
    * example:
        ```
        trip, which has one
          planned route, which has many
            route legs, each of which has two
              waypoints, each of which has many
                participations, each of which has one
                  passenger
        ```
* monitor everything
* what's the point of microservices if the client has to talk to the monolith to get to it
    * many subdomains?
* make sure to not send a bajillion requests if you don't need to
* make it easy to read and process logs
* write to master, read from slaves
* impersonating users is useful
* forced update is useful
* make the clients do as much as possible - but not too much
* microservices are good unless they interact too much - like they share the same db
