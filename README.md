# Отчёт. Домашнее задание 4. 
## _Русанов Андрей, БПИ219_

## Вариант 31
> Вторая задача об Острове Сокровищ. Шайка пиратов под предводительством Джона Сильвера высадилась на берег Острова Сокровищ. Не смотря на добытую карту старого Флинта, местоположение сокровищ по-прежнему остается загадкой, поэтому искать клад приходится практически на
ощупь. Так как Сильвер ходит на деревянной ноге, то самому бродить по
джунглям ему не с руки. Джон Сильвер поделил остров на участки, а пиратов
на небольшие группы. Каждой группе поручается искать клад на нескольких
участках, а сам Сильвер ждет на берегу. Группа пиратов, обшарив одну часть
острова, переходит к другой, еще необследованной части. Закончив поиски,
пираты возвращаются к Сильверу и докладывают о результатах. Требуется
создать многопоточное приложение с управляющим потоком, моделирующее действия Сильвера и пиратов. При решении использовать парадигму портфеля задач.

Задание выполнено на оценку 8.

## Модель параллельных вычислений
В программе используется парадигма портфеля задач. Каждый поток сначала обращается к портфелю задач, коим является обычное целое число, для выяснения текущего номера задачи(в данном случае номер исследуемого участка) после этого увеличивает его, выполняет задачу, затем обращается к портфелю задач для выяснения следующего номера участка.
В общем случае, я использую модель Общей памяти (Shared Memory) с помощью использования мьютексов.
[**Источник информации**](https://vestnik.susu.ru/cmi/article/viewFile/8796/7201)

## Входные данные 
Сначала вводятся два целых числа от 0 до 9 - координаты сокровища
Затем вводится целое число - количество групп пиратов(определяет число потоков).
Затем вводится число участков, на которые будет разбит остров. 

Остров Сокровищ - единственный и уникальный, его размеры постоянны и равны 20X20 клеток.

Клетка острова попадает в участок номер n если (i + j*20)/20 = n-1, где i, j - координаты.

## Выходные данные 
Прогррамма выоводит сообщения об активности групп пиратов на острове. В конце работы выводится сообщение о том, какая по счёту группа нашда клад, на каком по счёту участке в каких координатах.

## Решение на С++
В файле main.cpp представлено решение задачи с использованием мьютексов.
Приложение компилируется с помощью  комманды:
> $g++ -lpthread main.cpp

## Сценарий, описывающий поведение объектов разрабатываемой программы
Капитан Сильвер имеет в распоряжении несколько групп пиратов. Сильвер делит остров на несколько участков и отправляет группы пиратов на участки искать клад. После того как группа полностью исследовала участок, она отправляется на следующий незанятый участок. После как группа пиратов исследовала один участок, она переходит на другой свободный участок. Если одна из групп нашла сокровище, все группы заканчивают исследрвание своих участков и идут докладывать Флинту об успехах.

## Обобщённый алгоритм
- Капитан сильвер - пользователь. Он назначает количество групп пиратов и разбивает остров на участки. В конце получает сообщение о том, какая по счёту группа нашла клад и на каком по счёту участке острова. 

- Группа пиратов в программе представлена в виде потока, выполняющего функцию pirate_group. У группы есть номер group_number, чтобы отличать её от другиз групп в результате вывода программы.

- Группа пиратов продолжает поиски пока одна из групп не найдёт клад.

- Остров Сокровищ представляет собой массив bool в котором сокровище отмечено единственным элементом true.

## Ввод через аргументы командной строки
- Чтобы ввести n и m через аргументы командной строки, нужно после ./a.out ввести флаг i, а затем через пробел tr_i, tr_j, number_of_groups, number_of_areas. Например: `./a.out i 2 5 4 6.`
- Чтобы ввести данные через файл и вывести результат работы в другой файл, нужно после ./a.out ввести флаг f, а затем пути к файлам через пробел. Например:  ./a.out f ./input.txt ./output.txt. Результат также будет выводится в консоль. В файле должны быть записаны только 4 числа через пробел: tr_i, tr_j, number_of_groups, number_of_areas. Например: `./a.out f ./input.txt ./output.txt`.
- Чтобы сгенерировать данные случайным образом, нужно после ./a.out ввести флаг r. Например: `./a.out r`.

В папку tests добавлены тесты с возможными результатами вывода.