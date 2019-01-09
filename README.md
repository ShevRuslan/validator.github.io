# Перейти к **[Демонстрации](https://shevruslan.github.io/form-validator/)**.
## Что это?
Это  __JavaScript__ библиотека, упрощающая валидацию форм и дальнейших действий с ней.
## Синтаксис
Чтобы инициализировать валидатор, напишите вот такой код:
```
let validator = new Validator(config);
```
Конструктор принимает параметры настроек(_config_) :
```
let config = {
    elements: inputs,
    callback: functionCallback,
    checkPasswords: true,
}
```
__elements__ - все поля, которые должны пройти проверку (_принимает либо один объект о поле, либо массив объектов о полях_).

_Пример:_

```
let inputs = [
    {
        name: 'email',
        regex: '^([a-z0-9_-]+\.)*[a-z0-9_-]+@[a-z0-9_-]+(\.[a-z0-9_-]+)*\.[a-z]{2,6}$',
        error: 'Неправильно набран email.',
    },
    {
        name: 'phone',
        regex: '^[a-zA-Zа-яёА-ЯЁ]+$',
        error: 'Неправильно набран телефон'
    },
]
```
Каждый объект поля должен содержать следующие:
- name   - id поля, по-которому он будет искаться (__обязательный параметр__)
- regex  - регулярное выражение, по-которому будет проверятся значение поле (__необязательный параметр__)
- error  - текст ошибки, которая будет выведена, если поле не пройдет валидацию.

__callback__ - функция, которая будет исполнена, после успешного завершения валидации(принимает функцию).

_Пример:_
```
const callback = () => console.log('Поля прошли валидацию!');
```
__checkPassword__ - булево значение, показывающие, будет ли проверятся на идентичность 2 пароля. 
- true - пароли будут проверятся на идентичность по значению
- false - пароли не будут проверятся на идентичность по значению

__Если значение не передано, то будет false.__
> Чтобы функция по проверки паролей работало корректно, вы должны дать __id__ полям с паролями, соответственно __password__ и  __repeatPassword__.

## Как это использовать?

После того, как вы проинициализировали валидатор, и отдали ему параметры настроек, вы должны получить форму в переменную и дать ей обработчик события __'submit'__ :

```
const form = document.querySelector('.form');

form.addEventListener('submit', (e) => {

    e.preventDefault();


    if(validator.validate()) {
        e.currentTarget.submit();
    }

    return false;
});

```
Строчка __e.preventDefault()__ нужна для того, чтобы форма сразу не отсылалась на сервер, а прошла валидацию.
Если валидацию прошла, то строчка __e.currentTarget.submit()__ отправляет ее на сервер.

Это пример показал отправку на сервер без технологии __ajax__, если вам нужно отправить с помощью __ajax__, вы можете передать функцию в callback (при инициализации валидатора), либо использовать следующий образом:

```
form.addEventListener('submit', (e) => {

    e.preventDefault();


    if(validator.validate()) {
        funcAjax();
    }

    return false;
});
```
__Таким образом произойдет валидация, и данные с форм отправятся на обработку, или выполнится какое-то другое действие.__