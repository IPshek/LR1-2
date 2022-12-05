Лабораторная работа № 5
Тема: разработка игрового проекта Endless Runner
Цель: разработать игровой проект Endless Runner
Ход работы
1.	Выполнение работы

Создание игры Endless Runner

Создал в Assets папки Scripts, Sprites, Prefabs

Импортировал спрайты в папку Sprites
 
 ![image](https://user-images.githubusercontent.com/118774422/205603524-d0f2c760-2102-49d4-93ba-a936a08fe31f.png) 
 
Рис. 5.1 Assets Sprites

Создал объект Player с дочерним спрайтом, настроил его. Добавил RigidBody2D.

Создал скрипт Player

Листинг Player.cs

using UnityEngine;

public class Player : MonoBehaviour
{
    public float playerSpeed;
    private Rigidbody2D rb;
    private Vector2 playerDirection;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        float directionY = Input.GetAxisRaw("Vertical");
        playerDirection = new Vector2(0, directionY).normalized;
    }

    void FixedUpdate()
    {
        rb.velocity = new Vector2(0, playerDirection.y * playerSpeed);
    }
}


Создал скрипт CameraMovement

Листинг CameraMovement.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraMovement : MonoBehaviour
{
    public float cameraSpeed;

    void Update()
    {
        transform.position += new Vector3(cameraSpeed * Time.deltaTime, 0, 0);
    }
}

Создал пустой объект Game Manager

Добавляем скрипт CameraMovement к пустому объекту Game Manager

Настроил Background

 ![image](https://user-images.githubusercontent.com/118774422/205606503-d3a1c0e9-6275-41f1-bff6-ec0cf4ceecde.png)

Рис. 5.2 Иерархия объектов

Создаем скрипт LoopingBackground

Листинг LoopingBackground.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class LoopingBackground : MonoBehaviour
{
    public float backgroundSpeed;
    public Renderer backgroundRenderer;

    void Update()
    {
        backgroundRenderer.material.mainTextureOffset += new Vector2(backgroundSpeed * Time.deltaTime, 0f);
    }
}

Создаем скрипт SpawnObstacles

Листинг SpawnObstacles

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class SpawnObstacles : MonoBehaviour
{
    public GameObject obstacle;
    public float maxX;
    public float minX;
    public float maxY;
    public float minY;
    public float timeBetweenSpawn;

    private float spawnTime;

    void Update()
    {
        if (Time.time > spawnTime)
        {
            Spawn();
            spawnTime = Time.time + timeBetweenSpawn;
        }
    }

    void Spawn()
    {
        float randomX = Random.Range(minX, maxX);
        float randomY = Random.Range(minY, maxY);

        Instantiate(obstacle, transform.position + new Vector3(randomX, randomY, 0), transform.rotation);
    }
}

Создал пустой объект Spawn Point, настроил. Сделал объект Spawn Point дочерним от GameManager

Создал и настроил Borders
 
  ![image](https://user-images.githubusercontent.com/118774422/205606576-07993f36-1974-41d5-b905-b7fabe83e6b7.png)

Рис. 5.3 Сброс компонента Transform

Создал скрипт Obstacle
Листинг Obstacle.cs
using UnityEngine;

public class Obstacle : MonoBehaviour
{
    private GameObject player;

    private void Start()
    {
        player = GameObject.FindGameObjectWithTag("Player");
    }

    private void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision.tag == "Border")
        {
            Destroy(this.gameObject);
        }

        else if(collision.tag == "Player")
        {
            Destroy(player.gameObject);
        }
    }
}

 ![image](https://user-images.githubusercontent.com/118774422/205606610-f822fa7a-f98e-472f-9c27-896893a4f195.png)

Рис. 5.4 Настройки компонента Canvas Scaler


Создал объект Wooden Panel

Добавил Restart Button

Создал скрипт GameOver

Листинг скрипта GameOver.cs

using UnityEngine;
using UnityEngine.SceneManagement;

public class GameOver : MonoBehaviour
{
    public GameObject gameOverPanel;

    void Update()
    {
        if (GameObject.FindGameObjectWithTag("Player") == null)
        {
            gameOverPanel.SetActive(true);
        }
    }

    public void Restart()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }
}

Добавляем компонент скрипта GameOver объекту GameManager и передаем обьект Game Over Panel в поле GameOverPanel

Создал объект Score Text, настроил его

  ![image](https://user-images.githubusercontent.com/118774422/205606673-150f1c8e-6caf-4bb0-9982-cd35ddcf5350.png)

Рис. 5.5 Настройки объекта Score Text

Создал скрипт ScoreManager

Листинг ScoreManager.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class ScoreManager : MonoBehaviour
{
    public Text scoreText;
    private float score;

    // Update is called once per frame
    void Update()
    {
        if (GameObject.FindGameObjectWithTag("Player") != null)
        {
            score += 1 * Time.deltaTime;
            scoreText.text = ((int)score).ToString();
        }
    }
}

  ![image](https://user-images.githubusercontent.com/118774422/205606708-5a0d1a8d-5c4d-4743-bc80-c16d48c71a12.png)

Рис. 5.6 Иерархия объектов

Создал билд
 
  ![image](https://user-images.githubusercontent.com/118774422/205606731-f47a10d4-192f-4f70-a41c-7e039b0f46bc.png)

Рис. 5.7 Билд

2.	Вывод

Был разработан игровой проект Endless Runner.
