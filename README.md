# GOST_34.10-2012
Российский стандарт, описывающий алгоритмы формирования и проверки электронной цифровой подписи ГОСТ Р 34.10-2012
## Описание алгоритма

Для реализации формирования и проверки цифровой подписи под сообщением необходимо, чтобы всем пользователям были известны параметры схемы цифровой подписи. Кроме того, каждый пользователь должен иметь ключ подписи d и ключ проверки подписи Q(xq, yq).  

###Формирование цифровой подписи:
Для получения цифровой подписи под сообщением MϵV* необходимо выполнить следующие действия (шаги) по алгоритму:
- Шаг 1 – вычислить хэш–код сообщения M:h ̅=h(M) 
- Шаг 2 – вычислить целое число a, двоичным представлением которого является вектор h ̅, и определить: e≡a(mod q) 
Если e=0, то определить e=1.  
- Шаг 3 – сгенерировать случайное (псевдослучайное) целое число k, удовлетворяющее неравенству: 0<k<q  
- Шаг 4 – вычислить точку эллиптической кривой C=kP и определить  
r≡x_c (mod q)  
где x_c – x-координата точки C.  
Если r=0, то вернуться к шагу 3.  
- Шаг 5 – вычислить значение: s≡(rd+ke)(mod q)  
Если s=0, то вернуться к шагу 3.  
- Шаг 6 – вычислить двоичные векторы r ̅  и s ̅ , соответствующие r и s, и определить цифровую подпись ζ = (r ̅||s ̅) как конкатенацию двух двоичных векторов.
Исходными данными этого процесса являются ключ подписи d и подписываемое сообщение M, а выходным результатом – цифровая подпись ζ.  

###Проверка цифровой подписи:
Для проверки цифровой подписи ζ под полученным сообщением M необходимо выполнить следующие действия (шаги) по алгоритму:  
- Шаг 1 – по полученной подписи ζ вычислить целые числа r и s. Если выполнены неравенства 0 < r < q, 0 < s < q, то перейти к следующему шагу. В противном случае подпись неверна.  
- Шаг 2 – вычислить хэш–код полученного сообщения M: h ̅=h(M) 
- Шаг 3 – вычислить целое число a, двоичным представлением которого является вектор h ̅ и определить e≡a(mod q) 
Если e=0, то определить e=1.  
- Шаг 4 – вычислить значение v≡e^(-1) (mod q)  
- Шаг 5 – вычислить значения z_1≡sv(mod q),z_2≡-rv(mod q)  
- Шаг 6 – вычислить точку эллиптической кривой C=z_1 P+z_2 Q и определить R≡x_c (mod q)  
где x_c – х–координата точки C.  
- Шаг 7 – если выполнено равенство R=r, то подпись принимается, в противном случае - подпись неверна.  
Исходными данными этого процесса являются подписанное сообщение M, цифровая подпись ζ и ключ проверки подписи Q, а выходным результатом – свидетельство о достоверности или ошибочности данной подписи.


####Эллиптическая кривая
Для описания кривой в стандарте NIST используется набор из 6 параметров D=(p,a,b,G,n,h), где:  
- p — простое число, модуль эллиптической кривой;  
- a, b — задают уравнение эллиптической кривой y^2=x^3+ax+b;  
-	G — точка эллиптической кривой большого порядка (это означает что, если умножать точку на числа меньшие, чем порядок точки, каждый раз будут получаться совершенно различные точки);  
-	n — порядок точки G;  
-	h — параметр, называемый кофактор. Определяется отношением общего числа точек на эллиптической кривой к порядку точки G. Данное число должно быть как можно меньше.  

####Хэш-функция
В данном алгоритме цифровой подписи используется хэш-функция «Стрибог» обладающая следующими качествами:  
-	Не имеет свойств, которые позволяли бы применить известные атаки;  
-	Вычисление хэш-функции занимает мало времени;  
-	Каждое используемое в хэш-функции преобразование отвечает за определенные криптографические свойства;  
-	Требования, касающиеся трудоемкости атак на хэш-функцию.

####Параметры:
В работе использована следующая эллиптическая кривая, рекомендованная NIST, в которой значения параметров соответственно равны:  
-	p=6277101735386680763835789423207666416083908700390324961279;
-	a=-3;
-	b=2455155546008943817740293915197451784769108058161191238065;
-	xG=602046282375688656758213480587526111916698976636884684818 (x-координата точки G);
-	yG=174050332293622031404857552280219410364023488927386650641 (y-координата точки G);
-	n=6277101735386680763835789423176059013767194773182842284081;
-	h=1. 

#### Примеры:
> ##### Пример №1
> Исходное сообщение: ***Hello***  
> Цифровая подпись исходного сообщения: ***91973409C1E7767613A0B39A793FD044D49AF64C9996EAC095FBBB13C5AC97B5F903C10115E19115A258F88823BC80B6***  
> Открытый ключ:  
> ***a	{-3}  
> b	{2455155546008943817740293915197451784769108058161191238065}  
> x	{1706303312082624690911106161093747030542476084690097240288}  
> y	{5790727921573013387662764200314105420606516624828771438348}***  
> Проверяемое сообщение: ***Hello***  
> Результат верификации: ***Верификация прошла успешно. Цифровая подпись верна.***  


> ##### Пример №2
> Исходное сообщение: ***Hello***  
> Цифровая подпись исходного сообщения: ***91973409C1E7767613A0B39A793FD044D49AF64C9996EAC095FBBB13C5AC97B5F903C10115E19115A258F88823BC80B6***  
> Открытый ключ:  
> ***a	{-3}  
> b	{2455155546008943817740293915197451784769108058161191238065}  
> x	{1706303312082624690911106161093747030542476084690097240288}  
> y	{5790727921573013387662764200314105420606516624828771438348}***  
> Проверяемое сообщение: ***hello***  
> Результат верификации: ***Верификация не прошла! Цифровая подпись не верна.***  






