Понятие View
==========================

View - абстракция представления
--------------------------------------

В терминах TrylogicFramework-а, View является единицей отображения. Так что же представляет из себя View на самом деле? Давайте посмотрим:

| 
| 

.. image:: _static/ViewDescription.png
	:align: center
	
| 
| 
| 
| 
Для примера возьмём этот простой вид (для описания View так же используется MXML, хотя вы и можете делать это в Pure AS коде):

| 
.. code-block:: mxml

	<?xml version="1.0"?>
	<gui:ContainerBase xmlns:fx="http://ns.adobe.com/mxml/2009"
				   xmlns:gui="http://www.trylogic.ru/gui"
				   xmlns:trylogic="http://www.trylogic.ru/"
				   xmlns:s="library://ns.adobe.com/flex/spark">

		<gui:controllerClass>ru.trylogic.dummy.views.dummyApplicationView.DummyApplicationViewController</gui:controllerClass>

		<gui:eventMaps>
			<trylogic:EventMap source="{myButton}" type="tap" destination="{new Event('myButtonTapped')}" />
		</gui:eventMaps>

		<gui:states>
			<s:State id="defaultState" name="default" />
			<s:State id="anotherState" name="another" />
		</gui:states>

		<gui:Button id="myButton" x="100" y="100" y.another="200" />

	</gui:ContainerBase>

| 	

Как Вы видите, контроллер вида задаётся явно в виде класса. У одного вида может быть только один контроллер, и наоборот.

Связь с View его контроллер осуществляет с помощью трёх доступных методов:

#. Outlet-ы (в данном примере Button является Outlet-ом с идентификатором ``myButton`` )
#. Посредством наблюдения за Event-ами, которые вид диспатчит (в примере вид задиспачит событие с идентификатором ``myButtonTapped`` при возникновении события ``tap`` на кнопке ``myButton``)
#. States (Состояния) (в нашем View объявленно два состояния с именами ``default`` и ``another`` )

Давайте рассмотрим эти методы более детально.

| 
Outlets
~~~~~~~~~~~~~~~~~~~~~~

Понятие аутлетов было взято создателями TF из мира iOS. Они позволяют связать определённую составляющую вида с свойством контроллера. На самом деле, со стороны View аутлеты есть не что иное, как обычные свойства. Но так же есть возможность использовать специальный тег ``Outlet``, позволяющий мапить аутлеты с помощью биндинга:

.. code-block:: mxml

	<trylogic:Outlet id="myButtonFace" instance="{myButton.face}" />

| 
EventMaps
~~~~~~~~~~~~~~~~~~~~~~

Карты событий (EventMaps) - это механизм, дающий возможность проксировать одно событие, возникающее на ``source`` с типом ``type``, на другое событие, которое будет задиспатчено относительно View.

Рассмотрим данный выше пример EventMap-а:

.. code-block:: mxml

	<trylogic:EventMap source="{myButton}" type="tap" destination="{new Event('myButtonTapped')}" />
	 
Это - самый примитивный, но чаще всего используемый вариант. Когда у объекта ``myButton`` произойдёт событие ``tap``, то View задиспатчит событие ``myButtonTapped`` и контроллер (либо другой View) сможет об этом узнать.

| 
States
~~~~~~~~~~~~~~~~~~~~~~

States (состояния) - это очень удобный концепт, позволяющий Вам менять параметры View в зависимости от его текущего состояния. В TF используется модель States от Flex 4 (важно понимать, что TF не наследуется от Flex-а и не тянет за собой ничего лишнего).

Пример объявленных состояний:

.. code-block:: mxml

	<gui:states>
		<s:State id="defaultState" name="default" />
		<s:State id="anotherState" name="another" />
	</gui:states>

... и их использования

.. code-block:: mxml

	<gui:Button id="myButton" x="100" y="100" y.another="200" />
	
Обратите внимание на то, как объявленно свойство ``y`` у кнопки ``myButton``. Запись ``y.another`` означает "значение ``y`` в состоянии ``another``". Когда у свойства не указано состояние, то это означает, что значение будет использовано всегда, когда не указано другое для другого состояния.