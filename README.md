# Задание 1

### Исходный проект

**Структура исходного проекта:**

- 📁 backend
	- ...
- 📁 frontend
	- 📁 microfrontend
		- ПУСТО
	- 📁 public
		- логотипы и др. ...
	- 📁 src
		- 📁 blocks
			- 📁 auth-form
				- файлы стилей *.css ( шрифт, скругление углов, относительное позиционирование, и т.п.) для auth-form
			- 📁 card
				- файлы стилей *.css
			- 📁 content 
				- ...
			- 📁 footer
				- ...
			- 📁 header
				- ...
			- 📁 login
				- ...
			- 📁 page
				+ ...
			- 📁 places
				- ...
			- 📁 popup
				- ...
			- 📁 profile
				- ...
		- 📁 **components**
			- 📄 AddPlacePopup.js
			- 📄 App.js
			- 📄 Card.js
			- 📄 EditAvatarPopup.js
			- 📄 EditProfilePopup.js
			- 📄 Footer.js
			- 📄 Header.js
			- 📄 ImagePopup.js
			- 📄 InfoTooltip.js
			- 📄 Login.js
			- 📄 Main.jd
			- 📄 PopupWithForm.js
			- 📄 ProtectedRoute.js
			- 📄 Register.js 
		- 📁 context
			- CurrentUserContext.js
		- 📁 images
			- 🖼️ ....
		- 📁 utiles
			- api.js
			- auth.js
		- 📁 vendor
			- 📁 fonts 
				- ...
			- fonts.css
			- normalize.css
		- 📄 index.css
		- 📄 index.js
		- 🖼️ logo.svg
		- 📄 serviceWorker.js
		- 📄 setupTests.js
	- 📄 index.spec.js
	- 📄 package.json - информацию о сторонних пакетах (библиотеках), которые используются приложением
	- 📄 package-lock.json
- docker - файлы

### Выбор подхода 

Чтобы в будущем проект могли развивать несколько независимых команд, декомпозируем фронтенд проекта. Рассмотри два популярных инструмента для реализации микрофронтендов: Single SPA и Webpack Module Federation. 

Для данной задачи хорошо подойдёт Webpack Module Federation. Он хорошо пожходит, если мы хотим:
- Оптимизировать загрузку зависимостей и уменьшить дублирование кода
- Все микрофронтенды используют один и тот же фреймворк (Лучше подходит для проектов, где все микрофронтенды используют один и тот же стек технологий, как в нашем случае.)

Преимущества предоставляет Webpack Module Federation:
- Возможность использовать разные версии библиотек без конфликтов
- Независимость разработки и развёртывания микрофронтендов
- Оптимизация использования ресурсов за счёт ленивой загрузки модулей

### Новая структура проекта, разбивка на модули

Module Federation объединяет разные модули приложения в единое целое и позволяет им использовать общий код. В такой композиции есть две роли — хост и удалённый модуль:

- **Хост** (host) — это основное приложение. Когда вы его запускаете, оно динамически загружает удалённые модули.
- **Удалённый модуль** (remote) — это отдельный микрофронтенд. Он предоставляет часть своей функциональности хосту или другим удалённым модулям.

Модули:
- Хост (основное приложение)
	+ App (основной компонент, является ядром приложения и отвечает за: маршрутиацию, управление состоянием, всплывающие окна, взаимодейтвие с API, авторизацию)
	+ Footer (отображение подвала)
	+ Header (отображение шапки)
	+ Main (отображение профиль пользователя и список карточек, имеет кнопки редактирования крточек, профиля и т.п.)
	+ ProtectedRoute (создание защищённого маршрута, проверяет, авторизован ли пользователь)
- Аутентификация (auth)
	+ Login (компонент логина пользователя)
	+ Register (компонент регистрации пользователя)
	+ InfoTooltip (информационное окно для регистрации)
- Работа с профилем пользователя (отображение и редактирование)
	+ EditAvatarPopup (редектирование аватарки)
	+ EditProfilePopup (редактирование профиля)
- Карточки мест (добавление и удаление карточек, добавление и удаление лайков)
	+ Card (карточка места, отображает карточку с изображением, названием, кнопкой лайка и кнопкой удаления)
	+ ImagePopup (отображение карточки)
	+ AddPlacePopup (добавление новой карточки)
	+ PopupWithForm (диалоговое окно для сохранения)



**Новая структура проекта :**

- 📁 microfrontend
	- 📁 auth-microfrontend
		+ 📁 components
			* 📄 Register.js 
			* 📄 Login.js
			* 📄 InfoTooltip.js
		+ 📁 utiles
			* 📄 auth.js
		+ 📁 blocks
			* 📁 auth-form 
				- файлы стилей *.css ( шрифт, скругление углов, относительное позиционирование, и т.п.) для auth-form
			- 📁 login
				+ - файлы стилей *.css
		- 📄 package.json - информацию о сторонних пакетах (библиотеках), которые используются модулем
		- 📄 webpack.config.js - файл конфигурации для инструмента сборки Webpack (Он определяет, как Webpack должен обрабатывать и компилировать различные файлы проекта, такие как JavaScript, CSS, изображения и другие ресурсы)
		- 📄 index.js - точка входа микрофронтенда
	- 📁 profile-microfrontend
		- 📁 components
			+ EditAvatarPopup.js
			+ EditProfilePopup.js
		+ 📁 blocks
			* 📁 profile
				- файлы стилей *.css
		- 📁 images 
			+ 🖼️ avatar.jpg
			+ и др.
		+ 📄 package.json
		- 📄 webpack.config.js 
		- 📄 index.js
	- 📁 places-microfrontend
		+ 📁 components
			*  📄 AddPlacePopup.js
			*  📄 Card.js
			*  📄 ImagePopup.js
			*  📄 PopupWithForm.js
		+ 📁 blocks
			* 📁card
				- файлы стилей *.css
			* 📁places
				- файлы стилей *.css
			* 📁popup
				- файлы стилей *.css
		+ 📁 images
			* add-icon.svg
			* card_1.jpg
			* и др.
		* 📄 package.json
		- 📄 webpack.config.js  
		- 📄 index.js
	- 📁 host
		+ 📁 components
			* App.js
			* Footer.js
			* Header.js
			* Main.js
			* ProtectedRoute.js
		+ 📁 contexts
			* 📄 CurrentUserContext.js
		+ 📁 blocks
			* 📁 footer
				- файлы стилей *.css
			- 📁 header
				+ файлы стилей *.css
			* 📁 page
				- файлы стилей *.css
		- 📁 utils 
			+ 📄 api.js
		+ 📁 images
			* logo.svg
			* и др.
		* 📁 public 
			-  favicon.ico
			-  index.html
			-  и др.
		- 📄 package.json
		- 📄 webpack.config.js   
		- 📄 index.js
		 
	
# Задание 2

В этом задании вам нужно декомпозировать схему веб-приложения на Django на микросервисы. 
Решение содержится в файле _arch_template_task2_new.drawio_ (конвертированная в изображение схема _arch_template_task2_new.drawio.png_)

[Посмотреть схему](./arch_template_task2_new.drawio.png)
