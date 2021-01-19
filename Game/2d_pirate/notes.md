导入素材

切割素材

设置素材的参数



#### rigidbody 2d



mass 改变质量

桌子质量更大一些

`Collision Detection ` 设置为 continuous



Constraints

锁定旋转Z轴



Update里面获得输入



#### Prefabs

更改后要overwrites



人物左右翻转的两种方法

1. Rotation Y 180
2. Scale X -1

#### Update() 和 FixedUpdate()

`Update()` 每个frame执行一次，设备不同导致帧数不同。

可用来检测键盘或手柄输入

`FixedUpdate()` 固定次数大约每秒50，使用物理的方法（函数）放到这里面





#### 跳跃

```c#
[Header("Ground Check")]
public Transform groundCheck; // 放那个检测碰撞的Gizmos Object
public float checkRadius;	// 覆盖半径， 可稍后设置
public LayerMask groundLayer; // 哪个layer， 之前地面都放在了Ground layer


[Header("States Check")]
public bool isGround;
public bool canJump;
```

单独创建一个GameObject 用来检测与地面的碰撞

覆盖住脚部



