제곱하여 -1 이 되는 숫자 하나를 가정하고 이것을 '존재하지 않는 숫자의 기본 단위'라는 뜻에서 Imaginary unit (허수 단위) 라고 부른다. 이는 $i$ 로 표기하면 다음과 같다.
$$i = \sqrt{-1},\quad i^2 = -1$$
허수 체계의 숫자들은 일반적으로 다음의 꼴을 가진다.
$$ z = a + bi$$
여기에서 $b$ 가 0 인 경우 $z$ 는 실수이고 $b$ 가 0 이 아닌 경우 $z$ 는 허수이다.
위와 같이 $a +bi$ 의 형태로 나타낼 수 있는 숫자를 Complex Number(복소수) 라 하며 , 이때 $a$ 를 실수부, $b$ 를 허수부라 한다.

어떤 복소수 $z$ 에서 실수부나 허수부만을 취할 경우 다음과 같다.
$$\begin{align} &Re(a + bi) = a \\ &Im(a+bi) = b\end{align}$$
또한 전체 복소수의 집합은 기호 $C$ 로 나타낸다. (그러므로 $R\subset C$ 이다.)

- 복소수의 상등
복소수가 서로 상등하기 위해서는 실수부와 허수부가 모두 같아야 한다.
즉, $z_1$ 과 $z_2$ 를 복소수라 할 때 다음이 성립한다.
$$(z_1 = z_2)\Leftrightarrow (Re(z_1) = Re(z_2))\; \cap \; (Im(z_1)=Im(z_2))$$

- 복소수의 사칙연산
두 복소수 $z_1 = a+bi$ 와 $z_2 = c+di$ 에 대하여
$$\begin{align}&z_1 + z_2 = (a + bi) + (c+di) = (a+c) + (b+d)i \\ & z_1 - z_2 = (a + bi) - (c+di) = (a-c) + (b-d)i \\ & z_1\cdot z_2 = (a+bi)(c+di) = ac + adi + bci + bd(i^2) = (ac-bd) +(ad+bc)i\end{align}$$
$$\begin{align}z_1\div z_2 &= \frac{a+bi}{c+di} \\ &= \frac{(a+bi)(c-di)}{(c+di)(c-di)}\\&=\frac{ac+bci-adi-bd(i^2)}{c^2-(di)^2}\\&=\frac{(ac+bd)+(bc-ad)i}{c^2+d^2}\end{align}$$

- 켤레복소수 (complex conjugate)
복소수 $z$ 에 대하여 $z$ 와 허부수의 부호만 다른 복소수를 $z$ 의 켤레복소수라 하고 $\overline{z}$ 로 표기한다.

