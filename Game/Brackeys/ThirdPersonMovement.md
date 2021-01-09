https://www.youtube.com/watch?v=4HpC--2iowE

1. 创建一个cylinder

2. 弄个小头， z方向一致

3. 现在cylinder下面创建一个empty gameobject， reset，重命名为`Third Person Player`

4. TPP add `Character Controller` component

5. 在把cylinder放到 `Third Person Player` 下面

6.  Cinemachine -> Create FreeLook Camera

7. Drag Player to `Follow` and `Look At` in the `CinemachineFreeLook`

8. Orbits

   1. | Type      | Height | Radius |
      | --------- | ------ | ------ |
      | TopRig    | 14     | 12     |
      | MiddleRig | 5      | 18     |
      | BottomRig | -1.6   | 12     |

9. Heading ->Definition : Target Forward

10. drag Character Controller to controller in the Scripts

```c#
// Third Person Movement
using UnityEngine;

public class ThirdPersonMovement : MonoBehaviour
{
  public CharacterController controller;
  public Transform cam;
  
  public float speed = 6f;
  public float turnSmoothTime = 0.1f;
  float turnSmoothVelocity;
  
  void Update()
  {
    float horizontal = Input.GetAxisRaw("Horizontal");
    float vertical = Input.GetAxisRaw("Vertical");
    Vector3 direction = new Vector3(horizontal, 0f, vertical).normalized;
    
    if (direction.magnitude >= 0.1f)
    {
      // 朝着相机位置移动 rotate
      
      //1.  figure out how much we should rotate our player on the y-axis to point in that direction
      // Atan2
      float targetAngle = Mathf.Atan2(direction.x, direction.z) * Mathf.Rad2Deg + cam.eulerAngles.y;
      
      float angle = Mathf.SmoothDampAngle(transform.eulerAngles.y,
                                         targetAngle,
                                         ref turnSmoothVelocity,
                                         turnSmoothTime);
      transform.rotation = Quaternion.Euler(0f, angle, 0f);
      // 2. follow camera

      Vector3 moveDir = Quaternion.Euler(0f, targetAngle, 0f) * Vector3.forward;
      controller.Move(moveDir.normalized * speed * Time.deltaTime);
    }
  }
}
```



防止视角进入建筑物

Third Person Camera -> Extension -> Cinemachine Collider



问题

1. GetAxisRaw 与GetAxis的不同
2. normalized。 视频说当同时按两个键时不会加速

3. FixUpdate() 与 Update（）的不同

4. Quaternion 用法