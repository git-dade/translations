﻿# 7.2 Блоки в Ruby #

## Lambdas против Блоков ##

Lambda это частьк кода, которую можно сохранить в переменную и она является объектом. Самое простое объяснение блока это часть кода, он не может быть сохранен в переменной и не является объектом. Блоки, как следствие, быстрее чем lambda, но не такой гибкий и также один редких примеров где правило Ruby " все является объектом" нарушается.

Как со множеством вещей в программировании, этого много для этого обучения, но блоки это расширенная тема, таким образом давайте сейчас не будем усложнять. Если вам интересно изучить блоки детально, взгляните на нашу главу о блоках в нашей книге "Ruby Primer:Ascent" в которой объясняются промежуточные и расширенные темы.

Давайте посмотрим на пример блока, который делает тоже самое как и lambda, который вы написали в предыдущем упражнении.

	def demostrate_bock(number)
	  yield(number)
	end

	puts demostrate_block(1) { |number| number + 1|

Давайте перечислим способы в которых этот пример отличается от прошлого упражнения, которое мы сделали когда изучали lambdas.

> нет lambda
> есть что-то новое, называемое yeild
> есть метод у которого есть тело lamda-ы сразу после списка параметров, который немного непонятный.

## Пропуская детали ##

Оказывается, одним из самым распространенным использованием lambda предусмотреть передачу только одного блока в метод, который в порядке очереди использует его, чтоы сделать некоторую работу. Вы увидите это повсеместно в Ruby - повторение Array это отличный пример.

Ruby оптимизирует для этого варианта использования предлагая ключевое слово yield, который может вызывать единственную lambda, которая была явно передана методу без использования списка параметров.

Если вы посмотрели пример в предыдущем разделе, вы обратили внимание на такой шаблон - один метод, один блок переданный ему вне списка параметров и вызовом блока используя yeild.

Сейчас для небоьльшой практики. Используя то, чтовы изучили в прошлых примерах и упражнениях, проведем тесты.

	def calculate(first,second) 
	  yeid(first,second) if block_given?
	end

