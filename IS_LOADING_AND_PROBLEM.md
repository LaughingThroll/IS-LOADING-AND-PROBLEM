# IS LOADING AND PROBLEMS
# Всем привет) Сейчас опишу проблему с который мы столкиваемся и какое решение я предлагаю.
 
Все мы используем состояние isLoading или isLoaded. 
Состояние isLoading показывает, что данные сейчас загружаются. 
Состояние isLoaded показывает, что данные загруженные.
То есть если смотреть на это абстрактно, то мы имеем 2 разных состояния. 
Однако у нас используется и тот и тот подход раздельно. 
В одних slice есть isLoading, в остальных isLoaded. 

Раньше мы общалиcь на эту тему с Димой(это наш бывший тех лид). И пришли к выводу, что лучше использовать 
isLoading в slices.
Так как изначально мы имеем флаг isLoading в false и какую то data которая в null то при первом рендере мы можем замечать какие то мелькание на интефейсе.

![alt text](https://github.com/LaughingThroll/IS-LOADING-and-PROBLEM/blob/main/InitialState.png)

Что нам нужно сделать, чтобы избавиться от них. Мы должны написать,
что то на подобие такого `isLoading || !data`, чтобы корректно показывать loader. 

![alt text](https://github.com/LaughingThroll/IS-LOADING-and-PROBLEM/blob/main/isLoading.png)

Мы также можем это засунуть в какой то селектор или локальную переменную 
`const selectIsLoading = (state) =>  state.isLoading || !state.data` 
`const isLoading = isLoading || !data`  

Прекрасно.Однако проблемы не кончаются. 
Следующая проблема заключается в том, что состояние `isLoading || isLoaded` имеет только 2 состояния true или false. 

#### И в чем проблема? 
Например: в моем случае мне нужно узнать, 
что компонент уже загрузился и у него нету данных. 

#### Как это сделать?
Проверить на `!isLoading && !data`. Однако начальное состояние состояний(извините, синоним не пришел в голову) `isLoading = false, data = null`. И мы имеем на первые рендеры мелькание от которого я хотел бы избавиться. 

#### Решение проблемы 
Я понял, что мне нужно 3 состояние, и это `null || undefined`. То есть `isLoading === null && !data`.
Я видел еще второй случай. 
Он находится в компонете AddAppointment(это компонент Дани, возможно он расскажет об этом), 
где непонятно почему у нас есть проверка на `isLoading === undefined`. 

![alt text](https://github.com/LaughingThroll/IS-LOADING-AND-PROBLEM/blob/main/AddAppointment.png)


## Проблемы: 
 - не совсем понятно какая здесь должна быть логика.
 - Не всегда понятно с почему разработчик решил сделать эту проверку.   
 - isLoading или isLoaded имеем только 2 состояния true | false(иногда этого бывает мало).

И пошел я бедный и несчастный в интернет с этой проблемой. 
Немного погуглив я нарвался на статьи которые говорят, 
что лучше вместо isLoading использовать status. 
Он точно будет отображать в каком состоянии наши данные.
Среди этих статей есть статья Kent C. Dodds. 
Которая показывает нам почему мы должны использовать status вместо isLoading.
Также я рассказал об этой проблеме Насте и мы написали Косте. Костя ответил, что status предпочтительние.
Переписку прикрепляю:

![alt text](https://github.com/LaughingThroll/IS-LOADING-AND-PROBLEM/blob/main/chat.png)

### Предлагаю использовать подход со статусами : 
 - его очень легко расширять 
 - дает больше гибкости 
 - код станет больше понятным(мы будем понимать в каком состояние наши данные)   

### Статьи: 
 * https://kentcdodds.com/blog/stop-using-isloading-booleans
 * https://dev.to/joannaotmianowska/status-instead-of-isloading-boolean-53im?utm_source=dormosheio&utm_campaign=dormosheio
 * https://github.com/forwebdev/ui-developer-tips/tree/master/tips/009-data-state
