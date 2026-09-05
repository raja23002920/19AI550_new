# Ex.No: 10  Implementation of 2D space shooter game

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
