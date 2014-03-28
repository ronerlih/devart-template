# Project Title
Insert the name of your project

## Authors
- Ron Erlih, github account:ronerlih
- Doron aviguy
- 
## Description
Mediums consists of 4 pieces that show people reflections of themselves crossed with information from the web.

Google Mirror, 2013 
A live video of the space. Every few seconds a current frame is used to search by Google’s “Image similarity” a similar image and dissolves it in and out of the projection, and moves on to search for a new image.

Mirror, 2012 
If looked at for few seconds, the viewer’s reflection appears,and his/hers facial expression is analysed in real time (by “NViso - Facial expressions). 

YouTube mirror, 2013, sound work. 
Uses the facial expression result to search and play the vocals of a YouTube clip by the speakers on the floor.

Twitter Mirror, 2013
According to a Facial expression analysis of the viewer by the piece in front of it, the work searches for relevant tweets from twitter.

Background
All of the pieces in Mediums are live reflections of us.

In a larger perspective, the body of work containing Google Mirror functions as an 
agent of information between the space/viewers and data from the web.

Thank you for your attention,
Ron Erlih



## Link to Prototype
NOTE: If your project lives online you can add one or more links here. Make sure you have a stable version of your project running before linking it.

[Example Link](http://www.google.com "Example Link")

## Example Code
NOTE: Wrap your code blocks or any code citation by using ``` like the example below.
```
///Google image code
import codeanticode.gsvideo.*;

import processing.video.*;
import processing.net.*;

import processing.net.*;
import java.net.*;

import java.io.*;
import org.seltar.Bytes2Web.*;
  int keypress=0;
int[] maskArray = new int[width*height];
int[] sendMaskArray = new int[width*height];
GSCapture cam;

int imgCycle=10000;
int currentTime=0;
int imgPickupTime=0;
int x=0;
int numPixels;
int[] previousFrame;
PImage[] frames = new PImage[3];
boolean sameImg=false;
int blendingCounter=20;
int tinting=10;
int state=0;
int lastState=0;
float t=255;
float TintFlag=0;
ImageToWeb img; 
//google logo
PImage googleLogo;
String url="http://getimgx.aws.af.cm/lastSearchedImage";
 String uploadUrl ="http://getimgx.aws.af.cm/upload.php";
PImage GoogleSearchImg;
PImage GoogleSearchImgResized;
PImage GoogleSearchImgBkp;
//url2[0]="  "

Client c;
String match;
String Search="http://images.google.com/searchbyimage?image_url=https%3A%2F%2Fdl.dropbox.com%2Fu%2F31433565%2Fmetafora%2Fsearching.JPG";
//"https://www.google.com/search?tbs=sbi:AMhZZitiDUQMg7MNi_1KznYQiKtC9ZmkcXJdPhZcevD95-1xqQUcuZeZ2rossablGxwAJaSezblmdeCTat9b0KCWtvw1H2JSWrIXotseUbTG8xk-4IjixQfGDdwfpavvmTeNT1OpQYkol1EkWvvELSX2KD5dww7ur00aOMog-qAHyV8047v7t5k-payV1XlFkIbUSpfV7uHe4QUm6RKlxpitMZ8N0tpalSIUy-w2IDmKAt0lkDslQ8qXnfrwQdqnWMr69D6xiQKvzDhQ-4xJhuCzEe6seXEhudn3smAByd1jdlo5cWMnaviueq6jAukt1-nx1KtW_1qI_1ntiDkBpP8YSrqTI1xe1tNwBAkJc7ZFDu_13I-Wg4W4h-kLR1Oo0IWYFPZyBls8smCM0Jq-hnK39RIejGfh30uAb6pekz4qdODVY1H_1EkV6BZqBvnm5BrAVzwwei5fBsoSB7g5PoxJRSMYapJCOf8nX0aQjx2CF-cD5uHtMevNDAYU9fUWP1st5YkWNmU9Q8rjeYv4tn-Di8pk43_1XAHciFsXsgaDoEDNROga7OUBJtjOErs39yXFvr6MplcgxU3R5Ea6YBBud_1ojCVJd82lnARmcVw8SHWMAPx09m6ud2goEjX-oqs2EJL6A42dGfxc5hJbkBxBrwziC0wjf0WN6XG3LQ56ZcEGKyTYHf5CdofXFmyGEFyhGIKisQa2b9Y3oLxW8nmNanqYb1AEMRQggievWXM7ISkm4TRFxp7_1P-kYrlqKe5UqQuNeFkC3REnIL8r9oMJGPNyMEhHRmeZ8d4w15plkyO_1rohA9DJA_1VuI8F0yrArQL3wignS4gNenlbuJTAEp1bA_18kt43_1OJ4B-DxXj4Q3c4CPeyo30XROgwtdwVzWloXLclufzRKuG9hjiNOfd9ryJRowDcVnpFDInts1ZEafm5kZPTO2p8c5K4YWXkbZPIjxPebWiO7eLCjsX8tJ-tlY_1nsj7Glt0QN2rCzJe3uj5iIqIp6AP4K8vpJFkqdeP4My-YEI3EJJ1Asql55jYmFFivr3RDx_1v86kFdRZG1uKIy3r6LKiHkSf9Gv4iq7HgYmHjyOVyJQB9Bqt1bi_1hHvcaq0KapNBTFv7xq12PBYmp1P8SpKE3ukvYLndJ01haoqkkPbHw-sH-btUFoXc7qzfzoashvedoJe-fNdJMaQHpbR_1FxhNKVLf3yfb4gKLxy67S1hTyWRZ4Cn0qAM02B2JIB4xVMEkl5sl4yy5rGxQ4DN5jmv9L9PiSjQ1A3BVN8jQ_1tC8TKyUEgA_18RKWSU68oz4IMElhB5cJy1h4GhI2ksGeCqnAjRFucgCG4"
//https://www.google.com/search?tbs=sbi:https%3A%2F%2Fdl.dropbox.com%2Fu%2F31433565%2Fmetafora%2Fsearching.JPG
//http://images.google.com/searchbyimage?image_url=https%3A%2F%2Fdl.dropbox.com%2Fu%2F31433565%2Fmetafora%2Fsearching.JPG";
 // imgSearchUrl += encodeURIComponent(imgUrl);
String data;
String result[];
String html;
String[][] mtchs;
int k=2;
PGraphics pg;

void setup() {
 
    size(800, 600);
    //seethrough frame@@@@@!!!!
  //   frame.removeNotify(); 
  //  frame.setUndecorated(true);
    ////////
cam = new GSCapture(this, width, height/*,"USB Video Class Video:1"*/);  
  cam.start();
    pg = createGraphics(width, height,P2D);

  pg.beginDraw();
 // pg.background(102);
  pg.fill(0);
  pg.rect(0,0,width,height);
  pg.endDraw();
  
  maskArray = new int[width*height];
  for(int i=0; i<width*height;i++){
         maskArray[i]=255;}
  sendMaskArray = new int[width*height];
  for(int i=0; i<width*height;i++){
         sendMaskArray[i]=120;}
  //cam = new Capture(this, 800, 600);
  frames[0]=cam.get();
  frames[1]=frames[0];
  frames[2]=frames[0];
  GoogleSearchImg=frames[0];
  GoogleSearchImgResized=frames[0];
//differencing
  int movementSum = 0;

  numPixels = cam.width * cam.height;
  previousFrame = new int[numPixels];
  loadPixels();
//  googleLogo=loadImage("images_logo_lg.gif");similar_images_labs_logo_large.gif
  googleLogo=loadImage("similar_images_labs_logo_large.gif");
}

void captureEvent(GSCapture cam) {
  cam.read(); 


 
}
void blending(){
/////////////////////////null pointer exception--> show last searched img
  if (GoogleSearchImg!=null){
   //   GoogleSearchImg.resize(width,height);

    GoogleSearchImgBkp=GoogleSearchImg;
   //  GoogleSearchImgBkp.mask(pg);//////////////////////////////////////////8888888888888888
   if(GoogleSearchImgBkp.height*GoogleSearchImgBkp.width==height*width){
    GoogleSearchImgBkp.mask(maskArray);}
    
     
     image(GoogleSearchImgBkp,0,0);
//image(googleLogo,width-148,10);

      }else{
         GoogleSearchImgBkp.mask(maskArray);
        image(GoogleSearchImgBkp,0,0);
            println("in exception: 'null pointer exception'");
          println("showing last searched img");}
///////////////
  /* if(t==255){
      delay(600);
   }*/
//  tint(255,t);
 // delay(40);

 
  }

void backgroundCam(){
    background(cam);}
    
void draw() {
// removeCache(GoogleSearchImg);
 //removeCache(GoogleSearchImgBkp);
 if (t==255){
   delay(1500);}
        background(cam);
               
//    image(googleLogo,width-148,10);
/*
fill(250,240,230,20);
rect(0,0,width,height);
*/
     
      
        
     
     
  /*    currentTime=millis();
        if(currentTime>imgPickupTime+imgCycle){
          frames[1]=frames[2];
          frames[1]=cam.get();
          imgPickupTime=millis();
          differencing(); */
    
     t=t+TintFlag;
      for(int i=0; i<width*height;i++){

         maskArray[i]=int(t);}
     if(t==255){
       TintFlag=-1;
     }
 if(sameImg==false && t==0) {
   println("in send");
         send();
         
         TintFlag=1;
        
 }
             blending();
println("t: "+t);


  }

  
 // if (state==0){
   // lastState=0;
  //}
 

void send(){
  try{
  println("actualy in send");
   img  = new ImageToWeb(this);
   // img.save("####",true);
  // img.resize(400,300);
    img.post("img",uploadUrl,"jpg-test",true,img.getBytes(g));
      println("after post");

    String getImageSearchUrl[] = loadStrings(url); 
    String getImageSearchUrlBkp = getImageSearchUrl[0];
    
 
  if(getImageSearchUrl != null){
 println("image URL:   "+getImageSearchUrl[0]);
if(getImageSearchUrl[0]!=null){
  GoogleSearchImg =loadImage(getImageSearchUrl[0]);}
  GoogleSearchImg.resize(width,height);
  GoogleSearchImg.set(width-148,10,googleLogo);
  //image(show,0,0);
  }else{GoogleSearchImg=loadImage(getImageSearchUrlBkp);
  }}
   catch (Exception e){
       println("Exception");
       
    } 
}
  

```
----------
```
//Twitter mirro code
String tweetText="empty";
String tweetText2="empty";

color emotionColorCurrent;
color emotionColor;
color emotionColorLast;
boolean colorFlag=false;
int Tcounter=0;
int sendCounter=millis();

//String EmotionImgRecieve ="http://getimg2.eu01.aws.af.cm/getImgEmotionurl.old";

String bigEmotionValueRandom = "neutral";
String[] stringXYsplit;
String[] StringXY = new String[300];
String StringXYurl="http://getimg2.eu01.aws.af.cm/getImgEmotioDots";
String[] bigEmotionValue = new String[3];
String EmotionImgRecieve ="http://getimg2.eu01.aws.af.cm/getImgEmotionurl";
String EmotionImgSend ="http://getimg2.eu01.aws.af.cm/uploadImgEmotion";
String tweeterGetUrl ="http://getimg2.eu01.aws.af.cm/searchtwit?q=";
String[] tweetResult;
String[] a;

String[] getImageEmotion;
//String[] imgEmotionResult;
PFont font;
color lastEmotionColor=0;
color lastEmotionColorBuffer=0;
PImage textBox;
float TextSizeFlag=5;


float t=255;
float XposMap=0;
float Tbuffer;
int randomIndex=0;
PFont fontA;
PFont fontB;
//logo
PImage twitterLogo;
void setup() {
 
    //seethrough frame@@@@@!!!!
//frame.removeNotify(); 
  //  frame.setUndecorated(true);
    ////////
    twitterLogo=loadImage("twitter-logo.png");
     background(0);
  send();
  size(1024,768/*,P3D*/);
    emotionColorLast=emotionColor;
  // Parameters go inside the parentheses when the object is constructed.
  myCar1 = new Car(tweetText,1024,384,2); 
//  myCar2 = new Car(tweetText2,0,10,1);
    fontA = loadFont("PicoWhiteAl-48.vlw");
  fontB = loadFont("Helvetica-48.vlw");
 
  bigEmotionValue[0]="neutral";
  bigEmotionValue[1]="neutral";
  bigEmotionValue[2]="neutral";

}

 
void send(){
  StringXY = loadStrings(StringXYurl);
 try{
   TextSizeFlag=0;
  println("actualy in send");
    println("after post");


   getImageEmotion = loadStrings(EmotionImgRecieve); 
   if (getImageEmotion==null){
     getImageEmotion= new String[1];
     getImageEmotion[0]="neutral";
   println("Emotions :"+getImageEmotion[0]);
   }
   

   String[] listEmotions = split(getImageEmotion[0], ',');
   println("listEmotions "+ listEmotions[0]);
   String[] sadnessList = split(listEmotions[1], ':');
   String[] disgustList = split(listEmotions[2], ':');
   String[] angerList = split(listEmotions[3], ':');
   String[] surpriseList = split(listEmotions[4], ':');
   String[] fearList = split(listEmotions[5], ':');
   String[] happinessList = split(listEmotions[6], ':');
   
   println("sadnessList: "+sadnessList[0]); 
   
   float sadness = float(sadnessList[1]);
   float disgust = float(disgustList[1]);
   float anger = float(angerList[1]);
   float surprise = float(surpriseList[1]);
   float fear = float(fearList[1]);
   float happiness = float(happinessList[1]);
   
 if(sadness>disgust && sadness>anger && sadness>surprise && sadness>fear && sadness>happiness ){
       bigEmotionValue[0] =  "sadness";
       bigEmotionValue[1] = "down";
       bigEmotionValue[2] = "miserable";
   }
 if(disgust>sadness && disgust>anger && disgust>surprise && disgust>fear && disgust>happiness ){
   
       bigEmotionValue[0] =  "disgust";
       bigEmotionValue[1] = "loath";
       bigEmotionValue[2] = "nausea";  
  }
 if(anger>sadness && anger>disgust && anger>surprise && anger>fear && anger>happiness ){
     
       bigEmotionValue[0] =  "anger";
       bigEmotionValue[1] = "rage";
       bigEmotionValue[2] = "upset";    
 }
 if(surprise>sadness && surprise>disgust && surprise>anger && surprise>fear && surprise>happiness ){
       bigEmotionValue[0] =  "surprise";
       bigEmotionValue[1] = "wonder";
       bigEmotionValue[2] = "shock";    
  }
 if(fear>sadness && fear>disgust && fear>anger && fear>surprise && fear>happiness ){

       bigEmotionValue[0] =  "fear";
       bigEmotionValue[1] = "terror";
       bigEmotionValue[2] = "creeps";    
}
 if(happiness>sadness && happiness>disgust && happiness>anger && happiness>surprise && happiness>fear ){
       bigEmotionValue[0] =  "happiness";
       bigEmotionValue[1] = "joy";
       bigEmotionValue[2] = "love";   
  } 
// println("emotion compar: "+ bigEmotionValue);
randomIndex = int(random(bigEmotionValue.length));
 bigEmotionValueRandom=bigEmotionValue[randomIndex];
 
 
 float r=((map(anger,0,0.3,0,255)+map(happiness,0,0.3,0,255))/2);
 float g=((map(fear,0,0.3,0,255)+map(disgust,0,0.3,0,255))/2);
 float b=((map(sadness,0,0.3,0,255)+map(surprise,0,0.3,0,255))/2);
 println("rgb :"+r+","+g+","+b);
 //
 //
 int rr=int(r);
 int gg=int(g);
 int bb=int(b);
  println("rgb int :"+rr+","+gg+","+bb);
 println("sd");
 emotionColor=color(r,g,b);
 println("emotion color: " + emotionColor);

 tweetResult = loadStrings(tweeterGetUrl+bigEmotionValueRandom);
 if(tweetResult == null ){
   tweetResult = new String[1];
   tweetResult[0]="empty";
   println("tweeter result: " +tweetResult[0]);
 }
tweetText= tweetResult[0];
  //textFont(fontA, 25);
textSize(35);
 }

  catch (Exception e){
      println("Exception");
    } 
}
 
 
void draw() {
  
  background(0);
  
    //send();
  
  //  tint(emotionColorLast,t);
   
//fill(emotionColorLast,t);
    Tbuffer=map(t,0,255,255,0);
     //rect(0,0,width,height);
  /*   if(Tbuffer<50){
    fill(emotionColor,Tbuffer);///the only line that is really necessary
      }
    println("Tbuffer: "+Tbuffer);
   beginShape();
if(Tbuffer<200){fill(emotionColor,Tbuffer);}else{
    fill(emotionColor,255);}*/
    noFill();
     
// "laDefense.jpg" is 100x100 pixels in size so
// the values 0 and 100 are used for the
// parameters "u" and "v" to map it directly
// to the vertex points
noStroke();
vertex(0, 0);
vertex(width/*-20*/, 0);
vertex(width/*-33*/, height);
vertex(0, height);
endShape();
   // rect(0,0,width,height-100);
    println("EMOtion color!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"+emotionColor);
  println ("emotionColorCurrent :"+emotionColorCurrent);
    println ("emotionColor :"+emotionColor);
  println ("colorFlag :"+colorFlag);
  println ("Tcounter :"+Tcounter);

  
   // translateEmotionGoeth();
//background(emotionColorCurrent,Tcounter);

      
      
  myCar1.drive();
  myCar1.display();
 // myCar2.drive();
 // myCar2.display();
}

// Even though there are multiple objects, we still only need one class. 
// No matter how many cookies we make, only one cookie cutter is needed.
class Car { 
  String tweet;
  float xpos;
  float ypos;
  float xspeed;

  // The Constructor is defined with arguments.
  Car(String tweet, float tempXpos, float tempYpos, float tempXspeed) { 
    tweet = tweetText;
    xpos = tempXpos;
    ypos = tempYpos;
    xspeed = 5;
  }

  void display() {
    //fill(20*sin(t),(10+(sin(t/255))),4*(10-(sin(t/255))));
     //textFont(fontA, 35);
   //stroke(255);
 //fill(35,155,233,Tbuffer+20);
image(twitterLogo,width/2-70,150);
    // text("Follow:",50,140);
     textFont(fontB, 40);
     textAlign(CENTER);
    fill(255);
    stroke(0);
    float textPositionZ;
   // if(t>225){textPositionZ=t*3-500;}else{textPositionZ=t-225;}
    
    text(tweetText,50,260,width-150,height-80);
    
    //rectMode(CENTER);
    //rect(xpos,ypos,20,10);
  }

  void drive() {
    xpos = xpos - xspeed;
    if (xpos+(tweetText.length()*170) < 0) {
      emotionColorLast=emotionColor;
     
      xpos = 1024;
    }
    XposMap=-(tweetText.length()*170);
    t=map(xpos,XposMap,width,0,255);
    println("TTTTTTTTTTTTTTTTTTTTTTTTTTTTTT:  "+t);
 
  if(  sendCounter+8000<=millis()){
    send();
    sendCounter=millis();
}
  }
}
```

----------
```
//Mirror and Youtube mirror code:

//import gifAnimation.*;

import hypermedia.video.*;
import processing.video.*;
import codeanticode.gsvideo.*;
import java.awt.Rectangle;
import hypermedia.video.*;

import java.io.*;
import org.seltar.Bytes2Web.*;
 import java.net.*;

import java.awt.image.BufferedImage;
Blob[] blobs;

float tt=255;
int ttFlag=0;
int CaptureLength=15;
int faceX=0;
int faceY=0;
int    faceW=0;
int    faceHeight=0;
float[] X = new float[102]; 
float[] Y = new float[102]; 
//youTube url: http://getimg2.eu01.aws.af.cm/radio.html
//youTube parameteres
String youTubeSetUrl = ("http://getimg2.eu01.aws.af.cm/set/radio/");
String[] youTube;
byte[] youTubeBytes;


int randomIndex=3;
int randomI;
String bigEmotionValueRandom="neutral";
String[] stringXYsplit;
String[] StringXY = new String[300];
String StringXYurl="http://getimg2.eu01.aws.af.cm/getImgEmotioDots";
String[] bigEmotionValue = new String[4];
String EmotionImgRecieve ="http://getimg2.eu01.aws.af.cm/getImgEmotionurl";
String EmotionImgSend ="http://getimg2.eu01.aws.af.cm/uploadImgEmotion";
String tweeterGetUrl ="http://getimg2.eu01.aws.af.cm/searchtwit?q=";
String[] tweetResult;
String[] a;
ImageToWeb img; 
String[] getImageEmotion;
//String[] imgEmotionResult;
PFont font;
color emotionColor=255;
color lastEmotionColor=0;
color lastEmotionColorBuffer=0;
PImage textBox;
float TextSizeFlag=5;
PImage camm;

PImage onlineFaceImg;
//online faces parameters
//PImage[] onlineFaces;
PImage onlineFacesImage;
//Cell[][] grid;
//int cols = 6;
//int rows = 8;
//PImage im;
PImage cellImage;
PImage grid;

//recording parameters
int i=0;
int io=0;
PImage[] live = new PImage[CaptureLength];
PImage[] oldLive = new PImage[CaptureLength];
//state parameters
int state=0;
int lastState=0;
PImage camFrame;

//detect parameters
int faceWide=0;
OpenCV opencv;
GSCapture cam;
int faceTime=0;
int noFaceTime=0;
PImage opencvImage;

//blend parameters:
float t=255;
float Tbuffer;
//Gif gifLoad = new Gif(this,"ajax-loader.gif");
//int GifCounter=0;
PGraphics pglive;
PGraphics pgoldLive;
int[] imagePixels;

int[] rectPixels;
//PImage[] gifLoad;
//int gifSize=3;
PImage liveCam; 
long allocated;
int firstLoop=1;
PImage nvisoLogo;
PImage youtubeLogo;

Rectangle[] faces;

void setup() {
  nvisoLogo=loadImage("nviso.jpg");
  youtubeLogo=loadImage("youtubeLogo.jpeg");
  //seethrough frame@@@@@!!!!
  //   frame.removeNotify(); 
  //  frame.setUndecorated(true);
background(0);
      //seethrough frame@@@@@!!!!
    // frame.removeNotify(); 
    //frame.setUndecorated(true);
    ////////
  fill(46,67,138);
rect(0,0,width/2,80);
image(nvisoLogo,100,20);
//gifLoad = Gif.getPImages(this, "facebook-loading.gif");
 //  gifSize= gifLoad.length-1;
  size(1600, 600);
//  gifLoad.ignoreRepeat();
  //  gifLoad.loop();
    bigEmotionValue[0]="neutral";
  bigEmotionValue[1]="neutral";
  bigEmotionValue[2]="neutral";
   // send();

 // frameRate(fps);
  //detection camera:
  opencv = new OpenCV(this);
  opencv.allocate(width/2, height);   
  opencv.cascade( OpenCV.CASCADE_FRONTALFACE_ALT );  

  cam = new GSCapture(this, width/2, height/*,"USB Video Class Video:0"*/);  
  cam.start();
  
  for(int x=0;x<CaptureLength;x++){
   oldLive[x]=loadImage("ref.jpg"); 
  }
  youTube = new String[10];
  youTubeBytes= new byte[10];
    //StringXY = loadStrings(StringXYurl);
//grid=loadImage("widegrid2.png");
//online faces cells
/*//im=loadImage("rect.jpg");

//grid = new Cell[cols][rows];
  //for (int i = 0; i < cols; i++) {
    //for (int j = 0; j < rows; j++) {
      // Initialize each object

   //   grid[i][j] = new Cell(width/2+i*width/16,j*height/6, im);
      //image(im,width/2+j*width/16,i*height/6);
    }
  }
  
  int setupCellsCounter =0;
  if(setupCellsCounter<50){
    onlineFaces.add(setupCellsCounter,im);
    setupCellsCounter++;
}*/
//onlineFaces = new PImage[8*16];


imagePixels = new int[(width/2)*height];
}
void captureEvent(GSCapture cam) {
  cam.read(); 


 
}

void detect() {
  //  background(cam);
  faceWide=0;
  //cam.read();
 // image(cam, 0, 80);
  opencv.copy(cam);
  opencv.read();
  opencv.convert(GRAY);
//camm = opencv.image();
  //fill(0, 0);
//opencvImage= opencv.detect(1.2, 2, OpenCV.HAAR_DO_CANNY_PRUNING, 40, 40);
  /*Rectangle[]*/ faces = opencv.detect(1.2, 2, OpenCV.HAAR_DO_CANNY_PRUNING, 40, 40);
    //  stroke(100, 0,0);

  for (int i = 0; i < faces.length; i++) {
   // stroke(100);
    rect(faces[i].x, faces[i].y+80, faces[i].width, faces[i].height);
    faceX=faces[i].x;
    faceY=faces[i].y;
    faceW=faces[i].width;
    faceHeight=faces[i].height;
   // fill(0, 0);
    //stroke(0, 0);
    if (i+1==faces.length) {
      faceWide=faces[i].x;
      faceTime=millis();
    }
  }
  println(faceWide);
  if (faceWide<25) {
    noFaceTime=millis();
  }
  image(oldLive[io],0,0);
   
  noTint();
 
 // tint(255,0);
   // draw blob results
 //  fill(0);
//   stroke(0);
  // blobs = opencv.blobs( 100, 100, 100, true);


  }


void innitialState() {
  
  // if(t==0){t=255;}
 // else{t=t-2.5;}
  if(io>=CaptureLength-2){
  io=1;
}

    detect();
    if ((faceWide>25 && faceWide<800)  && faceTime+200>millis()){
      state=1;
      lastState=0;
    
      i++;
}



// fill(254,254,254);

font = loadFont("FacebookLetterFaces-Regular-48.vlw"); //AdobeHebrew-Bold-48,DialogInput-48,AndaleMono-48,AppleCasual-48,ArialRoundedMTBold-48,Baskerville-Bold-48

  
image(oldLive[io],0,0);
  io++;
  noTint();
 

}

void faceDetected(){

 //io--;/////////////////////////////////////////////////////////freeeeeez the img
   if(io>=CaptureLength-1){

  io=0;
//}
if(i>=CaptureLength){
  i=1;
}}
  detect();
  
// if(faceTime-noFaceTime>2000){
  if ((faceWide>25 && faceWide<800)  || faceTime+600>millis()){
  record();
//println("in record loop");
  //image(oldLive[io],0,0);
  }else{
    state=0;
   // image(oldLive[io],0,0);
  // live=oldLive;
  }
  lastState=1;
  
  image(oldLive[io],0,0);
  io++;

}
void record(){

    fill(46,67,138);
rect(0,0,width/2,80);
image(nvisoLogo,width/2-180,10);
image(youtubeLogo,width/2-320,10);


   //  image(gifLoad[GifCounter],40,35);
//if(GifCounter==gifSize){

 // GifCounter=0;}else{GifCounter++;}

 fill(254,254,254);

font = loadFont("FacebookLetterFaces-Regular-48.vlw"); //AdobeHebrew-Bold-48,DialogInput-48,AndaleMono-48,AppleCasual-48,ArialRoundedMTBold-48,Baskerville-Bold-48
textFont(font,52);
println("emotion: " +bigEmotionValueRandom);
  text(bigEmotionValue[0],140,55/*,-t*20+100*/);
  textSize(14);
  text("sending key word ..",340,55/*,-t*20+100*/);  
  if(lastState==0){
    i=0;
  }
  if(i<CaptureLength-1){
   // cam.read();
   // camFrame=cam;
   image(cam,0,80);
   camFrame=get(0,0,width/2,height);
//  camFrame.copy(cam,0,0,width,height);
 live[i]=camFrame;
i++; 

//println("adding frame");
}else{
  /*PImage onlineFace;
  onlineFace=get(faceX-60, faceY-40, faceW+60, faceHeight+80);
    onlineFace.resize(width/16,height/6);
    onlineFaces.add(0,onlineFace);*/
  image(oldLive[i],0,80);
 
//fonlineFaces();
send();
 
      
  state=2;
 // i=1;
}
}

void blending(){
 // image(gifLoad, 10,10);


   if(i>CaptureLength-3){
    i=1;
  }
  if(io>CaptureLength-3){
    io=1;
  }
//  delay(10);

 //  if(faceTime-noFaceTime>2000){
 
// print("I is:");
 //println(i);

  
  image(live[i],0,0);/////////////////////////////live[i]
     noStroke();



   i++;
   io++;
//   imagePixels[]=t;
      oldLive[io].loadPixels();

  for (int i = 0; i < width/2*height; i++) { 
    imagePixels[i] = int(t); 
  } 
   oldLive[io].mask(imagePixels);
image(oldLive[io],0,0);/////////////////////
 io++;
// tt=t;
  if(t>0){
    delay(100);
 // tint(255,t);
    t=t-(255/(CaptureLength-3));
  }else{
   
  
 //   thread("send");///////////////////
    println("for loop!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!");
    for(int x=0;x<CaptureLength-1;x++){
  //  live[x].save(x+".jpg");
    //oldLive[x]=loadImage(x+".jpg");
oldLive[x]=live[x];
    }
    state=0;
    faceTime=0;
    noFaceTime=0;
    i=0;
    io=0;
    t=255;
   // noTint();
   state=0;
  }
//  }else{
  //state=0;}
   

}


void draw() {
 
;
allocated = Runtime.getRuntime().totalMemory();
long free = Runtime.getRuntime().freeMemory();
println("allocated memory: "+allocated);
println("free memory: "+free);
//image(liveCam,0,80);
  //   onlineFaces();

 if(state==2){
  if (lastState!=2){
  }
 blending(); 

lastState=2;
 
}
  if(keyPressed==true){
    send();
  }
  if (state==0) {
 
    innitialState();

  }
  if (state==1){
  



    faceDetected();
 fill(255);
  rect(10,68,((width/2)/CaptureLength)*i,10);
}  



for(int i=CaptureLength-1;i>=0; i--){
  
removeCache(oldLive[i]);
//removeCache(live[i]);

}
/*removeCache(camFrame); 
removeCache(liveCam);
removeCache(pglive); 
removeCache(pgoldLive);
*/
/*
print("face detected time: ");
print(faceTime);
print(", no face detected time: ");
println(noFaceTime);

print("state: ");
println(state);
print("i: ");
println(i);
print("io: ");
println(io);*/
//fill(0);
int emotionColorTint=80;
/*if(state!=2){
if (io<=30){
Tbuffer=map(i,0,30,20,150);
}
if (io>30){
Tbuffer=map(i,30,60,150,60);
}
fill(emotionColor,Tbuffer);///////////////////////////////////////////////////////////////

}
else{*//*
  if (t>=125){
Tbuffer=map(t,0,255,250,0);
}
if (t<125){
Tbuffer=map(t,255,0,250,0);
}*/
//fill(emotionColor,Tbuffer);///////////////////////////////////////////////////////////////


  
//noStroke();
//rect(0,80,width,height);
//fill(emotionColor,Tbuffer);///////////////////////////////////////////////////////////////


  
//noStroke();
//rect(0,80,width/2,height);

//   fill(0);
  



//filter(emotionColor);
//stroke(255,0,0);
//noFill();


/*
line(width/3,height/3+51,width/3+151,height/3+51);
line(width/3+151,height/3+51,width/3+151,height/3+1);
filter( BLUR, 4 );
*/



//shadowW.filter(BLUR,4);


//

//stroke(232,232,232);

//fill(128);
/*image(gifLoad[GifCounter],40,45);
if(GifCounter==gifSize){

  GifCounter=0;}else{GifCounter++;}
*/
//textSize(100);

//  noFill();
  //stroke(60);
//  line((width/2)-30-bigEmotionValue[randomIndex].length()*12,70,(width/2)+bigEmotionValue[randomIndex].length()*12,70);
//  line((width/2)-30-bigEmotionValue[randomIndex].length()*12,71,(width/2)+bigEmotionValue[randomIndex].length()*12,71);

//opencv.read();           // grab frame from camera
//image( opencv.image(), 0, 0 );                   // show original
if(state!=2){
  //  opencv.restore();
 // opencv.capture(width/2, height);
// opencv.threshold(80 );
//
////  opencv.ROI( faceX-50, faceY-100, int(faceW*1.5), int(faceHeight*2));
//    //  opencv.absDiff();                            // make the difference between the current image and the image in memory
//  opencv.invert();
//
//  Blob[] blobs = opencv.blobs( 250, width/2*height/2, 10 , true, OpenCV.MAX_VERTICES );
// //noStroke();
// stroke(emotionColor,tt);
//  //noFill();
//  fill(emotionColor,tt);
//
//   // draw blob results
//   for( int i=0; i<blobs.length; i++ ) {
//       beginShape();
//       for( int j=0; j<blobs[i].points.length; j++ ) {
//           vertex( blobs[i].points[j].x, blobs[i].points[j].y+80 );
//       }
//       endShape(CLOSE);
//   }     
//
//   opencv.restore();

 println("tbuffer: " +Tbuffer);
 //opencv.ROI(null);
//opencv.ROI( 0, 0, width/2, height);
 stroke(0,0);
}
if(tt<255 && ttFlag==1){
  tt=tt+15;}
if(tt>0 && ttFlag==0){
  tt=tt-15;}
  if(tt==0){ttFlag=1;
tt=15;}
  if(tt==255){ttFlag=0;
tt=240;}
/*
tint(255,60);
 liveCam=cam.get(0,80,width,height-80);
 image(liveCam,0,160);
//noTint();*/
//set(0,80,liveCam,40);*/
//image(cam,0,80);
   stroke(200,0,0);
   noFill();
   
    rect(faceX, faceY+80, faceW, faceHeight);
    noStroke();
}


void send(){
 

 // StringXY = loadStrings(StringXYurl);
  try{
   TextSizeFlag=0;
  println("actualy in send");
   img  = new ImageToWeb(this);
 //   img.save("http://getimg2.eu01.aws.af.cm/get/ron/radio",true);
  // saveStrings("http://getimg2.eu01.aws.af.cm/get/ron/radio",new String[] {});
      img.post("img",EmotionImgSend,"jpg-test",true,img.getBytes(g));
      println("after post");

   getImageEmotion = loadStrings(EmotionImgRecieve); 
   if (getImageEmotion==null){
     getImageEmotion= new String[1];
     getImageEmotion[0]="neutral";
   println("Emotions :"+getImageEmotion[0]);
   }
// link ("http://getimg2.eu01.aws.af.cm/set/ron/"+getImageEmotion[0]);

 tweetResult = loadStrings(tweeterGetUrl+getImageEmotion[0]);
 if(tweetResult == null ){
   tweetResult = new String[1];
   tweetResult[0]="empty";
   println("tweeter result: " +tweetResult[0]);
 }
   String[] listEmotions = split(getImageEmotion[0], ',');
   println("listEmotions "+ listEmotions[0]);
   String[] sadnessList = split(listEmotions[1], ':');
   String[] disgustList = split(listEmotions[2], ':');
   String[] angerList = split(listEmotions[3], ':');
   String[] surpriseList = split(listEmotions[4], ':');
   String[] fearList = split(listEmotions[5], ':');
   String[] happinessList = split(listEmotions[6], ':');
   
   println("sadnessList: "+sadnessList[0]); 
   
   float sadness = float(sadnessList[1]);
   float disgust = float(disgustList[1]);
   float anger = float(angerList[1]);
   float surprise = float(surpriseList[1]);
   float fear = float(fearList[1]);
   float happiness = float(happinessList[1]);
   
 if(sadness>disgust && sadness>anger && sadness>surprise && sadness>fear && sadness>happiness ){
      bigEmotionValue[0] = "sadness" /*"Sadness"*/ ;
       bigEmotionValue[1] = ":(";
       bigEmotionValue[2] = "im sorry, be well";
       bigEmotionValue[3] = "sad";
     }
 if(disgust>sadness && disgust>anger && disgust>surprise && disgust>fear && disgust>happiness ){
     
       bigEmotionValue[0] =  "tired";
     //  bigEmotionValue[0] =  "to much thinking";
       bigEmotionValue[1] = "tired of this";
       bigEmotionValue[2] = "tired of watching";  
       bigEmotionValue[3] = "tired";
       }
 if(anger>sadness && anger>disgust && anger>surprise && anger>fear && anger>happiness ){
      bigEmotionValue[0] =  "Angry";
       bigEmotionValue[1] = "!!!!!!!!!!!!!";
       bigEmotionValue[2] = "it's yours, i don't deserve it!"; 
       bigEmotionValue[3] = "anger";
       }
 if(surprise>sadness && surprise>disgust && surprise>anger && surprise>fear && surprise>happiness ){
       bigEmotionValue[0] =  "Surprise";
       bigEmotionValue[1] = "curiosity is a good thing!";
       bigEmotionValue[2] = "wonderfull, thank you";
       bigEmotionValue[3] = "surprising";
        }
 if(fear>sadness && fear>disgust && fear>anger && fear>surprise && fear>happiness ){
     bigEmotionValue[0] =  "Afraid";
       bigEmotionValue[1] = "so deal with it";
       bigEmotionValue[2] = "afraid of being average";
       bigEmotionValue[3] = "fear";}
 if(happiness>sadness && happiness>disgust && happiness>anger && happiness>surprise && happiness>fear ){
       bigEmotionValue[0] =  "In love";
       bigEmotionValue[1] = "woo hoo";
       bigEmotionValue[2] = "love and light"; 
       bigEmotionValue[3] = "love";
       }
 println("emotion compar: "+ bigEmotionValue);
 
 float r=((map(anger,0,0.3,0,255)+map(happiness,0,0.3,0,255))/2);
 float g=((map(fear,0,0.3,0,255)+map(disgust,0,0.3,0,255))/2);
 float b=((map(sadness,0,0.3,0,255)+map(surprise,0,0.3,0,255))/2);
 println("rgb :"+r+","+g+","+b);
 //
 //
 int rr=int(r);
 int gg=int(g);
 int bb=int(b);
  println("rgb int :"+rr+","+gg+","+bb);
//println("sd");
 emotionColor=color(r,g,b);
 println("emotion color: " + emotionColor);
     randomIndex= int(random(bigEmotionValue.length-1  ));
bigEmotionValueRandom=bigEmotionValue[randomIndex];
     
//   emotionColor=
  // neutral:0.02,sadness:0.01,disgust:0.04,anger:0.01,surprise:0,fear:0.09,happiness:0.83
/*   int neutral= int(getImageEmotion[0])  ;
   int sadness= int(getImageEmotion[1])  ;
   int sadnessColor= int(getImageEmotion[2])  ;
   int disgust= int(getImageEmotion[3])  ;
   int disgustColor= int(getImageEmotion[4]) ; 
   int anger= int(getImageEmotion[5])  ;
   int angerColor= int(getImageEmotion[6])  ;
   int surprise= int(getImageEmotion[7]) ; 
   int surpriseColor= int(getImageEmotion[8]);  
   int fear= int(getImageEmotion[9])  ;
   int fearColor= int(getImageEmotion[10])  ;
   int happiness= int(getImageEmotion[11]) ; 
   int happinessColor= int(getImageEmotion[12]);  */
 
 //  tweetResult=""
 img=null;
 
// saveStrings("http://getimg2.eu01.aws.af.cm/set/radio/",bigEmotionValue);
 //youTube = loadStrings("http://getimg2.eu01.aws.af.cm/set/radio/"+bigEmotionValue);
   link("http://getimg2.eu01.aws.af.cm/set/radio/"+bigEmotionValue[3],"_same");
   //youTubeBytes  = loadBytes("http://getimg2.eu01.aws.af.cm/set/radio/"+bigEmotionValue[3]);

  }

  catch (Exception e){
      println("Exception");
    } 
  // translateEmotionGoeth();
 
}
```

## Links to External Libraries
 NOTE: You can also use this space to link to external libraries or Github repositories you used on your project.

## Images & Videos
NOTE: For additional images you can either use a relative link to an image on this repo or an absolute link to an externally hosted image.

![Example Image](project_images/cover.jpg?raw=true "Example Image")

![Example Image](http://ronerlich.com/wp-content/uploads/2013/06/04308467-1024x680.jpg "Example Image1")
![Example Image](http://ronerlich.com/wp-content/uploads/2013/06/04308374.jpg "Example Image1")
![Example Image](http://ronerlich.com/wp-content/uploads/2013/06/04308363.jpg "Example Image1")
![Example Image](http://ronerlich.com/wp-content/uploads/2013/06/04308380.jpg "Example Image1")

http://vimeo.com/68919739
http://vimeo.com/84759062
http://vimeo.com/79904254

