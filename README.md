# SKY WAR

## Summary 
- [Overview](#Overview) 
- [Drawing the Player](#Drawing-the-Player)
- [Moving With the Arrow Keys](#Moving-With-the-Arrow-Keys)
- 3
- 4
- 5
- 6
- 7 
## Overview

- In this final project we chooses to work on a game that's gonna be called "SKY WAR" it is about a war plane that fight other enemy planes. We are aiming to make it an enjoyable game with an attractive look. 
- we are expecting it to look like this:

![Sky War](https://user-images.githubusercontent.com/86841843/150900170-e6521443-5cdd-42b4-87e8-25c187451d3e.png)
## Drawing the Player
- So first we need to create our player which gonna be a rectangle its a 3 steps work first we will create a scene using `QGraphicsScene` then implement `QGraphicsRectItem` to create an item that will be put into the scene and finally add a view to visualize the scene and for that we will use `QGraphicsView`.
* `main.cpp`
```cpp
#include <QApplication>
#include <QGraphicsScene>
#include <QGraphicsRectItem>
#include <QGraphicsView>

int main(int argc, char *argv[])
{
QApplication a(argc, argv);

// create a scene
QGraphicsScene * scene = new QGraphicsScene();

// create an item to put into the scene
QGraphicsRectItem * rect = new QGraphicsRectItem();
rect->setRect(0,0,100,100);
// add the item to the scene
scene->addItem(rect);

// add a view to visualize the scene
QGraphicsView * view = new QGraphicsView(scene);
view->show();

return a.exec();
}
```
- this is our result :

![1](https://user-images.githubusercontent.com/86841843/150908656-ad35393d-9bd5-4022-9968-46a77c34b2c0.png)
## Moving With the Arrow Keys
