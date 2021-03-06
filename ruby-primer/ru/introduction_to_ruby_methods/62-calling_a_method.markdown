﻿# 6.2 Вызов метода #

## Cooperative objects ##

До сих пор, мы имели дело только с методами, которые только принимают единственный объект как аргумент или параметр. Мы раскроем это и поговорим о способах в которых методы могут принимать более одного параметра. 

Давайте не будем торопиться и начнем с двух параметров.

	def add(a_number, another_number)
	  a_number + another_number
	end

	puts add(1,2)

Таким образом, присоединение второго параметра - действительно просто - просто добавим новый параметр разделяя его от оригинала запятой.

	def add(a_number, another_number, yet_another_number)
	  a_number + another_number + yet_another_number
	end

	puts add(1, 2, 3)

Параметры тоже могут иметь значения по умолчанию. Скажем, мы регулярно складываем три числа, но иногда просто складываем два. Мы можем установить по умолчанию последний параметр в предыдущем примере в 0, если ничего не передается.

	def add(a_number, another_number, yet_another_number = 0)
	  a_number + another_number + yet_another_number
	end

	puts add(1,2)

Старые версии Ruby - 1.8.x и старше - требовали, чтобы вы устанавливали значения по умолчанию для параметров начиная с последнего параметра в списке и перемещаясь назад к первому. Текущая версия Ruby (1.9.x) не имеет ограничений, но все еще заслуживает внимание, поскольку Ruby 1.8.7 все еще используется.

Теперь ваша очередь. Я просто попрошу вас сделать тесты перейдя к упражнение ниже.

	def say_hello(name="Qui-Gon Jinn")
	  "Hello, #{name}
	end

## Аргументы как массив ##

Список параметров передаваемых объекту, в действительности доступны как список. Чтобы это сделать мы используем то, что называется оператор звездочка - который просто знак звездочка ( * ).

Оператор звездочка используется чтобы обрабатывать методы, которые имеют переменный список параметров. Давайте использовать его, чтобы создать метод add, который может обрабатывать любое число параметров.

Мы используем метод inject чтобы перебирать аргументы, которые охвачены в главе Коллекции. Он не имеет прямого отношения к этому уроку, но найдите его, если он возбуждает ваш интерес.

	def add(*numbers)
	  numbers.inject(0) {|sum,number| sum + number}
	end

	puts add(1)
	puts add(1,2)
	puts add(1,2,3)
	puts add(1,2,3,4)

Оператор звездочка работает в оба направления - вы можете использовать его для преобразования массива в список параметров также легко как мы просто преобразовали список параметров в массив.

Я покажу, как мы можем применить оператор звездочка к массиву из трех чисел в список параметров таким образом, что мы сможем работать с одним из этих примеров
ранее этого урока, которые принимает только три параметра.

	def add(a_number, another_number, yet_another_number)
	  a_number + another_number + yet_another_number
	end

	numbers_to_add = [1, 2, 3]
	puts add(*numbers_to_add)

Если ты знаешь некоторые параметры своего метода, ты можешь даже смешивать список параметров и применение оператора звездочка.
Еще раз, в старых версиях Ruby (1.8.x или старше) требует чтобы ты указывал параметры со звездочкой в конце списка параметров, но сейчас этого больше не требуется.

В примере ниже, я расширю пример ранее чтобы добавить сумму, чтобы распечатывать ее как часть сообщения. Мы знаем, что это за сообщение, но мы не знаем как много чисел нам необходимо добавить.

	def add(*numbers)
	  numbers.inject(0) { |sum,number| sum + number }
	end

	def add_with_message(message, *numbers)
	  "#{message} : #{add(*numbers)}"	   
	end

	puts add_with_message("The Sum is",1,2,3)

Почему бы не попробовать его размер? Создайте метод, под названием introduction, который принимает возраст человека, пол и любое количество имен, затем вернуть String, который знакомит этого человека, объединив все эти параметры, чтобы создать сообщение принимаемые тестами.

Как всегда, ваша задача провести тесты. Удели пристальное внимание комментариям тестов, потому что даже простая, поставленная не на то место запятая может явиться причиной, чтобы не выполнить их.


	def introduction(age,gender,*names)
 	  "Meet #{add(*names)}, who's #{age} and #{gender}"
	end

	def add(*names)
  	  names.inject {|str, name| str + " " + name}
	end  

## Именованные параметры ##

В этом последнем разделе метод invocation легче всего продемострировать, чем объяснить. Удели особое внимание в методе invocation методу add в примере ниже. Посмотри как четко можно передать конфигурационные настройки методу; пользователь метода add будет решать, абсолютное значение должно быть возвращено или округление должно произойти.

	def add(a_number, another_number, options = {})
	  sum = a_number + another_number
	  sum = sum.abs if options[:absolute]
	  sum = sum.round(options[:precision] if options[:round]
	  sum
	end

	puts add(1.0134, -5.568)
	puts add(1.0134, -5.568, absolute: true)
	puts add(1.0134, -5.568, absolute: true, round: true, precision: 2)
 
Ruby делает это возможным, позволяя последнему параметру в списке параметров, пропустить использование фигурный скобок если это хеш, делая намного привлекательнее вызов метода. Вот почему мы применяем параметр по умолчанию options={} - потому что если он не передан, должен быть пустой Hash.

Как следствие, первый вызов в примере имеет два параметра, второй три и последний по-видимому пять. На самом деле, второй и третий вызов оба имеют три параметра - 2 числа и hash.

## Не такая легкая тренировка ##

Вы привыкните к этому сейчас. Напишите для меня три метода - calculate, add и sustract. Тесты следует пройти все. Посмотрите на подсказку если у вас есть проблемы! И как небольшая подсказка: запомните то, что вы можете использовать something.is_a?(Hash) или another_thing.is_a?(String) чтобы проверить тип объекта.

	
	def add(*numbers)
  	  numbers.inject(0) {|sum, number| sum + number }
	end

	def subtract(*numbers)
  	  first_number = numbers.shift
	  numbers.inject(first_number) {|result, number| result - number }
	end

	def calculate(*args)

	  options = args.last.is_a?(Hash) ? args.delete(args.last) : {}
	  options[:add] = true if options.empty?
	  return add(*args) if options[:add]
	  return subtract(*args) if options[:subtract]

	end

