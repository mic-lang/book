# 深さ修飾子

これまで、この章では、ある一種類のメモリ管理手法で確保されたメモリアドレスを参照するポインタ型しか見てきませんでした。しかしながら、こんにちの主要なシステムプログラミング言語では、通常3種類のメモリ管理手法を用意しています。
1つ目はスタック領域を用いてメモリ管理する方法。今まで紹介したのはすべてこの方法でした。
2つ目はヒープ領域を用いてメモリ管理する方法。Micは標準メモリアロケータにmimallocを用いているため、mimallocの`mi_heap_malloc`、`mi_free`関数を通じて行います。
3つ目は静的領域を用いてメモリ管理する方法。グローバル領域で定義された変数は、この方式で管理されます。

Micでは、この3つそれぞれで確保されたポインタを扱う型はすべて異なります。それぞれどのように型が表現されるのか見ていきましょう。

## スタック領域のポインタ型

スタック領域で定義された変数のポインタ型は、これまで通り、深さ識別子を用いて次のように表します。
また、深さ修飾子`auto`を深さ識別子の後ろにおいて、明示的にスタック領域に確保された、自動変数であることも強調することができますが、スタック領域のポインタ型の場合は深さ識別子のみだけでもいいので、通常は`auto`は省かれます。

```c
using p {
    int x, y = 0;
    int p* p1 = &x;
    int p auto* p2 = &y;
}
```

## ヒープ領域のポインタ型

ヒープ領域で定義された変数のポインタ型は、深さ識別子に加え、深さ修飾子`dyn`を用いて次のように表します。
このとき、`dyn`を省いてしまうと、スタック領域のポインタ型になってしまうので、必ず、`dyn`を深さ識別子の後ろに置きます。

```c
using p {
    int p dyn* p1;
}
```

また、関数外で、ヒープ領域のポインタ型を定義したいときは、深さ識別子は省きます。
```c
// Global Scope
int dyn* p;
```

## 静的領域のポインタ型

静的領域で定義された変数のポインタ型は、これまでと違ってやや特殊です。
静的領域は、グローバル領域に変数を定義しただけでこのメモリ管理手法になるため、実は、深さ識別子も深さ修飾子も必要ありません。次のようにするだけで静的領域のポインタ型を定義できます。また、関数内で、静的領域の変数を確保する場合は、通常のC言語と同じように、`static`キーワードを使います。

```c
int* p;

void func() {
    static int* p;
}
```

# つぎに

さて、もともとのC言語には`free`関数もあります。つまり、参照先の変数も早期解放される可能性があるのです。その場合は、どのようにメモリ安全性を達成するのか次の章で見ていきましょう。いわゆる所有権システムの登場です。
ただ、Micの所有権システムはこれまであった言語のそれとは違い、比較的簡単です。