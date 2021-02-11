## Levien tanh approximation
license: Public Domain

by Raph Levien and adpated by William Light

C:
```c
inline static float
tanh_levien(const float x)
{
    const float x2 = x * x;
    const float x3 = x2 * x;
    const float x5 = x3 * x2;

    const float a = x
        + (0.16489087f * x3)
        + (0.00985468f * x5);

    return a / sqrtf(1.f + (a * a));
}
```
