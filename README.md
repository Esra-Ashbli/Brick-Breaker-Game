# Brick-Breaker-Game
Brick Breaker game consists of bricks aligned at the top of the screen. The player is represented as a tiny ball that is placed on a small platform at the bottom of the screen. The platform can be moved around from left to right on the screen with the help of arrow keys on the keyboard. The player uses the platform to keep the ball running. The goal is to break the bricks without missing the ball with your platform.


# BB.java

```java

import javax.swing.JFrame;

public class BB {

    public static void main(String[] args) {

        //  Create an empty frame.
        JFrame obj = new JFrame(); 
        Gameplay gameplay = new Gameplay(); // The game will be shown in this frame.
        obj.setBounds(10,10,700,600); // Size of the frame.
        obj.setTitle("Brick Breaker Game "); // Title of the frame.
        obj.setResizable(false); //  The parameter is false then the user cannot re-size the frame.
        obj.setVisible(true); // This method makes the frame appear on the screen.
        obj.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); /* This method picks the action to perform when the "close" button of the frame is clicked.*/
        obj.add(gameplay); // Add the object 'gameplay' to the frame.
    }  
}


```
# Gameplay.java
```java



import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.Rectangle;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import javax.swing.Timer;
import javax.swing.JPanel;
public class Gameplay extends JPanel implements KeyListener, ActionListener{
    /*
        KeyListener in Java handles all events pertaining to any action with regards to keyboard.
        KeyListener to move the 'slider'.
        ActionListener in Java is a class that is responsible for handling all action events.
        ActionListener to move the 'ball'.
    */
    private boolean play = false; // Prevent the game to start by itself.
    private int score = 0;
    
    private int totalBricks = 21; // 3*7 Number of Bricks.
    
    private Timer timer; // setting the time of the ball to establish how fast it will move.
    private int delay = 0;  // the speed that will be given to the timer
    
    private int playerX = 310; // Starting position of our slider.
    
    private int ballposX = 120;
    private int ballposY = 350;
    private int ballXdir = -1;
    private int ballYdir = -2;
    
    private MapGenerator map; // To be able to use MapGenerator class.
    //constructor
   public Gameplay(){
       map = new MapGenerator(3, 7);
       addKeyListener(this); // to work with key listener
       setFocusable(true); // enables the component to gain focus once the Frame is displayed.
       setFocusTraversalKeysEnabled(false); /*decides whether or not focus traversal keys (TAB key, SHIFT+TAB, etc.) are allowed
       to be used when the current Component has focus.*/
       timer = new Timer(delay, this);
       timer.start();
       
       
   } 
    // g object will be used to draw the ball or the slider...
   public void paint(Graphics g){
       //Background
       g.setColor(Color.black);
       g.fillRect(1, 1, 692, 692); //(x, y, WIDTH, HEIGHT), x and y for its position
       
       //Drawing map for bricks
       map.drow((Graphics2D)g); //pass g object after type casting to 'Graphics2D'.
       
       //borders
       g.setColor(Color.yellow);
       
       //create three rectangular for the borders
       //We will NOT add a border at the BOTTOM, Because we want the BALL to FALL DOWN to end up the game.
       g.fillRect(0, 0, 3, 592);//LEFT
       g.fillRect(0, 0, 692, 3);//TOP
       g.fillRect(691, 0, 3, 592);//RIGHT
       
       //Scores
       g.setColor(Color.yellow);
       g.setFont(new Font("serif", Font.BOLD, 25));
       g.drawString(""+score, 590, 30);
       
       //The paddle
       g.setColor(Color.GREEN);
       g.fillRect(playerX, 550, 100, 8);//playerX is the starting position of the slider.
       
       //The ball
       g.setColor(Color.red);
       g.fillOval(ballposX, ballposY, 20, 20);
       
       //if the number of bricks are done, YOU WON!.
       if(totalBricks <=0){
            play=false;
            ballXdir=0;
            ballYdir=0;
            
            g.setColor(Color.red);
            g.setFont(new Font("serif", Font.BOLD, 30));
            g.drawString("YOU WON !, SCORE:"+score, 190, 300);
            
            g.setFont(new Font("serif", Font.BOLD, 20));
            g.drawString("Press 'Enter' to restart", 230, 350);
       }
       
       //if the ball went down, GAME OVER.
       if(ballposY > 570){
            play=false;
            ballXdir=0;
            ballYdir=0;
            
            g.setColor(Color.red);
            g.setFont(new Font("serif", Font.BOLD, 30));
            g.drawString("GAME OVER, SCORE:"+score, 190, 300);
            
            g.setFont(new Font("serif", Font.BOLD, 20));
            g.drawString("Press 'Enter' to restart", 230, 350);
       }
       //dispose(); Causes the JFrame window to be destroyed and cleaned up by the operating system.
       g.dispose();
   
   }//End paint
   
   //Methods in order to make Keylistener and Actionlistener work.
    @Override
    public void actionPerformed(ActionEvent e) {
        timer.start();
        
        //Method for moving the ball
        if(play){
            //For detecting the intersection between two objects "the paddle & the ball"
           if(new Rectangle(ballposX, ballposY, 20,20).intersects(new Rectangle(playerX, 550, 100, 8))){
               ballYdir = -ballYdir;
           }
           
           A:for(int i = 0; i< map.map.length; i++){ //first map is the object, the second map is array in the MapGenerator class.
                for(int j = 0; j< map.map[0].length; j++){
                   if(map.map[i][j]>0){//if the brick is there detect the intersection
                       int brickX = j*map.brickWidth + 80;
                       int brickY = i*map.brickHeight + 50;
                       int brickWidth = map.brickWidth;
                       int brickHeight = map.brickHeight;
                       
                       //create the rectangle around the brick.
                       Rectangle rect = new Rectangle(brickX, brickY, brickWidth, brickHeight);
                       Rectangle ballRect = new Rectangle(ballposX, ballposY, 20, 20);
                       Rectangle brickRect = rect;
                       
                       //Changing the value of the brick from 1 to 0 if the ball intersect it
                       if(ballRect.intersects(brickRect)){
                           map.setBrickValue(0, i, j);
                           totalBricks--;// decrease the number of bricks by 1
                           score += 5;// increase the user score by 5
                           
                           //for the left & right intersections
                           if(ballposX+19 <= brickRect.x || ballposX+1>= brickRect.x + brickRect.width){
                               ballXdir = -ballXdir;
                           }else { 
                                ballYdir = -ballYdir;
                           }
                           
                           break A; /*here we used break label because we need to take the compiler out of the outer loop, 
                           in case of the bricks are all end or the ball kept moving ...*/
               
                       }    
                   }
           
               }
        
           }   
            
           ballposX += ballXdir;
           ballposY += ballYdir;
           
           //For the left border
           if(ballposX < 0){
               ballXdir = -ballXdir;
           }
           
           //For the top border
           if(ballposY < 0){
               ballYdir = -ballYdir;
           }
           
           //For the right border
           if(ballposX > 670){
               ballXdir = -ballXdir;
           }
        }
        
        repaint();////from JComponent.java class
        /*it will recall paint method and draw everything again because when we are incrementing or
        decrementing playerX we need all the shapes to be drawn again*/
    }//End actionPerformed
    
   
    @Override
    public void keyPressed(KeyEvent e) {//inside this method we can do the detecting of the arrow
        if(e.getKeyCode() == KeyEvent.VK_RIGHT){// detecting the right key
           if(playerX >=600) {//Checking if the ball getting outside the border.
               playerX = 600;//keep it inside the border of the panel.
           } else{
               moveRight();
           }
        }
        if(e.getKeyCode() == KeyEvent.VK_LEFT){ 
            if(playerX < 10) {//Checking if the ball getting outside the border.
               playerX = 10;////keep it inside the border  of the panel.
           } else{
               moveLeft();
           }
        }
        
        //If the game end then player pressed Enter, the default values of the game should be restored.
        if(e.getKeyCode()== KeyEvent.VK_ENTER){
            if(!play){
                play = true;
                ballposX = 120;
                ballposY = 350;
                ballXdir = -1;
                ballYdir = -2;
                playerX =310;
                score =0;
                totalBricks =21;
                map= new MapGenerator(3,7);
                
                repaint();
            }
        }
    }//End keyPressed
    
    
    //If the user pressed right key, the ball moves to the right side 20px.
    public void moveRight(){
        play = true;
        playerX+=20;
    }
    
    //If the user pressed left key, the ball moves to the left side 20px.
    public void moveLeft(){
        play = true;
        playerX-=20;
    }
    
    //These methods We didn't use it, but it's necessary to include it in our code.
    @Override
    public void keyTyped(KeyEvent e) {}
    @Override
    public void keyReleased(KeyEvent e) {}

}

```

# MapGenerator.java
```java
 
import java.awt.BasicStroke;
import java.awt.Color;
import java.awt.Graphics2D;
public class MapGenerator {
    public int map[][];//in order to creat the bricks
    public int brickWidth;
    public int brickHeight;
    
    public MapGenerator(int row, int col){//constructor 
        //decides how many rows and columns are needed to create a specific number of bricks.
        map = new int[row][col];
        for(int i = 0; i< map.length; i++){
            for(int j = 0;j< map[0].length;j++){
                map[i][j]=1; // 1 meanes this brick have not intersected by the ball yet.
            }
        }
        
        brickWidth = 540/col;
        brickHeight = 150/row;        
    }
    public void drow(Graphics2D g){// Method to draw the bricks. 
        for(int i = 0; i< map.length; i++){
            for(int j = 0;j< map[0].length;j++){
                if(map[i][j]>0){
                    g.setColor(Color.white);
                    g.fillRect(j*brickWidth+80, i*brickHeight+50, brickWidth, brickHeight);
                    
                    //Drawing the borders around each break
                    g.setStroke(new BasicStroke(3));
                    g.setColor(Color.black);
                    g.drawRect(j * brickWidth + 80 , i * brickHeight + 50 , brickWidth, brickHeight);
                }
                
            }
        }
    }
    
    //Intersections between the ball and the bricks.
    public void setBrickValue(int value, int row, int col) {
        map[row][col] = value; 
    }   
}
 
```
