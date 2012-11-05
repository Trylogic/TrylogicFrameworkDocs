Типичная структура приложения
==========================

Базовая иерархия приложения
--------------------------------------

Скелет программы в простейшем виде представлен на данной картинке:

.. image:: _static/TrylogicFrameworkStructure.png
	:align: center


Bootstrap
~~~~~~~~~~~~~~~~~~~~~~

Это "точка входа" Вашего приложения, а так же его главный класс. 

Рассмотрим типичный пример такого класса (настоятельно рекомендуется использовать MXML для его описания):

.. code-block:: mxml
	:linenos:

	<?xml version="1.0"?>
	<native:Bootstrap xmlns:fx="http://ns.adobe.com/mxml/2009"
					  xmlns:native="http://www.trylogic.ru/native"
					  xmlns:services="ru.trylogic.dummy.services.*"
					  xmlns:trylogic="http://www.trylogic.ru/ioc/">
		<fx:Metadata>
			[SWF(width="1024", height="768", frameRate="60", backgroundColor="0x909090")]
		</fx:Metadata>

		<native:services>
			<services:DummyService someParam="someValue" anotherParam="someAnotherValue" />
		</native:services>

		<native:iocMap>
			<trylogic:Associate iface="tl.actions.IActionLogger" withClass="ru.trylogic.dummy.core.DummyActionLogger" factory="tl.factory.SingletonFactory" />
		</native:iocMap>

		<native:applicationViewClass>ru.trylogic.dummy.views.dummyApplicationView.DummyApplicationView</native:applicationViewClass>

	</native:Bootstrap>

Выбор системы отрисовки, используемой фреймворком, осуществляется с помощью указания одного лишь namespace-а для этого класса. В данном примере указано пространство имён "http://www.trylogic.ru/native", что означает, что для отрисовки будет использован стандартный пакет ``flash.display.*``, но, сменив его на, к примеру, "http://www.trylogic.ru/starling", приложение будет отрисовываться уже с помощью Starling Framework-а.

Так же Trylogic Framework даёт Вам возможность написать собственные View Adapter-ы для систем отрисовки, которые не идут "в комплекте" (например, Genome2D). 

Рассмотрим остальные свойства Bootstrap-а, доступные программисту:

#. services - это ``Vector.<IService>``, позволяющий указать сервисы, которые будут обслуживать данное приложение 
#. iocMap - "карта" ассоциаций IoC фреймворка, который используется в TrylogicFramework (см. Insulin)
#. applicationViewClass - класс View приложения, который обязательно надо указать, иначе приложение не сможет стартовать (что, впринципе, разумно:))

ApplicationView
~~~~~~~~~~~~~~~~~~~~~~

Какое приложение без его View (вида)? А везде, где кого-то может быть несколько, есть главный. ApplicationView - это главный вид, отправная визуальная точка. В зависимости от Вашей структуры, он может иметь потомков в виде других View, либо представлять собой единственный вид в приложении.

Кроме того, что этот вид - главный, больше его от других видов ничего не отличает и им может быть любой View. 