# SKY WAR

## Summary 
- [Overview](#Overview) 
- [Drawing the Player](#Drawing-the-Player)
- [Moving the Player](#Moving-the-Player)
- [Shooting](#Shooting)
- [Refinement](#Refinement)
- [Adding Enemies](#Adding-Enemies)
- [Adding Player Health and Score](#Adding-Player-Health-and-Score)
- [Adding Sound Effects and Music](#Adding-Sound-Effects-and-Music)
- [Adding Graphics](#Adding-Graphics)
- [Conclusion](#Conclusion)
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
- and then to make the bullet move up we will substract from the `y()`:
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
- Easy we are solving this problem by adding  these tow lines in our `main.cpp` so basically it makes our Horizontal Scroll Bar and Vertical Scroll Bar disabled :
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
```cpp
//location  of  the  player
player->setPos(view->width()/2.3,view->height()/1.03- player->rect().height());
```
 - here we are :
 
 ![90](https://user-images.githubusercontent.com/86841843/151165872-ca109f7d-f38f-430b-9a32-583b1d76f23c.png)
 
- while progressing in our project we figure out that making the player move up and  down is not serving to mush so we are going to prevent the player from doing that by deleting  this tow condition from our code in `MyRect.cpp`  :
```cpp
else if (event->key() == Qt::Key_Up){
setPos(x(),y()-10);
}
else if (event->key() == Qt::Key_Down){
setPos(x(),y()+10);
}
```
- also in that same file we will try and prevent the player from going off the screen on the left and right eventually our code will be looking like this :
```cpp
  if (event->key() == Qt::Key_Left){
  if (pos().x() > 0) // prevent the player frome going off the screen on the left
  setPos(x()-10,y());  
  }
  else if (event->key() == Qt::Key_Right){
  if (pos().x() + 100 < 800) // prevent the player frome going off the screen on the right
  setPos(x()+10,y());
  }
```
- Now there is an other thing we should fix , it's that when we shoot bullet they still in the memory and that's a waste , so we are heading to `bullet.cpp` and adding this condition in the `void  Bullet::move()` :
```cpp
if (pos().y() + rect().height() < 0){
  scene()->removeItem(this);
  delete this;
  }
```
## Adding Enemies
- now we will add some enemies so our player can fight them and have funn , let's get thro the processe :
- so let's go ahead and add a source and header files `Enemy.cpp` and `Enemy.h`
- Now we are going to create the enemy class in our enemy header file then add a constructer and  a slot to it :
```cpp
class Enemy: public QObject,public QGraphicsRectItem{
  Q_OBJECT
public:
  Enemy();
public slots:
  void move();
};
```
- After that let's move to our `Enemy.cpp` file and make the enemy appear randomly then create it's shape after that we will connect it to a timer that will call the move slot of the enemy :
```cpp
Enemy::Enemy(): QObject(), QGraphicsRectItem(){
  //set random position
  int random_number = rand() % 700;
  setPos(random_number,0);
  // drew the rect
  setRect(0,0,100,100);
  // connect
  QTimer * timer = new QTimer(this);
  connect(timer,SIGNAL(timeout()),this,SLOT(move()));
  timer->start(50);
}
```
- And in order to make the enemy move down we will add to the `y()`:
```cpp
void Enemy::move(){
  // move enemy down
  setPos(x(),y()+5);
  if (pos().y() + rect().height() < 0){
  scene()->removeItem(this);
  delete this;
  }
}
```
- So now in our `main.cpp` we will include `<Qtimer>` because we want to connect a timer to a function that constantly create enemies
```cpp
// spawn enemies
  QTimer * timer = new QTimer();
  QObject::connect(timer,SIGNAL(timeout()),player,SLOT(spawn()));
  timer->start(2000);
```  
- After that we will go ahead and add the spawn slot in `MyRect` class located in `MyRect.h` and make sure that the object inherits from `<QObject>` by including it :
```cpp
#include <QGraphicsRectItem>
#include <QObject>

class MyRect:public QObject, public QGraphicsRectItem{
  Q_OBJECT
public:
  void keyPressEvent(QKeyEvent * event);
public slots:
  void spawn();
};
```
- Now let's move to the `MyRect.cpp` to create our enemies :
```cpp
void MyRect::spawn(){
  // create an enemy
  Enemy * enemy = new Enemy();
  scene()->addItem(enemy);
}
```
- After that in the bullet move slot  located in our `bullet.cpp` we will make the bullet destroy the enemy by deleting both when they collides :
```cpp
void Bullet::move(){
  // if bullet collides with enemy, destroy both
  QList<QGraphicsItem *> colliding_items = collidingItems();
  for (int i = 0, n = colliding_items.size(); i < n; ++i){
  if (typeid(*(colliding_items[i])) == typeid(Enemy)){
  // remove them both
  scene()->removeItem(colliding_items[i]);
  scene()->removeItem(this);
  // delete them both
  delete colliding_items[i];
  delete this;
  return;
  }
  }
```
- here is our result :

![lol](https://user-images.githubusercontent.com/86841843/151475044-fb0b44dd-6529-49af-b2a6-b83b84eb7d64.gif)
## Adding Player Health and Score
- so before continuing the work on our project we will make our main.cpp cleaner by taking all the code that used to be there and put it inside a new class we will call `game`  that will derives from `QGraphicsView` and has a pointer to the scene and a pointer to the player 
-  `Game.cpp`
```cpp
Game * game;
int main(int argc, char *argv[]){
  QApplication a(argc, argv);
  game = new Game();
  game->show();
  return  a.exec();
}
```
- `Game.h`
```cpp
class Game: public QGraphicsView{
public:
  Game(QWidget * parent=0);
  QGraphicsScene * scene;
  Player * player;
};
```
- now we will create a class in the `Score.h` file with a constructor called  `Score` that take parent as an optional parameter also add a member function that add to the score called `increase` and a getter to return the score `getScore`  and then add a private variable also called `score` to keep track of what the current score is (we included `<QGraphicsTextItem>`) 
```cpp
class Score: public QGraphicsTextItem{
public:
  Score(QGraphicsItem * parent=0);
  void increase();
  int getScore();
private:
  int score;
};
```
- now in the `Score.cpp` we will initialize the score to 0 and draw the text (we included `<QFont>`)
```cpp
Score::Score(QGraphicsItem *parent): QGraphicsTextItem(parent){
  // initialize the score to 0
  score = 0;
  // draw the text
  setPlainText(QString("Score: ") + QString::number(score)); // Score: 0
  setDefaultTextColor(Qt::blue);
  setFont(QFont("times",16));
}
```
- `increase()`
```cpp
void Score::increase(){
  score++;
  setPlainText(QString("Score: ") + QString::number(score)); // in order to update the text
}
```
- `getScore()`
```cpp
int Score::getScore(){
  return score;
}
```
- now for the health it the same as we did for the score but with a little change first in `Health.h` 
```cpp
class Health: public QGraphicsTextItem{
public:
  Health(QGraphicsItem * parent=0);
  void decrease();
  int getHealth();
private:
  int health;
};
```
- now in `health.cpp`
```cpp
Health::Health(QGraphicsItem *parent): QGraphicsTextItem(parent){
  // initialize the health to 0
  health = 3;
  // draw the text
  setPlainText(QString("Health: ") + QString::number(health)); // Health: 3
  setDefaultTextColor(Qt::red);
  setFont(QFont("times",16));
}
```
- `decrease()`
```cpp
void Health::decrease(){
  health--;
  setPlainText(QString("Health: ") + QString::number(health)); // Health: 2
}
```
- `getHealth()`
```cpp
int Health::getHealth(){
  return health;
}
```
- now we need to give our game class in `Game.h` file a pointer for the score and an other for health not to forget mentioning that we included `"Health.h"` and `"Score.h"`
```cpp
  Score * score;
  Health * health;
};
``` 
- after that we are heading to `Game.cpp` to make the score and health
```cpp
// create the score/health
  score = new Score();
  scene->addItem(score);
  health = new Health();
  health->setPos(health->x(),health->y()+25);
  scene->addItem(health);
```
- now in order to really make the score increase we will go inside the `Bullet.cpp` file spicily in the move method we will access the game score object and call it's `increase()` member function and before that we have added `extern  Game  *  game;` to inform that there  is  an  external  global  object  called  game
```cpp
// increase the score
  game->score->increase();
```
- to make the health really decrees we will do the same as we did for score in the `bullet.cpp` file but this time we will do it inside the `enemy.cpp` 
```cpp
//decrease the health
  game->health->decrease();
```
- here is our result 

![Swar 2022-02-06 10-59-51](https://user-images.githubusercontent.com/86841843/152676014-3cd0ff66-a1b7-4537-b61f-8d9e38f001e7.gif)
## Adding Sound Effects and Music
- to make our game more enjoyable we need to add some sound effects and music
- so first we add a resource file called `res`  then add a prefix called sounds inside of it then we can add whatever sounds and music we like to 
- so to play the back ground music we need to add `multimedia` module in the `Swar.pro`
- now in the `game.cpp` file we will include `<QMediaPlayer>` 
```cpp
// play background music
  QMediaPlayer * music = new QMediaPlayer();
  music->setMedia(QUrl("qrc:/sounds/bgsound.mp3"));
  music->play();
```
- now the next thing we want to do is to play a bullet sound every time our player shoots a bullet  
- so first in the `player.h` file we will keep another media player as a privet member for the player called `bulletsound`
```cpp
private:
  QMediaPlayer * bulletsound;
```
-  now let's initialize it in the `Player.cpp` file
```cpp
Player::Player(QGraphicsItem *parent): QGraphicsRectItem(parent){
  bulletsound = new QMediaPlayer();
  bulletsound->setMedia(QUrl("qrc:/sounds/bullet.wav"));
}
```
- after that we will move to `void Player::keyPressEvent(QKeyEvent *event)` spicily  in the `else if (event->key() == Qt::Key_Space)` condition we will make the bullet sound plays every time we press space
```cpp
//  play  bulletsound
  if  (bulletsound->state() == QMediaPlayer::PlayingState){
  bulletsound->setPosition(0);
  }
  else if (bulletsound->state() == QMediaPlayer::StoppedState){
  bulletsound->play();
  }
```
## Adding Graphics
- now we will add some images to give the game more esthetics 
- so first we will add a prefix in the res file created earlier while adding sounds let's call it images this time and the we will include all the images we are adding to our game in it. it's to similar to what we did in the last part.
- now we will replace `<QGraphicsRectItem>` to `<QGraphicsPixmapItem>` because it contain  `QPixmap` and `QImage` that are very necessary to proceed in our work
- we will go ahead and start with the bullet in `Bullet.cpp` inside `Bullet::Bullet(QGraphicsItem  *parent):  QObject(),  QGraphicsPixmapItem(parent)`
```cpp
// set bullet image
  setPixmap(QPixmap(":/images/bullet.png"));
```
- `Enemy.cpp` inside `Enemy::Enemy(QGraphicsItem  *parent):  QObject(),  QGraphicsPixmapItem(parent)`
```cpp
// set enemy image
  setPixmap(QPixmap(":/images/enemy.png"));
```
- `Player.cpp` inside `Player::Player(QGraphicsItem  *parent):  QGraphicsPixmapItem(parent)`
```cpp
  // set player image
  setPixmap(QPixmap(":/images/player.png"));
```
- `Game.cpp` inside `Game::Game(QWidget  *parent)`
```cpp
  // set the background
  setBackgroundBrush(QBrush(QImage(":/images/bg.png")));
```
- here is our final result 

![Swar 2022-02-06 12-47-50](https://user-images.githubusercontent.com/86841843/152679461-2168c030-930f-4195-8d9a-52dc3d03821f.gif)
## Conclusion
- Working on this project we have made a lot of mistakes and overcome a lot of deficits , learned new thigs and enhanced our skills . It was a long journey since we started and finally here we are , thank for sticking this long . 
