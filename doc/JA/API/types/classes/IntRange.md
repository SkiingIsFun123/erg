# IntRange L, R

`L..R`のクラス。

```erg
IntRange L, R: Int == L..R
```

## methods

* .`_+_`: Self(L1, R1), Self(L2, R2) -> Self(L1+L2, R1+R2)

通常の加算。`Int`や`Nat`の加算はそれぞれのクラスで定義されていると見せかけて、ここで定義されている。

```erg
0..10 + 1..12 == 1..22
Int + 0..10 == _..|Int|_ + 0..10 == _..|Int|_ == Int
Nat + Nat == 0.._ + 0.._ == 0.._ == Nat
```
