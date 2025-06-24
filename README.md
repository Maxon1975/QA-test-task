# ✅ Тестовое задание — QA (Тестировщик)

## 📌 Задача:
Написать E2E-тесты для системы дистанционного обучения:  
🔗 [https://dev2.getinfo.radugi.net](https://dev2.getinfo.radugi.net.radugi.net)  
🔐 Логин: `dumbledore@sct.team`  
🔓 Пароль: `12345678qQ1`

---

## 🔧 Используемые инструменты:

- **Фреймворк**: [Cypress](https://www.cypress.io/)
- **Язык**: JavaScript
- **Дополнительно**: Mocha + Chai

---

## 📁 Структура проекта:

```
cypress/
├── e2e/
│   ├── login.cy.js
│   ├── forgot_password.cy.js
│   ├── company_page.cy.js
│   └── user_profile.cy.js
├── support/
│   └── commands.js
└── cypress.config.js
```

---

## 1. ✅ Форма авторизации

### Тест-кейсы:
1. Успешный вход с корректными учетными данными
2. Ошибка при неверном логине или пароле
3. Проверка, что после успешного входа пользователь перенаправляется на главную страницу

```javascript
// cypress/e2e/login.cy.js
describe('Авторизация', () => {
    it('Успешный вход', () => {
        cy.visit('/login');
        cy.get('#email').type('dumbledore@sct.team');
        cy.get('#password').type('12345678qQ1');
        cy.get('form').submit();
        cy.url().should('include', '/dashboard'); // проверка редиректа
    });

    it('Ошибка при неверном логине/пароле', () => {
        cy.visit('/login');
        cy.get('#email').type('wrong@example.com');
        cy.get('#password').type('wrongpass');
        cy.get('form').submit();
        cy.contains('Неверный логин или пароль').should('be.visible');
    });
});
```

---

## 2. ✅ Форма восстановления пароля

### Тест-кейсы:
1. Наличие ссылки "Забыли пароль?"
2. Открытие формы восстановления
3. Проверка отправки письма с инструкциями

```javascript
// cypress/e2e/forgot_password.cy.js
describe('Восстановление пароля', () => {
    it('Проверка наличия ссылки и открытия формы', () => {
        cy.visit('/login');
        cy.contains('Забыли пароль?').click();
        cy.contains('Восстановление пароля').should('be.visible');
    });

    it('Отправка запроса на восстановление', () => {
        cy.visit('/forgot-password');
        cy.get('#email').type('dumbledore@sct.team');
        cy.get('form').submit();
        cy.contains('Инструкции отправлены на email').should('be.visible');
    });
});
```

---

## 3. ✅ Доступность страницы "Компания" после авторизации

```javascript
// cypress/e2e/company_page.cy.js
describe('Страница Компания', () => {
    beforeEach(() => {
        cy.login('dumbledore@sct.team', '12345678qQ1');
    });

    it('Доступ к странице Компания', () => {
        cy.visit('/company');
        cy.contains('О компании').should('be.visible');
    });
});
```

> Примечание: реализация `cy.login()` описана ниже.

---

## 4. ✅ Совпадает ли текущий пользователь с указанным руководителем

```javascript
// cypress/e2e/user_profile.cy.js
describe('Проверка профиля пользователя', () => {
    beforeEach(() => {
        cy.login('dumbledore@sct.team', '12345678qQ1');
    });

    it('Пользователь указан как руководитель компании', () => {
        cy.visit('/company');
        cy.contains('Руководитель: Dumbledore').should('be.visible');
    });
});
```

---

## 🛠 Поддержка: добавляем команду входа

```javascript
// cypress/support/commands.js
Cypress.Commands.add('login', (email, password) => {
    cy.visit('/login');
    cy.get('#email').type(email);
    cy.get('#password').type(password);
    cy.get('form').submit();
    cy.url().should('include', '/dashboard');
});
```

---

## 📦 Инструкция по запуску

1. Установите Cypress:
```bash
npm init -y
npm install cypress --save-dev
```

2. Замените содержимое файла `cypress.config.js` следующим:

```js
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    baseUrl: 'https://dev2.getinfo.radugi.net',
    setupNodeEvents(on, config) {
      return config;
    },
  },
});
```

3. Вставьте ваши тестовые файлы в папку `cypress/e2e/`.

4. Запустите Cypress:
```bash
npx cypress open
```

или выполните все тесты в headless режиме:
```bash
npx cypress run
```

---

## 📁 Как отправить результат?


---












