# PINGPONGGAME

## AIM:



## ALGORITHM:
### Step 1:
Create a new scene and save. Then right click hierarchy and click ->create empty (name as Game manager).
### Step 2:
In the Asserts create new C# script and (name as GameManager) two more scripts (named as Ball and Paddle).
### Step 3:
Right click creat-> 2D->spirates-> circle then create->2D->spirates->square. Drag these from asserts to hierarchy and change the position and scale in the inspector.
### Step 4:
For both the sprites->Add Components-> BoxCollider 2D (Tick in IsTigger) and Rigidbody 2D(Change the body type to Kinematics )
### Step 5:
For both the sprites -> Add the tag. In inspector-> Tag-> Click AddTag and create the tag with name as(Paddle) and make the tag as Paddle so we can whether ball is hitting paddle or somewhere else in script. Similarly do for Ball.
### Step 6:
Drag the ball and paddle from hierarchy to the Asserts-> Sprites to create prefabs and reset the position of Paddle to (0,0,0) and delete ball and paddle from hierarchy.
### Step 7:
Click the Game manager in hierarchy and drag the GameManager script to Inspector and open the script
Public Ball ball;
Public Paddle paddle;
Check in the unity whether the variables are displaying in the unity.
### Step 8:
Now Click the ball game object which we deleted from hierarchy and incorporate the ball script to it, similarly,click paddle game object and incorporate the paddle script to it. Then drag the paddle variable and ball variable into game manager.
Type the code th GameManager and Paddle
### Step 9:
Edit-> Project settings-> Input -> Axes (2) -> Horizontal (name as PaddleLeft) and Vertical (name as PaddleRight)
### Step 10:
In PaddleRight (Negative button - down and positive buttom - up) and paddleLeft(Negative button - s and positive buttom - w)
 After completing, to move the ball, in the ball inspector give the value for speed
 
 ## PROGRAMM:
 ## GAME MANAGER.CS:
 ```
 using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public Ball Ball;
    public Paddle Paddle;
    public static Vector2 bottomLeft;
    public static Vector2 topRight;
    // Start is called before the first frame update
    void Start()
    {
        bottomLeft = Camera.main.ScreenToWorldPoint(new Vector2(0, 0));
        topRight = Camera.main.ScreenToWorldPoint(new Vector2(Screen.width, Screen.height));
        Instantiate(Ball);

        Paddle Paddle1 = Instantiate(Paddle) as Paddle;
        Paddle Paddle2 = Instantiate(Paddle) as Paddle;

        Paddle1.Init(true);
        Paddle2.Init(false);
    }

    // Update is called once per frame
    void Update()
    {

    }
}


 ```
 ## BALL.CS:
 ```
 using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Ball : MonoBehaviour
{
    [SerializeField]
    float speed;
    float radius;
    Vector2 direction;
    void Start()
    {
        direction = Vector2.one.normalized;
        radius = transform.localScale.x / 2;
    }
    // Update is called once per frame
    void Update()
    {
        transform.Translate(direction * speed * Time.deltaTime);
        if (transform.position.y < GameManager.bottomLeft.y + radius && direction.y < 0)
        {
            direction.y = -direction.y;
        }
        if (transform.position.y > GameManager.topRight.y - radius && direction.y > 0)
        {
            direction.y = -direction.y;
        }

        //Game Over
        if (transform.position.x < GameManager.bottomLeft.x + radius && direction.x < 0)
        {
            Debug.Log("Right player Wins");
            Time.timeScale = 0;
        }
        if (transform.position.x > GameManager.topRight.x - radius && direction.x > 0)
        {
            Debug.Log("Left player Wins");
            Time.timeScale = 0;
        }
    }
    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.tag == "Paddle")
        {
            bool isRight = other.GetComponent<Paddle>().isRight;
            if (isRight == true && direction.x > 0)
            {
                direction.x = -direction.x;
            }
            if (isRight == false && direction.x < 0)
            {
                direction.x = -direction.x;
            }
        }
    }
}

 ```
 ## PADDLE.CS:
 ```
 using System.Collections;
using System.Collections.Generic;
using System.Threading;
using UnityEngine;

public class Paddle : MonoBehaviour
{
    [SerializeField]
    float speed;
    float height;
    string input;
    public bool isRight;
    // Start is called before the first frame update
    void Start()
    {
        height = transform.localScale.y;
        speed = 6f;

    }
    public void Init(bool isRightPaddle)
    {
        isRight = isRightPaddle;
        Vector2 pos = Vector2.zero;
        if (isRightPaddle)
        {
            pos = new Vector2(GameManager.topRight.x, 0);
            pos -= Vector2.right * transform.localScale.x;
            input = "PaddleRight";
        }
        else
        {
            pos = new Vector2(GameManager.bottomLeft.x, 0);
            pos += Vector2.right * transform.localScale.x;
            input = "PaddleLeft";
        }
        transform.position = pos;
        transform.name = input;
    }

    // Update is called once per frame
    void Update()
    {
        float move = Input.GetAxis(input) * Time.deltaTime * speed;
        if (transform.position.y < GameManager.bottomLeft.y + height / 2 && move < 0)
        {
            move = 0;
        }
        if (transform.position.y > GameManager.topRight.y - height / 2 && move > 0)
        {
            move = 0;
        }
        transform.Translate(move * Vector2.up);
    }
}
```

 
 ## OUTPUT:

 ## INTERFACE:
 ![output](o1.png)
 ## GAME OVER:
 ![output](o2.png)
 
 ## RESULT:
Thus, a ping pong game was developed using C# program in unity.
