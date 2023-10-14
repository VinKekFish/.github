# VinKekFish

__For English__ see translator
https://translate.yandex.ru/?lang=ru-en
or
https://translate.google.com/?sl=ru&tl=en&op=translate

# Системные требования
Требует .NET 7.0

Программа предназначена для работы под Linux, т.к. использует часть путей Linux (/dev/random), некоторые функции библиотеки libc и sh-скрипты для сборки


Не уверен, что доведу проект до конца.
Пока для пользователя здесь нет ничего интересного. Приходите в 2024 году, возможно, я допишу.

# Описание
Это проект небольшого криптографического приложения с ключом 4096 битов симметричного шифрования (для платформы .NET 7.0, только под Linux). Основной шифр - самодельный VinKekFish стойкостью от 4096 до 45056 битов, построенный как губка на основе комбинации keccak и Threefish.
Также в проекте используется keccak 512 битов в самодельной схеме шифрования и специальная каскадная губка из keccak, стойкостью от 2 килобитов до 88 килобитов.
Так как шифр не проходил криптоанализ, стойкость не гарантируется. Так что на ваш страх и риск.

Реализация на C#.

# Для конечного пользователя
Пока для пользователя здесь нет ничего интересного. Приходите в 2024 году, возможно, я допишу.
В настоящий момент времени идёт разработка сервиса, который собирает энтропию для генерации ключей и паролей.

# Для программистов

## Реализованы примитивы

### keccak
Keccak ерсии 512 битов (максимальная длина из возможных)

	Сам примитив
	https://github.com/VinKekFish/VinKekFish/blob/main/src/main/3%20cryptoprime/keccak/KeccakPrime.cs

	функция getHash512 - это пример, как рассчитать хеш (обратите внимание, этот хеш отличается от sha-3)
	https://github.com/VinKekFish/VinKekFish/blob/main/src/main/5%20main-crypto/keccak/keccak-20200918/keccak-base-20200918.cs

	генератор псевдослучайных чисел (очень медленно работает; не протестирован)
	https://github.com/VinKekFish/VinKekFish/blob/master/vinkekfish/keccak/keccak-20200918/Keccak_PRNG_20201128.cs

### Threefish
Threefish версии 1024 битов (тоже максимальная длина; только на шифрование). Осторожно, там нужно вычислить третий tweak (tweak1 ^ tweak2) и передать его в массиве твиков, а также вычислить расширение ключа (threefish_unsafe как раз это делает).
	[Класс для подготовки ключей и твиков](https://github.com/VinKekFish/VinKekFish/blob/main/src/main/3%20cryptoprime/ThreeFish/threefish_unsafe.cs)
	[Класс самого ThreeFish](https://github.com/VinKekFish/VinKekFish/blob/main/src/main/3%20cryptoprime/ThreeFish/Threefish_Static_Generated.cs)


Примеры использования смотрите в тестах в проекте main_tests.
	https://github.com/VinKekFish/VinKekFish/blob/main/src/tests/src/main/cryptoprime/keccak/tests/tests.cs
 	https://github.com/VinKekFish/VinKekFish/tree/main/src/tests/src/main/cryptoprime/ThreeFish/tests

## Основные примитивы программы
### VinKekFish (ВинКекФиш)
	[Описание примитива](https://github.com/VinKekFish/VinKekFish/blob/main/Docs/Dev/Crypto/VinKekFish/Description/VinKekFish.md)
	[Сам примитив](https://github.com/VinKekFish/VinKekFish/tree/main/src/main/5%20main-crypto/VinKekFish/VinKekFish-kn-20210525)

### CascadeSponge (каскадная губка)
	[Описание примитива (пока не закончено)](https://github.com/VinKekFish/VinKekFish/blob/main/Docs/Dev/Crypto/VinKekFish/Description/cascadeSponge.md)
 	[Сам примитив](https://github.com/VinKekFish/VinKekFish/tree/main/src/main/5%20main-crypto/CascadeSponge/20230930mt)
	[Генератор псевдослучайных чисел и таблиц перестановок до 64*1024 элементов (пока не протестирован)](https://github.com/VinKekFish/VinKekFish/blob/main/src/main/5%20main-crypto/CascadeSponge/20230905/CascadeSponge-1t_prng.cs)
