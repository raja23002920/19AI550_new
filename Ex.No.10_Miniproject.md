# Ex.No: 10  Implementation of 2D space shooter game
### DATE : 09/05/2025                                                                           
### REGISTER NUMBER : 212223240134
### AIM: 
To develop a 2D space shooter game in Unity where the rocket moves only up and down, and enemies spawn independently.
### Algorithm:

1. **Start the game** and load the main scene.
2. **Initialize player rocket** at a fixed X-position on the left side of the screen.
3. **Lock rocket movement** to a straight vertical axis so it can move only **up and down**.
4. **Read player input** (W/S or Up/Down keys) to control vertical movement.
5. **Update rocket position** every frame along the Y-axis only (no rotation, no curve).
6. **Initialize enemy spawner** object on the right side of the screen.
7. **Start enemy spawn timer** to create one enemy at a time with independent spacing.
8. **Spawn each enemy** at a random Y-position within half of the screen height.
9. **Move the enemy** horizontally from right to left at a constant speed.
10. **Check enemy boundary** and destroy the enemy if it goes off-screen.
11. **Attach enemy shooting script** that fires bullets toward the left side at fixed intervals.
12. **Spawn enemy bullets** using a prefab and give them a constant leftward speed.
13. **Check collision** between player's rocket & enemy bullets → trigger game over.
14. **Check collision** between rocket bullets & enemies → destroy enemy.
15. **Repeat spawning and movement** until the game ends or player loses all lives.


### Program:
#### 🧩 1️⃣ PlayerController.cs
```cs
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [Header("Movement Settings")]
    public float moveSpeed = 5f;
    public float verticalLimit = 4f;

    [Header("Shooting Settings")]
    public GameObject bulletPrefab;
    public Transform firePoint;
    public float bulletSpeed = 10f;

    void Update()
    {
        HandleMovement();
        HandleShooting();
    }

    void HandleMovement()
    {
        // Move Up/Down only
        float moveY = 0f;
        if (Input.GetKey(KeyCode.UpArrow))
            moveY = 1f;
        else if (Input.GetKey(KeyCode.DownArrow))
            moveY = -1f;

        Vector3 newPos = transform.position + new Vector3(0, moveY * moveSpeed * Time.deltaTime, 0);

        // Clamp to screen limits
        newPos.y = Mathf.Clamp(newPos.y, -verticalLimit, verticalLimit);
        transform.position = newPos;
    }

    void HandleShooting()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            if (bulletPrefab && firePoint)
            {
                GameObject bullet = Instantiate(bulletPrefab, firePoint.position, Quaternion.identity);
                Rigidbody2D rb = bullet.GetComponent<Rigidbody2D>();
                rb.velocity = Vector2.right * bulletSpeed;
            }
            else
            {
                Debug.LogWarning("PlayerController: Missing bulletPrefab or firePoint!");
            }
        }
    }
}

```
#### 🧩 2️⃣ PlayerBullet.cs
```cs
using UnityEngine;

public class PlayerBullet : MonoBehaviour
{
    public float lifeTime = 3f;

    void Start()
    {
        Destroy(gameObject, lifeTime);
    }

    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Enemy"))
        {
            Destroy(other.gameObject);
            Destroy(gameObject);
        }
    }
}

```
#### 🧩 3️⃣ EnemyGenerator.cs
```cs
using UnityEngine;

public class EnemyGenerator : MonoBehaviour
{
    [Header("Enemy Spawning Settings")]
    public GameObject alien;
    public float timeBetweenEnemies = 2f;
    public float secondsBeforeFirstEnemyAppears = 0f;

    [Header("Spawn Area Settings")]
    public float minY = 1.5f;
    public float maxY = 3.5f;
    public float spawnX = 10f;

    private float nextSpawnTime;
    private float lastY;

    void Start()
    {
        nextSpawnTime = Time.time + secondsBeforeFirstEnemyAppears;
        lastY = Random.Range(minY, maxY);
    }

    void Update()
    {
        if (Time.time >= nextSpawnTime)
        {
            SpawnEnemy();
            nextSpawnTime = Time.time + timeBetweenEnemies;
        }
    }

    void SpawnEnemy()
    {
        if (alien == null)
        {
            Debug.LogError("EnemyGenerator: Alien prefab not assigned!");
            return;
        }

        float randomY = Random.Range(minY, maxY);
        while (Mathf.Abs(randomY - lastY) < 0.8f)
            randomY = Random.Range(minY, maxY);
        lastY = randomY;

        Vector3 spawnPos = new Vector3(spawnX, randomY, 0f);
        Instantiate(alien, spawnPos, Quaternion.identity);
    }
}

```
#### 🧩 4️⃣ EnemyMovement.cs
```cs
using UnityEngine;

public class EnemyMovement : MonoBehaviour
{
    public float speed = 3f;

    void Update()
    {
        transform.Translate(Vector2.left * speed * Time.deltaTime);

        // Destroy off-screen enemies
        if (transform.position.x < -10f)
            Destroy(gameObject);
    }
}

```
#### 🧩 5️⃣ EnemyShooter.cs
```cs
using UnityEngine;

public class EnemyShooter : MonoBehaviour
{
    public GameObject bulletPrefab;
    public Transform firePoint;
    public float bulletSpeed = 6f;
    public float shootInterval = 2f;

    private float nextShotTime;

    void Update()
    {
        if (Time.time >= nextShotTime)
        {
            Shoot();
            nextShotTime = Time.time + shootInterval;
        }
    }

    void Shoot()
    {
        if (bulletPrefab == null || firePoint == null)
            return;

        GameObject bullet = Instantiate(bulletPrefab, firePoint.position, Quaternion.identity);
        Rigidbody2D rb = bullet.GetComponent<Rigidbody2D>();
        rb.velocity = Vector2.left * bulletSpeed;
    }
}

```
#### 🧩 6️⃣ EnemyBullet.cs
```cs
using UnityEngine;

public class EnemyBullet : MonoBehaviour
{
    public float lifeTime = 3f;

    void Start()
    {
        Destroy(gameObject, lifeTime);
    }

    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
        {
            Destroy(other.gameObject);
            Destroy(gameObject);
        }
    }
}

```
### Output:
<img width="1919" height="998" alt="image" src="https://github.com/user-attachments/assets/f011ac8c-6643-4db3-a32a-af32ee802b54" />


### Result:
The 2D shooter game was successfully created with smooth rocket movement, separate enemy spawning, and working shooting mechanics.
