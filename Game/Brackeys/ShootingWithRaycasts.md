[Resource](https://www.youtube.com/watch?v=THnivyG0Mvo)



```c#
// Gun.cs 
using UnityEngine;
public class Gun : MonoBehaviour 
{
	public float damage = 10f;
  public float frange = 100f;
  public float impactForce = 30f;
  public float fireRate = 15f; // 调节开枪频率
  
  public Camera fpsCam;
  public ParticleSystem muzzleFlash;
  public GameObject impactEffect;
  
  void Update () 
  {
  	if (Input.GetButtonDown("Fire1") && Time.time >= nextTimeToFire) 
    {
      nextTimeToFire = Time.time + 1f / fireRate;
      Shoot();
    }
  }
  
  void Shoot()
  {
    RaycastHit hit;
    if (Physics.Raycast(fpsCam.transform.position,
                       fpsCam.transform.forward,
                       out hit,
                       range))
    {
      Debug.Log(hit.transform.name);
      
      Target target = hit.transform.GetComponent<Target>();
      if (target != null)
      {
        target.TakeDamage(damage);
      }
      
      if (hit.rigidbody != null)
      {
        hit.rigidbody.AddForce(-hit.normal * impactForce)
      }
      
      GameObject impactGO = Instantiate(impactEffect, hit.point, Quaternion.LookRotation(hit.normal));
      Destroy(impactGO, 2f);
    }
  }
}
```



```c#
using UnityEngine;

public class Target : MonoBehaviour 
{
	public float health = 50f;
  public void TakeDamage ( float amount )
  {
    health -= amount;
    if (health <= 0f)
    {
      Die();
    }
  }
  void Die() 
  {
    Destroy(gameObject); // TODO 炸裂效果，
  }
}
```





基本步骤：

1. 在脚本中打开一个public ComponentType component
2. 可以在interface中添加相关组件。



问题：

1. Raycast的详细用法



TODO

1. 物体的炸裂效果
2. 子弹实体
3. 