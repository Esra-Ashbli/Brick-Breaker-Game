# Brick-Breaker-Game
Brick Breaker game consists of bricks aligned at the top of the screen. The player is represented as a tiny ball that is placed on a small platform at the bottom of the screen. The platform can be moved around from left to right on the screen with the help of arrow keys on the keyboard. The player uses the platform to keep the ball running. The goal is to break the bricks without missing the ball with your platform.


##BB.java

```java

import javax.swing.JFrame;
public class BB {
    public static void main(String[] args) {
     // create the JFrame
        JFrame obj = new JFrame();
     // Gameplay gameplay = new Gameplay();
        obj.setBounds(10,10,700,600);
        obj.setTitle("Breakout Ball");
        obj.setResizable(false);
        obj.setVisible(true);
        obj.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
     // obj.add(gameplay);
    }
    
}


```





##Gameplay.java

```java

import java.awt.Color;
import java.awt.Graphics;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import javax.swing.Timer;
import javax.swing.JPanel;
public class Gameplay extends JPanel implements KeyListener, ActionListener{
    private boolean play = false;
    private int score = 0;
    private int totalBricks = 21;
    private Timer timer;
    private int delay = 8;
    
    private int playerX = 310;
    
    private int ballposX = 120;
    private int ballposY = 350;
    private int ballXdir = -1;
    private int ballYdir = -2;
    
    //constructor
   public Gameplay(){
       addKeyListener(this);
       setFocusable(true);
       setFocusTraversalKeysEnabled(false);
       timer = new Timer(delay, this);
       timer.start();
       
       
   } 
    //function
   public void paint(Graphics g){
       //background
       g.setColor(Color.black);
       g.fillRect(1, 1, 692, 692);//((WIDTH, delay, WIDTH, HEIGHT))
       
       //borders
       g.setColor(Color.yellow);
       g.fillRect(0, 0, 3, 592);
       g.fillRect(0, 0, 692, 3);
       g.fillRect(691, 0, 3, 592);
       
       //the paddle
       g.setColor(Color.GREEN);
       g.fillRect(playerX, 550, 100, 8);
       
       //the ball
       g.setColor(Color.yellow);
       g.fillRect(ballposX, ballposY, 20, 20);
   
   }
   
    @Override
    public void keyTyped(KeyEvent e) {}

    @Override
    public void keyPressed(KeyEvent e) {
        if(e.getKeyCode() == KeyEvent.VK_RIGHT){
           if(playerX >=600) {
               playerX = 600;
           } else{
               moveRight();
           }
        }
        if(e.getKeyCode() == KeyEvent.VK_LEFT){
            if(playerX < 10) {
               playerX = 10;
           } else{
               moveLeft();
           }
        }
    }
    
    public void moveRight(){
        play = true;
        playerX+=20;
    }
    
    public void moveLeft(){
        play = true;
        playerX-=20;
    }

    @Override
    public void keyReleased(KeyEvent e) {}

    @Override
    public void actionPerformed(ActionEvent e) {}

        
}



```