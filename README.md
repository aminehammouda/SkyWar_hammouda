# SKY WAR

## Summary 
- [Overview](#Overview) 
- [Drawing the Player](#Drawing-the-Player)
- [Moving the Player](#Moving-the-Player)
-  [Shooting](#Shooting)
- 4
- 5
- 6
- 7 
## Overview

- In this final project we chooses to work on a game that's gonna be called "SKY WAR" it is about a war plane that fight other enemy planes. We are aiming to make it an enjoyable game with an attractive look. We will be going aver the various stapes of making this project from 0% to 100% . 
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
## Moving the Player
 Now we are going to make that player made earlier move:
 
- first we will add a header file `MyRect.h` , then in that         header file we will include the `QGraphicsRectItem` that we are inheriting from and then create our own class `MyRect`  :
```cpp
#include  <QGraphicsRectItem>

class  MyRect:  public  QGraphicsRectItem{
public:
  void  keyPressEvent(QKeyEvent  *  event);
};
```
 - Now in our `main.cpp` we will make a few changes 
     - First include `"MyRect.h"` in place of `<QGraphicsScene>`.
     - then we will replace `QGraphicsRectItem * rect = new QGraphicsRectItem();` with `MyRect  *  rect  =  new  MyRect();`.
   - and then to make our `rect` focusable other ways make it the item that receive the keyevent we will add :
```cpp
  //  make  rect  focusable
  rect->setFlag(QGraphicsItem::ItemIsFocusable);
  rect->setFocus();
```
- Now in `MyRect.cpp` we are using `<QKeyEvent>` that describes a key event.
```cpp
#include  "MyRect.h"
#include  <QKeyEvent>

void  MyRect::keyPressEvent(QKeyEvent  *event){
  if  (event->key()  ==  Qt::Key_Left){
  setPos(x()-10,y());
  }
  else  if  (event->key()  ==  Qt::Key_Right){
  setPos(x()+10,y());
  }
  else  if  (event->key()  ==  Qt::Key_Up){
  setPos(x(),y()-10);
  }
  else  if  (event->key()  ==  Qt::Key_Down){
  setPos(x(),y()+10);
  }
}
```
- here is the result :  

![result](https://user-images.githubusercontent.com/86841843/150929320-75a0b3f1-89eb-40aa-89d6-874f35389ef6.gif)

## Shooting
After finishing this part our player will be able to shoot some bullets .
- first we will tray and create our one class called `bullet`
