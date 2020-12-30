笔记：
scale 控制方向

```c#
float facedirection = Input.GetAxisRaw("Horizontal"); //直接获得  -1，0，1  获取整数
transform.localScale = new Vector3(facedirection, 1, 1);// 设置方向
```



保证不同帧率正常
Update 函数改为 FixedUpdate() 函数
rb.velocity = new Vector2(horizontalMove * speed * Time.deltaTime, rb.velocity.y); //速度乘以一个时间参数

跳跃
Input.GetButtonDown("Jump")  // 获取跳跃按键
rb.velocity = new Vector2(rb.velocity.x, jumpforce * Time.deltaTime); //改变y轴方向
Rigidbody2D 中的GravityScale 参数同样可以调整跳跃力度





### Animation

笔记：

添加文件夹 Assets -> Animation -> Player    create AnimationController
window -> Animation -> Animation 打开动画面板
Smaples 设置动画速率

window -> Animation -> Animator
parameters 设置变化参数

修改变化箭头
去掉  Has Exit Time
Transition Duration 设置成 0

将Player Animation  加到 Player
编辑变化代码
public Animator anim;
anim.SetFloat("running", Mathf.Abs(facedirection));



### Cinemachine



### Item

New Sprite

Cherry的Pixels Per Unit 设置成16



Layer 越在下面越在前面显示

##### 为sprite创建animator

1. add component -> animator