﻿# Оператор звездочка(splat) в Ruby #

Операто звездочка в Ruby, Groovy и Perl позволяет переключаться между параметрами и массивами: разбивает(split) список в серию параметров, или объединяет(collect) серию параметров для того, чтобы заполнить массив.

Режим разделения превращает массив во множество аргументов:

	duck, cow, pig = *["quack", "mooh", "oing"]
	=> ["quack", "mooh","oing"]

Режим объединения превращает множество аргументво в массив:
	*farm = duck, cow, pig
	=> ["quack", "mooh", "oing"]

Оператор звездочка(splat) может использоваться в case операторе(statement):

	WILD = ['lion', 'gnu']
	TAME = ['cat', 'cow']

	case animal
	  when *WILD
	    "Run"
	  when *TAME
	    "Catch"
	end

И он может использоваться для того, чтобы преобразовать hash в массив:
	
	a = *{:a=>1,:b=>2}
	=>[[:a,1],[:b,2]]
	Hash[*a.flatten]
	=>{:a=>1,:b=>2}


