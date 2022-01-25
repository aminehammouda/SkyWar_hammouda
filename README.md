# SKY WAR

## Summary 
- [Overview](#Overview) 
- [Drawing the Player](#Drawing-the-Player)
- [Moving the Player](#Moving-the-Player)
- [Shooting](#Shooting)
- [Refinement](#Refinement)
- 5
- 6
- 7 
## Overview

- In this final project we chooses to work on a game that's gonna be called "SKY WAR" it is about a war plane that fight other enemy planes. We are aiming to make it an enjoyable game with an attractive look. We will be going over the various stapes of making this project from 0% to 100% . 
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
QGraphicsRectItem * player = new QGraphicsRectItem();
player->setRect(0,0,100,100); // setRect is public Functions of QGraphicsRectItem Class

// add the item to the scene
scene->addItem(player); // addItem is Public Functions of QListWidget Class

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
 
- first we will add a header file `MyRect.h` , then in that header file we will include the `QGraphicsRectItem` that we are inheriting from and then create our own class `MyRect`  :
```cpp
#include <QGraphicsRectItem>

class MyRect: public QGraphicsRectItem{
public:
  void keyPressEvent(QKeyEvent *event); // keyPressEvent Protected Functions in QGraphicsView Class
};
```
 - Now in our `main.cpp` we will make a few changes 
     - First include `"MyRect.h"` in place of `<QGraphicsScene>`.
     - then we will replace `QGraphicsRectItem * player = new QGraphicsRectItem();` with `MyRect  *  player  =  new  MyRect();`.
   - and then to make our `player` focusable other ways make it the item that receive the keyevent we will add :
```cpp
  //  make the player focusable
  player->setFlag(QGraphicsItem::ItemIsFocusable); //setFlag Public Functions in QGraphicsItem Class
  player->setFocus();
```
- Now in `MyRect.cpp` we are using `<QKeyEvent>` that describes a key event , its going to let us move our `player` using arrow keys  .
```cpp
#include "MyRect.h"
#include <QKeyEvent>

void MyRect::keyPressEvent(QKeyEvent *event){
  if (event->key() == Qt::Key_Left){
  setPos(x()-10,y());
  }
  else if (event->key() == Qt::Key_Right){
  setPos(x()+10,y());
  }
  else if (event->key() == Qt::Key_Up){
  setPos(x(),y()-10);
  }
  else if (event->key() == Qt::Key_Down){
  setPos(x(),y()+10);
  }
}
```
- here is the result :  

![result (1)](https://user-images.githubusercontent.com/86841843/150952551-94836a54-726b-465b-a9e1-4c47c23c185f.gif)


## Shooting
After finishing this part our player will be able to shoot some bullets .
- first in our `Bullet.h` file we will tray and create our one class called `bullet` and add constructer let's call it also `bullet` and a slut called `move` , not to forget to include `<QObject>` which is necessary when creating a slot .
```cpp
#include <QGraphicsRectItem>
#include <QObject>

class Bullet: public QObject,public QGraphicsRectItem{
  Q_OBJECT
public:
  Bullet();
public slots:
  void move();
};
```
* then in my `bullet.cpp` we create the shape of the bullet which is also a rectangle without forgetting to include `"Bullet.h"` and then we are going to connect that slot created previously in our `bullet.h` file . 
```cpp
Bullet::Bullet(){
  // drew the rect
  setRect(0,0,10,10);
  // connect a certain signal to a certain slot
  QTimer * timer = new QTimer();
  // this next line will connect the timeout fonction of the timer to the move slot of the bullet   
  connect(timer,SIGNAL(timeout()),this,SLOT(move()));
  timer->start(50);
}
```
- and then in that same file we will define 
```cpp
void Bullet::move(){
  // move bullet up
  setPos(x(),y()-15);
}
```
- so now in `MyRect.cpp` we are are adding a condition in that early list of conditions made earlier so that we are gonna be able to shoot using space Butten so here the bullet is created 
```cpp
else if (event->key() == Qt::Key_Space){
  // create a bullet
  Bullet * bullet = new Bullet();
  bullet->setPos(x(),y()); // this line is for seting the psition of our bullet
  scene()->addItem(bullet); // and this is to make it visible
  }
```
- working on this part we faced an issue which is that the scroll bar get bigger and bigger every time our bullet get far   

![Swar 2022-01-25 17-16-31](https://user-images.githubusercontent.com/86841843/151015893-73514ad4-68db-4999-96a3-802a7f3a93cf.gif)
- easy we are solving this problem by adding  these tow lines in our `main.cpp` so basically it makes our Horizontal Scroll Bar and Vertical Scroll Bar disabled :
```cpp
view->setHorizontalScrollBarPolicy(Qt::ScrollBarAlwaysOff);
view->setVerticalScrollBarPolicy(Qt::ScrollBarAlwaysOff);
 ```
- we get this result :

![bullets](https://user-images.githubusercontent.com/86841843/150952024-5c9225ef-24a5-46e0-ab2d-e81405e1e117.gif)
## Refinement
- in this part we will try to rectify a little in our code to give our output more esthetic :
- so first in our `main.cpp` we will set our playing window to a fixed size we are choosing  800x600 we can do that by adding to simple lines :
```cpp
  // set a fixe size
  view->setFixedSize(800,600);
  scene->setSceneRect(0,0,800,600);
```
- and then we will make our player appear in the meddle_bottom so for that we are using `setPos` which is a public functions of `QGraphicsItem` Class
