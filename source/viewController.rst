Понятие ViewController
==========================

ViewController - управляющий видом
--------------------------------------

ViewController - это "мозги" вашего вида. Вся логика, весь код, если хотите, хранятся здесь и только здесь. ViewController-ы управляют видом, который им принадлежит, именно они говорят ему, как он должен повести себя (но не как он должен выглядеть).

Для примера возьмём этот простой контроллер вида:

| 
.. code-block:: as3

	public class DummyApplicationViewController extends ViewController
	{

		outlet var myButton : Button;
		outlet var myImage : Image;

		outlet var defaultState : State;
		outlet var imageState : State;

		public function DummyApplicationViewController()
		{
		}

		override lifecycle function viewBeforeAddedToStage() : void
		{
			myButton.text = "Click me!";
		}

		event function myButtonTapped() : void
		{
			myButton.text = "Well done!";

			view.currentState = imageState.name;

			(myImage.texture as BitmapData).noise(Math.random() * 100);
		}
	}

|
Здесь мы можем заметить 3 концепции:

#. Outlet-ы, знакомые нам уже из View
#. Lifecycle функции
#. event функции

Давайте рассмотрим их по порядку.

|
Outlets
~~~~~~~~~~~~~~~~~~~~~~

Чтобы получить ссылку на наш аутлет в контроллере, мы должны объявить свойство контроллера с таким же именем и namespace-ом outlet. Примерно вот так:
 
.. code-block:: as3

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

| 
Lifecycle функции
~~~~~~~~~~~~~~~~~~~~~~

У каждой составляющей TF есть свой жизненный цикл (lifecycle), который однозначно определяет этапы его жизни.

Рассмотрим жизненный цикл ViewController-а:

#. **viewLoaded** - ViewController загрузил его View и готов к работе с ним. В этой стадии все статичные Outlet-ы уже замаплены, с ними можно работать
#. **viewBeforeAddedToStage** - вызывается перед тем, как вид будет добавлен в контейнер
#. **viewBeforeRemovedFromStage** - вызывается перед тем, как вид будет удалён из контейнера
#. **viewUnloaded** - ViewController отвязался от View, outlet-ы уже недоступны, но ссылка на view ещё есть - это Ваш последний шанс "подчистить" View 
#. **destroy** - вызывается перед тем, как ViewController будет уничтожен (здесь вы можете отписаться от каких-то собственных событий, остановить таймеры и так далее)

| 
EventHandlers
~~~~~~~~~~~~~~~~~~~~~~

Из описания View мы уже поняли, что TF предоставляет удобный механизм EventMap-ов во View, но как нам обрабатывать эти события? Подписываться на них в контроллере? И ещё не забыть отписаться? Да ещё и думать, когда это всё делать? Нет! Всё это сделает за Вас фреймворк, а Вам останется лишь радоваться встроенной возможности удобного объявления подписчиков событий View.

Как объявить собственный слушатель событий View? Это очень просто! Достаточно создать функцию ViewController-а с namespace-ом event и именем по имени события, которое вы хотите получить. К примеру, вот так:

.. code-block:: as3

	public class DummyApplicationViewController extends ViewController
	{
		outlet var myButton : Button;

		public function DummyApplicationViewController()
		{
		}
		
		event function myButtonTapped() : void
		{
			myButton.text = "Well done!";
		}
	}
	
