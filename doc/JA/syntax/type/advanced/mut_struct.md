# 可変構造型

`T!`型は任意の`T`型オブジェクトを入れられて差し替え可能なボックス型であると説明した。

```erg
Particle! State: {"base", "excited"}! = Class(..., Impl: Phantom State)
Particle!.
    # このメソッドはStateを"base"から"excited"に遷移させる
    apply_electric_field!(ref! self("base" ~> "excited"), field: Vector) = ...
```

`T!`型は、データの差し替えは行えるが、その構造を変えることはできない。
より現実のプログラムの振舞いに近い言い方をすれば、(ヒープ上の)サイズを変更できない。このような型を、不変構造(可変)型と呼ぶ。

実は、不変構造型では表すことのできないデータ構造が存在する。
例えば、可変長配列である。`[T; N]!`型は任意の`[T; N]`であるオブジェクトを入れることができるが、`[T; N+1]`型オブジェクトなどに差し替えることはできない。

すなわち、長さを変えられないのである。長さを変えるためには、型自体の構造を変化させなくてはならない。

それを実現するのが可変構造(可変)型である。

```erg
v = [Str; !0].new()
v.push! "Hello"
v: [Str; !1]
```

可変構造型では可変化する型引数に`!`を付ける。上の場合は、`[Str; !0]`型を`[Str; !1]`型などに変更することができる。すなわち、長さを変更できる。
因みに、`[T; !N]`型は`MutLenArray(T, !N)`型の糖衣構文である。

可変構造型はもちろんユーザー定義も可能である。ただし、不変構造型とは構成法に関していくつか違いがあるので注意が必要である。

```erg
Nil T = Class(Impl: Phantom T)
List T, !0 = Inherit Nil T
List T, N: Nat! = Class {head = T; rest = List(T, !N-1)]}
List(T, !N).
    push! ref! self(N ~> N+1, ...) head: T =
        self.update! old -> Self.new {head; old}
```
