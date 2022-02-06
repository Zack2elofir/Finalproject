
      
 
#  Clash of Tanks

**Our goal is to create  a MainWindow based application using the designer**

##  **Credit**

**we would like to warmly thank our teacher for his supervision, his patience as well as for the time he has dedicated to teach us the skills needed for carrying out this work.**

## Introduction

The  **QGraphicsView class**  provides a widget for displaying the contents of a QGraphicsScene. QGraphicsView visualizes the contents of a QGraphicsScene in a scrollable viewport. To create a scene with geometrical items, see QGraphicsScene's documentation. QGraphicsView is part of the Graphics View Framework.

The  **QGraphicsScene class**  provides a surface for managing a large number of 2D graphical items. The class serves as a container for QGraphicsItems. It is used together with QGraphicsView for visualizing graphical items, such as lines, rectangles, text, or even custom items, on a 2D surface. QGraphicsScene is part of the Graphics View Framework. It also provides functionality that lets you efficiently determine both the location of items, and for determining what items are visible within an arbitrary area on the scene. With the QGraphicsView widget, you can either visualize the whole scene, or zoom in and view only parts of the scene.

The  **QGraphicsItem class**  is the base class for all graphical items in a QGraphicsScene. It provides a light-weight foundation for writing your own custom items. This includes defining the item's geometry, collision detection, its painting implementation and item interaction through its event handlers. QGraphicsItem is part of the Graphics View Framework.

![enter image description here](https://user-images.githubusercontent.com/93097467/152694825-6c2dd4a1-57e7-4d3a-8054-4d3499053778.png)
## Overview :

This project is a very important experience for us EIDIA students to become familiar with these new materials that we can consider very important.

During this period, the project that we are going to carry out is an game named  **Clash of Tanks**. To clarify things for you, and to bring you closer to the options offered by our project, we have written this report which contains all the details and explanations associated with our project.

To achieve **Clash of Tanks**, we used Qt-Creator which is a cross-platform integrated development environment (IDE) built for the maximum developer experience. Qt Creator runs on Windows, Linux, and macOS desktop operating systems, and allows developers to create applications across desktop, mobile, and embedded platforms.


**Here is a overview of how it is designed:**

 This is what the main window looks like.

![enter image description here](https://user-images.githubusercontent.com/93097467/152694852-c0ffe235-96b2-46cd-9ebb-64074c877027.jpg)
 
 and then When we click on play , this interface is shown:

![enter image description here](https://user-images.githubusercontent.com/93097467/152694863-143950cc-63d3-475f-9662-33ad2e390f19.jpg)

## Steps :

In carrying out this project, we had several Steps, thanks to which we were able to frame our work, and do our research in a well-organized way.

The steps that have been set to carry out this work are the following:

First We create our window , it will be a GraphicsScene with its own properties , and we generate the first class which we called MainWindow: Firstly ,We create our first window (game.h) :
```C
                     
    

     class  Game  :  public  QWidge {
    
    public:
    
      explicit  Game(QWidget  *parent  =  nullptr);
        
    QTextLayout  *  textLayout;
    
    protected:
    
      void  keyPressEvent(QKeyEvent  *e);
    
    signals:
    
      void  openmenu();
    
      void  GameOver(int  ID);
    
    private  slots:
    
      void  SlotEndOfTheGame(int  ID);
    
    };
     
    
    
```
then we implement this methods in the game.cpp:
We create the score widget showing the damage and add it to the main scene making the connection.
```C


    Game::Game(QWidget  *parent)  :  QWidget(parent)
    
    {
    
      QVBoxLayout*  game_layout  =  new  QVBoxLayout(this);
    
      game_layout->setContentsMargins(0,  0,  0,  0);
    
      game_layout->setSpacing(0);
    
      
    
      //WIDGET  WITH  SCORE
    
      Score*  score  =  new  Score();
    
      game_layout->addWidget(score);
    
      
    
      
    
      //widget  with  scene
    
      MainScene*  main_scene  =  new  MainScene();
    
      game_layout->addWidget(main_scene);
    
      connect  (main_scene,&MainScene::signalConnect,score,&Score::slotDamage);
    
      connect  (score,&Score::signalEndOfTheGame,this,&Game::SlotEndOfTheGame);
    
      this->setLayout(game_layout);
    
    }
    
      
    
    void  Game::keyPressEvent(QKeyEvent  *e)
    
    {
    
      if(  e->key()  ==  Qt::Key_Escape)
    
      {
    
      emit  openmenu();
    
      }
    
    }
    
    void  Game::SlotEndOfTheGame(int  ID){
    
      emit  GameOver(ID);
    
    }
```
Then we display our Menu which contains three Push Button , each one have its connections, we also add an image for our background.
First of all we implement the line for the menu header file:
```c


    class  GameMenu  :  public  QWidget
    
    {
    
      Q_OBJECT
    
    public:
    
      explicit  GameMenu(QWidget  *parent  =  nullptr);
    
      
    
    signals:
    
      void  play();
    
      void  newgame();
    
      void  settings();
    
      void  exit();
    
    };
```
Then we implement our methods in the gamemenu.cpp file:
```C


    GameMenu::GameMenu(QWidget  *parent)  :  QWidget(parent)
    
    {
    
      QVBoxLayout*  menu_layout  =  new  QVBoxLayout();
    
      QPushButton*  play  =  new  QPushButton("Play");
    
      QPushButton*  newgame  =  new  QPushButton("New Game");
    
      QPushButton*  settings  =  new  QPushButton("Settings");
    
      QPushButton*  exit  =  new  QPushButton("Exit");
    
      //play->setEnabled(false);
    
      //settings->setEnabled(false);
    
      
    
      int  WINDOW_WIDTH  =  QGuiApplication::screens().at(0)->availableGeometry().width();
    
      int  WINDOW_HEIGHT  =  QGuiApplication::screens().at(0)->availableGeometry().height();
    
      
    
      this->setAutoFillBackground(true);
    
      QPalette  palette  =  this->palette();
    
      
    
      //QString  file_path  =  PRO_FILE_PWD;
    
      palette.setBrush(QPalette::Window,  QBrush
    
      (QPixmap("C:/Users/TUF/Desktop/Tank war/Tanks-main/Tanks/images/gamemenu.jpg").scaled(QSize(WINDOW_WIDTH,  WINDOW_HEIGHT),  Qt::IgnoreAspectRatio)));
    
      this->setPalette(palette);
    
      
    
      menu_layout->setContentsMargins(WINDOW_WIDTH/3,  WINDOW_HEIGHT/5,  WINDOW_WIDTH/3,  WINDOW_HEIGHT/5);
    
      play->setSizePolicy(QSizePolicy::Expanding,  QSizePolicy::Expanding);
    
      newgame->setSizePolicy(QSizePolicy::Expanding,  QSizePolicy::Expanding);
    
      settings->setSizePolicy(QSizePolicy::Expanding,  QSizePolicy::Expanding);
    
      exit->setSizePolicy(QSizePolicy::Expanding,  QSizePolicy::Expanding);
    
      
    
      
    
      menu_layout->addWidget(play);
    
      menu_layout->addWidget(newgame);
    
      menu_layout->addWidget(settings);
    
      menu_layout->addWidget(exit);
    
      
    
      this->setLayout(menu_layout);
    
      
    
      connect(play,  &QPushButton::clicked,  this,  &GameMenu::play);
    
      connect(newgame,  &QPushButton::clicked,  this,  &GameMenu::newgame);
    
      connect(settings,  &QPushButton::clicked,  this,  &GameMenu::settings);
    
      connect(exit,  &QPushButton::clicked,  this,  &GameMenu::exit);
    
    }
```
**Here is the first interface shown :**

**The first button  _Start_  move us to the game.**


**We used a function called _setbrush_ to draw our Tanks by parts**
```C

    QRectF  Tank::boundingRect()  const
    
    {
    
      return  QRectF(-20,  -25,  40,  50);  //  Limiting  the  area  in  which  the  tank  lies
    
    }
    
      
    
    QPainterPath  Tank::shape()  const
    
    {
    
      QPainterPath  path;
    
      path.addEllipse(boundingRect());
    
      return  path;
    
    }
    
      
    
    void  Tank::paint(QPainter  *painter,  const  QStyleOptionGraphicsItem  *option,  QWidget  *widget)
    
    {
    
      if  (tank_ID  ==  1)  {
    
      painter->setBrush(Qt::blue);
    
      }  else  if  (tank_ID  ==  2)  {
    
      painter->setBrush(Qt::green);
    
      }
    
      
    
      painter->drawRect(-15,-25,30,40);//body
    
      painter->setBrush(Qt::black);
    
      painter->drawRect(-20,-20,5,30);//left  wheel
    
      painter->drawRect(15,-20,5,30);//right  wheel
    
      painter->drawRect(-5,-5,10,30);//gun
    
      target->setPos(mapToParent(0,  2000));
    
      
    
      
    
      Q_UNUSED(option);
    
      Q_UNUSED(widget);
    
    }
```
We created this class which is responsible for the first tank's movement and its functionality:
```C

    void  Tank::slotGameTimer1()
    
    {
    
      if(GetAsyncKeyState(VK_LEFT)){
    
      angle  -=  10;  //  Set  the  rotation  10  degrees  to  the  left
    
      setRotation(angle);  //  Rotating  the  object
    
      target->angle  -=  10;
    
      target->setRotation(target->angle);
    
      if(!(this->scene()->collidingItems(this).isEmpty())){
    
      angle  +=  10;  //  Set  the  rotation  10  degrees  to  the  right
    
      setRotation(angle);
    
      //  Rotating  the  object
    
      }
    
      }
    
      if(GetAsyncKeyState(VK_RIGHT)){
    
      angle  +=  10;  //  Set  the  rotation  10  degrees  to  the  right
    
      setRotation(angle);  //  Rotating  the  object
    
      target->angle  +=  10;  //  Set  the  rotation  10  degrees  to  the  left
    
      target->setRotation(target->angle);
    
      /*  Checking  for  a  collision
    
      *  if  a  collision  has  occurred,
    
      *  then  we  return  the  hero  back  to  the  starting  point
    
      *  */
    
      if(!scene()->collidingItems(this).isEmpty()){
    
      angle  -=  10;//  Set  the  rotation  10  degrees  to  the  right
    
      setRotation(angle);  //  Rotating  the  object
    
      }
    
      }
    
      if(GetAsyncKeyState(VK_UP)){
    
      setPos(mapToParent(0,  5));
    
      //  target->setPos(target->mapToParent(0,  5));
    
      /*Move  the  object  5  pixels  forward
    
      *  by  translating  them  into  the  coordinate  system
    
      *  graphic  scene
    
      *  */
    
      if(!scene()->collidingItems(this).isEmpty()){
    
      setPos(mapToParent(0,  -5));
    
      }
    
      }
    
      if(GetAsyncKeyState(VK_DOWN)){
    
      setPos(mapToParent(0,  -5));  /*  Move  the  object  5  pixels  back
    
      *  by  translating  them  into  the  coordinate  system
    
      *  graphic  scene
    
      *  */
    
      //  target->setPos(target->mapToParent(0,  -5));
    
      if(!scene()->collidingItems(this).isEmpty()){
    
      setPos(mapToParent(0,  5));
    
      }
    
      }
    
      
    
      if(GetAsyncKeyState('K')){
    
      emit  signalBullet(/*QPointF(this->x(),this->y())*/mapToScene(0,25),QPointF(target->x(),target->y()));
    
      
    
      }
    
      //  borders  check
    
      if(this->x()  <  -(width  /  2)){
    
      this->setX(-(width  /  2)  +  1);  //left
    
      }
    
      if(this->x()  >  (width  /  2)){
    
      this->setX((width/2)  -  1);  //right
    
      }
    
      
    
      if(this->y()<  -(height  /  2)){
    
      this->setY(-(height  /  2)  +  1);  //up
    
      }
    
      if(this->y()>  (height  /  2)){
    
      this->setY((height  /  2)  -  1);  //down
    
      }
    
    }
```
This enables us to control the movement of the first tank ,keeping it within the game frame and emitting the bullets.

The same thing goes as well for the second tank  and here is the methods implemented for its control:
```C

    void  Tank::slotGameTimer2()
    
    {
    
      if(GetAsyncKeyState('A')){
    
      angle  -=  10;
    
      setRotation(angle);
    
      
    
      if(!(this->scene()->collidingItems(this).isEmpty())){
    
      angle  +=  10;
    
      setRotation(angle);
    
      }
    
      }
    
      if(GetAsyncKeyState('D')){
    
      angle  +=  10;
    
      setRotation(angle);
    
      
    
      
    
      if(!scene()->collidingItems(this).isEmpty()){
    
      angle  -=  10;
    
      setRotation(angle);
    
      }
    
      }
    
      if(GetAsyncKeyState('W')){
    
      setPos(mapToParent(0,  5));
    
      
    
      if(!scene()->collidingItems(this).isEmpty()){
    
      setPos(mapToParent(0,  -5));
    
      }
    
      }
    
      if(GetAsyncKeyState('S')){
    
      setPos(mapToParent(0,  -5));
    
      if(!scene()->collidingItems(this).isEmpty()){
    
      setPos(mapToParent(0,  5));
    
      }
    
      }
    
      if(GetAsyncKeyState('C')){
    
      emit  signalBullet(mapToScene(0,25),QPointF(target->x(),target->y()));
    
      }
    
      //  borders  check
    
      if(this->x()  <  -(width  /  2)){
    
      this->setX(-(width  /  2)  +  1);  //left
    
    }
    
    if(this->x()  >  (width  /  2)){
    
      this->setX((width/2)  -  1);  //right
    
    }
    
      
    
    if(this->y()<  -(height  /  2)){
    
      this->setY(-(height  /  2)  +  1);  //up
    
    }
    
    if(this->y()>  (height  /  2)){
    
      this->setY((height  /  2)  -  1);  //down
    
    }
    
    }
```
In order to manipulate the damage score bar we implement this method here  that enables us to decrease the health of an injured tank:
```C

    void  Tank::hit(int  damage)
    
    {
    
      health  -=  damage;  //  Reduce  Target  Health
    
      emit  signalDamage(health,this->tank_ID);
    
      if(health  <=  0)  this->deleteLater();
    
      
    
    }
```
In order to set the main scene where the battle takes place we implement these methods which enable us to chose a map and to add our tanks and place them in the middle of the scene.
```C

    MainScene::MainScene(QWidget  *parent)
    
      :  QGraphicsView(parent)  {
    
      
    
      //Get  the  size  of  the  widget
    
      int  w  =  QGuiApplication::screens().at(0)->availableGeometry().width()  /2;
    
      int  h  =  QGuiApplication::screens().at(0)->availableGeometry().height()  /2;
    
      
    
      scene  =  new  QGraphicsScene();  //  Initialize  the  scene  for  rendering
    
      this->resize(w*2,  h*2  -  75);
    
      
    
      //Set  the  scene  size
    
      scene->setSceneRect(-w,-h+75,w*2  -2,h*2  -  75);
    
      
    
      this->setScene(scene);
    
      
    
      this->setRenderHint(QPainter::Antialiasing);  //Use  Antialiasing
    
      this->setHorizontalScrollBarPolicy(Qt::ScrollBarAlwaysOff);  //  Disable  the  horizontal  scrollbar
    
      this->setVerticalScrollBarPolicy(Qt::ScrollBarAlwaysOff);  //  Disable  the  vertical  scrollbar
    
      this->setAlignment(Qt::AlignCenter);  //  Binding  the  content  to  the  center
    
      this->setSizePolicy(QSizePolicy::Minimum,  QSizePolicy::Minimum);
    
      
    
      
    
      QBrush  back_brush(QColor(255,  243,  240));  //bricks  &  box
    
      //QBrush  back_brush(QColor(224,  255,  224));  //forest
    
      
    
      scene->setBackgroundBrush(back_brush);
    
      
    
      scene->addLine(-w,-h  +  75,  w,-h  +  75,  QPen(Qt::black));//upper  bound
    
      scene->addLine(-w,h,  w,h,  QPen(Qt::black));//lower  bound
    
      scene->addLine(-w,-h  +  75,  -w,h,  QPen(Qt::black));//left  bound
    
      scene->addLine(  w,-h  +  75,  w,h,  QPen(Qt::black));//right  bound
    
      
    
      
    
      MapCreator  map_creator;
    
      map_creator.setFile("C:/Users/TUF/Desktop/Tank  war/Tanks-main/Tanks/maps/map_3.txt");
    
      map_creator.CreateMap(scene);
    
      
    
      Tank::num_of_tanks  =  0;
    
      tank1  =  new  Tank();
    
      tank2  =  new  Tank();
    
      
    
    Target*  target1=new  Target();
    
    tank1->target=target1;
    
    scene->addItem(tank1->target);
    
    target_tank1.push_back(tank1);
    
      
    
      
    
    Target*  target2=new  Target();
    
    tank2->target=target2;
    
    scene->addItem(tank2->target);
    
    target_tank2.push_back(tank2);
    
      
    
      
    
    scene->addItem(tank1);  //  Add  a  tank  to  the  scene
    
    tank1->setPos(-690,-300);  //  Place  the  tank  in  the  center  of  the  scene
    
    scene->addItem(tank2);
    
    tank2->setPos(230,130);
    
      
    
      timer  =  new  QTimer();
    
      connect(timer,  &QTimer::timeout,  tank1,  &Tank::slotGameTimer1);
    
      
    
      
    
      connect(timer,  &QTimer::timeout,  tank2,  &Tank::slotGameTimer2);
    
      timer->start(1000  /  50);
    
      
    
      this->setScene(scene);
    
      connect  (tank1,&Tank::signalBullet,this,&MainScene::slotBullet);
    
      connect  (tank2,&Tank::signalBullet,this,&MainScene::slotBullet);
    
      
    
      connect  (tank1,&Tank::signalDamage,this,&MainScene::signalConnect);
    
      connect  (tank2,&Tank::signalDamage,this,&MainScene::signalConnect);
    
    }
```
we can chose between various maps which are basically txt files by linking their path here:

  ```C

    MapCreator  map_creator;
    
      map_creator.setFile("C:/Users/TUF/Desktop/Tank  war/Tanks-main/Tanks/maps/map_3.txt");
    
      map_creator.CreateMap(scene);
```

![](https://user-images.githubusercontent.com/93097467/152694872-64a63e2f-c6e9-4751-bd84-7547447b72e1.png)
![enter image description here](https://user-images.githubusercontent.com/93097467/152694880-03ea8f16-5cab-41b4-8b7d-f475dafb4a8c.jpg)
Now for the bullets part this how we handle their trajectory and general behaviour:

```C

    Bullet::Bullet(QObject  *parent)  :
    
      QObject(parent),  QGraphicsItem()
    
    {
    
      angle  =  0;  //  Setting  the  rotation  angle  of  the  graphic  object
    
      setRotation(angle);  //Set  the  angle  of  rotation  of  the  graphic  object
    
      
    
    }
    
    static  qreal  normalizeAngle(qreal  angle)
    
    {
    
      while  (angle  <  0)
    
      angle  +=  TwoPi;
    
      while  (angle  >  TwoPi)
    
      angle  -=  TwoPi;
    
      return  angle;
    
    }
    
      
    
    Bullet::Bullet(QPointF  start,  QPointF  target,  QObject  *parent)
    
      :  QObject(parent),  QGraphicsItem()
    
    {
    
      this->setRotation(0);//  Setting  the  initial  reversal
    
      
    
      
    
      this->setPos(start);//Setting  the  starting  position
    
      //  Determine  the  trajectory  of  the  bullet
    
      QLineF  lineToTarget(start,  target);
    
      //  Angle  of  rotation  towards  the  target
    
      qreal  angleToTarget  =  ::acos(lineToTarget.dx()  /  lineToTarget.length());
    
      if  (lineToTarget.dy()  <  0)
    
      angleToTarget  =  TwoPi  -  angleToTarget;
    
      angleToTarget  =  normalizeAngle((Pi  -  angleToTarget)  +  Pi  /  2);
    
      
    
      /*  Expand  the  bullets  along  the  trajectory
    
      *  */
    
      if  (angleToTarget  >=  0  &&  angleToTarget  <  Pi)  {
    
      ///  Rotate  left
    
      setRotation(rotation()  -  angleToTarget  *  180  /Pi);
    
      }  else  if  (angleToTarget  <=  TwoPi  &&  angleToTarget  >  Pi)  {
    
      ///  Rotate  right
    
      setRotation(rotation()  +  (angleToTarget  -  TwoPi  )*  (-180)  /Pi);
    
      }
    
      //  Initializing  the  flight  timer
    
      timerBullet  =  new  QTimer();
    
      //  And  connect  it  to  the  slot  to  handle  the  flight  of  the  bullet
    
      connect(timerBullet,  &QTimer::timeout,  this,  &Bullet::slotTimerBullet);
    
      timerBullet->start(20);
    
    }
    
      
    
    Bullet::~Bullet()
    
    {
    
      
    
    }
    
      
    
    QRectF  Bullet::boundingRect()  const
    
    {
    
      return  QRectF(0,0,2,4);
    
    }
    
      
    
    void  Bullet::paint(QPainter  *painter,  const  QStyleOptionGraphicsItem  *option,  QWidget  *widget)
    
    {
    
      painter->setPen(Qt::black);
    
      painter->setBrush(Qt::black);
    
      painter->drawRect(0,0,2,4);
    
      
    
      Q_UNUSED(option);
    
      Q_UNUSED(widget);
    
    }
    
      
    
    void  Bullet::slotTimerBullet()
    
    {
    
      setPos(mapToParent(0,  -10));
    
      
    
      
    
      /*  We  make  a  check  to  see  if  a  bullet  has  stumbled  on  any
    
      *  element  in  the  graphic  scene.
    
      *  To  do  this,  define  a  small  area  in  front  of  the  bullet,
    
      *  in  which  we  will  search  for  elements
    
      *  */
    
      QList<QGraphicsItem  *>  foundItems  =  scene()->items(QPolygonF()
    
      <<  mapToScene(0,  0)
    
      <<  mapToScene(-1,  -1)
    
      <<  mapToScene(1,  -1));
    
      /*  Then  we  check  all  the  elements.
    
      *  One  of  them  will  be  Bullet  herself  and  the  Hero  -  we  do  nothing  with  them.
    
      *  And  with  the  rest,  call  SIGNAL
    
      *  */
    
      foreach  (QGraphicsItem  *item,  foundItems)  {
    
      if  (item  ==  this)
    
      continue;
    
      emit  signalhit(item);
    
      this->deleteLater();  //We  destroy  the  bullet
    
      }
    
      
    
    /*  Checking  field  out  of  bounds
    
      *  If  the  bullet  flies  out  of  the  specified  boundaries,  then  the  bullet  must  be  destroyed
    
      *  */
    
      if(this->x()  <  -800){
    
      this->deleteLater();
    
      }
    
      if(this->x()  >  800){
    
      this->deleteLater();
    
      }
    
      
    
      if(this->y()  <  -800){
    
      this->deleteLater();
    
      }
    
      if(this->y()  >  800){
    
      this->deleteLater();
    
      }
    
    }
```
Now for the score we use a class named paintevent to create the damage widgets:
```C

    void  Score::paintEvent(QPaintEvent  *event)
    
    {
    
      Q_UNUSED(event);
    
      
    
      QPainter  painter1(this);
    
      QPainter  painter2(this);
    
      
    
      painter1.save();
    
      painter1.setBrush(Qt::blue);
    
      if(!hp1.isNull())
    
      painter1.drawRect(hp1);
    
      painter1.restore();
    
      
    
      painter2.save();
    
      painter2.setBrush(Qt::green);
    
      if(!hp2.isNull())
    
      painter2.drawRect(hp2);
    
      painter2.restore();
    
    }
```
![enter image description here](https://user-images.githubusercontent.com/93097467/152694889-94adc13d-dbdd-4d93-8514-395dd7729566.jpg)

Finally in order to handle the damage variations we implement tose methods that enable us decrease the health rate with each hit from a bullet until loss of one of the sides .
```C

    void  Score::slotDamage(int  health,int  ID){
    
      std::ofstream  fout;
    
      if(ID==1){
    
      resize(length*health/maxHealth,  this->height(),1);
    
      if(health<=0){
    
      std::ofstream  fout;
    
      fout.open("Results.txt");
    
      fout  <<  "Player2 WON!!!!";
    
      emit  signalEndOfTheGame(2);
    
      }
    
      }
    
      else  if(ID==2){
    
      resize(length*health/maxHealth,this->height(),2);
    
      if(health<=0){
    
      fout.open("Results.txt");
    
      fout  <<  "Player1 WON!!!!";
    
      emit  signalEndOfTheGame(1);
    
      }
    
      }
    
      fout.close();
    
    }
    
      
    
    void  Score::resize(int  width,int  height,int  ID){
    
      if  (width  <  0)  width  =  0;
    
      
    
      if(ID==1){
    
      hp1  =  QRectF(100,  0,  width,height);
    
      update();
    
      }else  if(ID==2){
    
      hp2  =  QRectF(SCORE_WIDTH  -  100,  0,  -width,height);
    
      update();
    
      }
    
    }
    
```
![](https://user-images.githubusercontent.com/93097467/152694889-94adc13d-dbdd-4d93-8514-395dd7729566.jpg)
We finally add a **slotgameover**event for showing the winner and giving the player two options either entering a new game or exit the game.
Here are the methods responsible for the task:
```C

    void  MainWindow::slotGameOver(int  ID){
    
      QWidget  *game_over=new  QWidget();
    
      this->setCentralWidget(game_over);
    
      
    
      QVBoxLayout*  vlayout  =  new  QVBoxLayout();
    
      QHBoxLayout*  hlayout  =  new  QHBoxLayout();
    
      QPushButton*  newgame  =  new  QPushButton("New Game");
    
      QPushButton*  exit  =  new  QPushButton("Exit");
    
      
    
      game_over->setLayout(vlayout);
    
      hlayout->addWidget(newgame);
    
      hlayout->addWidget(exit);
    
      hlayout->setContentsMargins(100,  100,  0,  10);
    
      
    
      
    
      
    
      if(ID==1){
    
      
    
      QLabel  *label1  =  new  QLabel();
    
      label1->setFrameStyle(QFrame::Plain  |  QFrame::Box);
    
      label1->setStyleSheet("background-color:rgb(68, 70, 171);");
    
      label1->setText("Player1 Won!!!! Congratulations!!!");
    
      label1->setAlignment(Qt::AlignTop  |  Qt::AlignCenter);
    
      
    
      QFont  font1=  label1->font();
    
      font1.setPixelSize(75);
    
      label1->setFont(font1);
    
      
    
      label1->setSizePolicy(QSizePolicy::Minimum,  QSizePolicy::Minimum);
    
      vlayout->addWidget(label1);
    
      
    
      }else  if(ID==2){
    
      
    
      QLabel  *label2  =  new  QLabel();
    
      label2->setFrameStyle(QFrame::Plain  |  QFrame::Box);
    
      label2->setStyleSheet("background-color:rgb(68, 70, 171);");
    
      label2->setText("Player2 Won!!!! Congratulations!!!");
    
      label2->setAlignment(Qt::AlignTop  |  Qt::AlignCenter);
    
      
    
      QFont  font2=  label2->font();
    
      font2.setPixelSize(75);
    
      label2->setFont(font2);
    
      
    
      label2->setSizePolicy(QSizePolicy::Minimum,  QSizePolicy::Minimum);
    
      vlayout->addWidget(label2);
    
      
    
      }
    
    vlayout->addLayout(hlayout);
    
      
    
    connect(newgame,  &QPushButton::clicked,  this,  &MainWindow::NewGame);
    
    connect(exit,  &QPushButton::clicked,  this,  &MainWindow::Exit);
    
      
    
    }
```
![enter image description here](https://user-images.githubusercontent.com/93097467/152694909-57876782-260c-41f8-ba28-7503abe56fa4.jpg)

This mini video here shows a round played  by us :
[enter link description here](https://user-images.githubusercontent.com/93097467/152695834-e1f305f8-18f4-4c82-b1f1-c4b9d5c0d7be.mp4)
## Conclusion
This project turned out to be very rewarding insofar as it consisted of a concrete approach to the interface development profession.
also, it was a very important experience for us to be familiar with this material and to develop our skills .
many thanks to the teacher who made that opportunity of learning possible.

**Made By:**

 - Zakaria ELOFIR -cybersecurity.
 - Abdellah guendouli -web developpement.

