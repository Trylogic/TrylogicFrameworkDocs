Понятие ViewController
==========================

ViewController - управляющий видом
--------------------------------------

ViewController - это "мозги" вашего вида. Вся логика, весь код, если хотите, хранятся здесь и только здесь. ViewController-ы управляют видом, который им принадлежит, именно они говорят ему, как он должен повести себя (но не как он должен выглядеть).



Outlets
~~~~~~~~~~~~~~~~~~~~~~

Понятие аутлетов было взято создателями TF из мира iOS. Они позволяют связать определённую составляющую вида с свойством контроллера. На самом деле, со стороны View аутлеты есть не что иное, как обычные свойства. Чтобы получить ссылку на наш аутлет в контроллере, мы должны объявить свойство контроллера с таким же именем и namespace-ом outlet. Примерно вот так:
 
.. code-block:: as3
	:linenos:

	public class DummyApplicationViewController extends ViewController
	{
		outlet var myButton : Button;

		public function DummyApplicationViewController()
		{
		}

		override lifecycle function viewBeforeAddedToStage() : void
		{
			myButton.text = "Click me!";
		}
	}


Просто, не правда ли?
	
Вы можете спросить, как так получилось, мы нигде не объявляли значение этой переменной, как контроллер узнал, что ей надо присвоить значение свойства ``myButton`` из View? Ответ кроется во внутренней реализации контроллера, который при создании View проходится по всем своим outlet-ам и запрашивает их у View. View обязательно должна предоставлять все запрашиваемые outlet-ы, иначе вы получите ошибку.

Вам не надо заботиться о том, что контроллер хранит ссылку на компонент вида, "связывание", как и "отвязывание" outlet-ов происходит автоматически внутри фреймворка.


EventMaps
~~~~~~~~~~~~~~~~~~~~~~