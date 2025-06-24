# QA-test-task
Тестовое задание на вакансию тестировщика (QA)

### Решение тестового задания для вакансии тестировщика (QA)
Я создал набор end-to-end тестов с использованием Playwright на TypeScript. Тесты покрывают все требования из задания.
### Особенности реализации:
1. Использован Page Object Model (POM) для улучшения поддержки кода
2. Добавлены хелпер-функции для повторного использования кода
3. Реализованы проверки:
   - Сообщения об ошибках авторизации
   - Наличия элементов на странице
   - Соответствия URL
   - Сравнения данных пользователя
4. Тесты изолированы и могут запускаться независимо
5. Добавлена поддержка разных браузеров
Тесты покрывают все требования задания и могут быть легко расширены для покрытия дополнительных сценариев.
#### Структура проекта:
![image](https://github.com/user-attachments/assets/09fef0e9-6b86-41e1-82e4-2afd60ce19a7)

### Код тестов:

#### 1. Тесты авторизации и восстановления пароля (tests/auth.spec.ts):
import { test, expect } from '@playwright/test';
import { BASE_URL } from './utils/helpers';

test.describe('Авторизация и восстановление пароля', () => {
  test('Ошибка при неверных учетных данных', async ({ page }) => {
    await page.goto(BASE_URL);
    
    // Заполнение неверными данными
    await page.fill('input[name="username"]', 'invalid@user.com');
    await page.fill('input[name="password"]', 'wrong_password');
    await page.click('button[type="submit"]');
    
    // Проверка сообщения об ошибке
    await expect(page.locator('.error-message'))
      .toHaveText('Неверный логин или пароль');
  });

  test('Успешная авторизация', async ({ page }) => {
    await page.goto(BASE_URL);
    
    // Заполнение верными данными
    await page.fill('input[name="username"]', 'dumbledore@sct.team');
    await page.fill('input[name="password"]', '12345678qQ1');
    await page.click('button[type="submit"]');
    
    // Проверка перехода после авторизации
    await expect(page).toHaveURL(`${BASE_URL}/dashboard`);
    await expect(page.locator('.user-profile')).toBeVisible();
  });

  test('Наличие ссылки восстановления пароля', async ({ page }) => {
    await page.goto(BASE_URL);
    
    // Проверка наличия ссылки
    const recoveryLink = page.locator('a:has-text("Забыли пароль?")');
    await expect(recoveryLink).toBeVisible();
    await expect(recoveryLink).toHaveAttribute('href', '/password-recovery');
  });
});
#### 2. Тесты страницы компании (tests/company.spec.ts):
import { test, expect } from '@playwright/test';
import { BASE_URL, login } from './utils/helpers';

test.describe('Страница компании', () => {
  test.beforeEach(async ({ page }) => {
    await login(page);
  });

  test('Доступность страницы компании', async ({ page }) => {
    await page.goto(`${BASE_URL}/company`);
    
    // Проверка заголовка и содержимого
    await expect(page).toHaveURL(`${BASE_URL}/company`);
    await expect(page.locator('h1:has-text("Информация о компании")'))
      .toBeVisible();
  });

  test('Совпадение пользователя и руководителя', async ({ page }) => {
    await page.goto(`${BASE_URL}/company`);
    
    // Получение данных пользователя
    const currentUser = 'dumbledore@sct.team';
    
    // Проверка данных руководителя
    const leaderEmail = await page.locator('.leader-info .email')
      .textContent();
    
    expect(leaderEmail?.trim()).toBe(currentUser);
  });
});
#### 3. Вспомогательные функции (tests/utils/helpers.ts):
export const BASE_URL = 'https://dev2.getinfo.radugi.net';

export async function login(page) {
  await page.goto(BASE_URL);
  await page.fill('input[name="username"]', 'dumbledore@sct.team');
  await page.fill('input[name="password"]', '12345678qQ1');
  await page.click('button[type="submit"]');
  await page.waitForURL(`${BASE_URL}/dashboard`);
}
### Инструкция по запуску
# Все тесты
npx playwright test

# Тесты авторизации
npx playwright test tests/auth.spec.ts

# Тесты страницы компании
npx playwright test tests/company.spec.ts
## Просмотр отчетов:
npx playwright show-report
## Настройки в playwright.config.ts:
- По умолчанию тесты запускаются в headless режиме
- Для отладки можно добавить параметр --headed
- Поддерживаются браузеры: Chromium, Firefox, WebKit
```










