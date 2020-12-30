```c#
public class SelectDifficulty:MonoBehaviour
{
  public enum LevelSelector
  {
    Easy,
    Normal,
    Hard,
    Expert
  }
  public LevelSelector curLevel;
}
```



####  Enum for AI

```c#
// unity scipt ...
public enum EnemyState
{
  Patroling,
  Attacking,
  Chasing,
  Death
}
public EnemyState curState;
```

