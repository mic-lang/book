# Micのユニークなポインタ型
この章では、Micのユニークなポインタ型について見ていきます。
Micでは名前付き複合文というMic独自の複合文で定義される、深さ識別子と呼ばれる識別子を用いて深さ付きポインタ型を表現します。
それらは、`int p*`、`int p* p*`といったあまり見慣れない形をしているかもしれません。ただ章を進めていけば、すぐにその有用性に気が付くでしょう。