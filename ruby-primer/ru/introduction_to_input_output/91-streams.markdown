﻿# 9.1 Потоки #

## Потоки ввода/вывода и класс ввода/вывода ##

Поток ввода/вывода это последовательность байтов данных, доступ к которым осуществляется последовательно или в случайном порядке. Это может казаться как абстракция, высокоуровневая концепция - это так! Потоки ввода/вывода используются почти со всем, что относится к твоему компьютеру, то, к чему ты можешь прикоснуться, увидеть или услышать:

* распечатка текста на экран
* нажатие клавиш клавиатуры
* проигрывание звука через колонки
* отправка и получение данных по сети
* чтение и запись файлов, сохраненных на диске

Все эти указыанные в списке операции рассматриваются как "побочный эффект" в Информатике. Метрика прикосновения/видимости/слышимости не похожи на работу сетевого трафика и дисковой активности, но побочные эффекты, не обязательно очевидные; в этих двух случаях, что-то в этом мире физически меняется даже если ты не можешь это увидеть.

Сравнительно, "чистый" код без побочных эффектов: код, который просто выполняет вычисления. Конечно, "чистая" программа не очень полезна, если она не может даже вывести на экран результыты! Это то, где потоки ввода/вывода пригодяться. Ruby класс IO позволяет инициализировать эти потоки.

	# open the file "new-fd" and create a file descriptor:
	fd = IO.sysopen("new-fd", "w")

	# create a new I/O stream using the file descriptor for "new-fd":
	p IO.new(fd) 

fd, первый аргумент IO.new, это дескриптор файла. Это значение типа Fixnum, которые мы присваиваем объекту IO. Мы используем варианты метода sysopen с IO.new, но мы можем также создать IO объекты используя классы BasicSocket и File, которые являются подклассами IO. Мы изучим больше о File в следующем уроке.

Я предупреждал вас, это будет считаться немного абстрактным! Идея создания "дескриптора файла" унаследована из UNIX, где все является файлом. Поэтому вы можете использовать технику выше, чтобы открывать сетевой socket и посылать сообщение другому компьютеру. Вы не сделаете этого конечно - вы возможно будете использовать BasicSocket или TCPSocket класс, который мы только упомянули.

Давайте оставим абстрактную часть в стороне и найдем что-нибудь конкретнее. Имеется связка I/O потоков, которые Ruby инициализирует когда интерпретатор загружается. Список может показаться длинным если вы запустите его локально. Эти примеры выполняются в компактном rubymonk окружении, состоящего из Rails, Passenger и всех других наших волшебных сущностях.

	io_streams = Array.new
	ObjectSpace.each_object(IO) { |x| io_streams << x }

	p io_streams

# Стандартный вывод, ввод и ошибка #

Ruby определяет константы STDOUT, STDIN, и STDERR это IO объекты, указывающие на потоки ввода, вывода, ошибки твоей программы, которые ты можешь использовать через свой терминал, без открытия новых файлов. Ты можешь использовать эти константы, определенные в списке объектов IO, которые мы вывели в последнем примере.

	p STDOUT.class
	p STDOUT.fileno
  
	p STDIN.class
	p STDIN.fileno

	p STDERR.class 
	p STDERR.fileno


Всякий раз, когда ты вызываешь puts, поток output посылается к IO объекту, на который указывает STDOUT. Тоже самое gets, где поток input захватывается объектом IO для STDIN и метод warn, который перенаправляет на STDERR.

Хотя, тут можно кое-что добавить. Модуль Kernel предоставляет нам глобальные переменные $stdout, $stdin,$stderr, которые указывают на те же IO объекты и являются константами STDOUT, STDIN и STDERR. Мы можем увидеть это проверив их object_id.

	p $stdin.object_id
	p STDIN.object_id

	puts

	p $stdout.object_id
	p STDOUT.object_id

	puts

	p $strerr.object_id
	p STDERR.object_id


Как можно видеть, object_ids корректный между глобальными переменными и константами. Всякий раз когда ты вызываешь puts, на самом деле ты вызываешь Kernel.puts (методы Kernel доступны везде в Ruby), который в свою очередь вызывает $stdout.puts.

Так почему все перенаправляется? Цель этих глобальных переменных временное перенаправление: ты можешь присовить эти глобальные переменные другим IO объектам и возвращать IO поток, помимо того, что он связан по умолчанию. Это иногда необходимо, чтобы записывать ошибки в лог или перехватывать ввод из клавиатуры, где обычными средствами это не сделать. Большую часть времени в этом нет необходимости...но это здорово знать, что ты можешь это сделать!

Мы можем использовать StringIO класс, чтобы легко обвести вокруг пальца объекты IO. Попробуй перехватить STDERR так, чтобы вызвать метод warn, перенаправив на наш текущий StringIO объект.

	capture = StringIO.new
	$stderr = capture
