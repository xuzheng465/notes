# Boost



## Onion Design



<img src="/Users/xuzheng/Projects/notes/Game/U_game_3d/boost/notes.assets/OnionDesign.png" alt="OnionDesign" style="width:700px;" />



### Core Feature

1. Movement / flying

2. Collision
3. Fuel mechnic



### Use Time.deltaTime



#### Frame-rate Independence

* The time each frame takes can vary wildly.
* `Time.deltaTime` tells you the last frame time.
* This is a good predictor of the current frame time.
* Longer frame time lead to more movement.



####  Exposing Member Variables

| Modifier         | ChangeInInspector | ChangeFromOtherScr |
| ---------------- | ----------------- | ------------------ |
| [SerializeField] | Yes               | No                 |
| public           | Yes               | Yes                |



#### Playing audio

```c#
[SerializeField] AudioClip mainEngine;
AudioSource audioSource;
audioSource.PlayOneShot(mainEngine);
```

