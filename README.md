# Инженер 24

Система эксплуатации нескольких зданий. Первый модуль: заявки, неисправности и ремонты.

## Стек
- PHP 8.3, Laravel 12, Livewire, PostgreSQL 16+
- Blade/Tailwind, адаптивный интерфейс на русском
- Хранение времени: UTC; отображение: Asia/Vladivostok

## Роли
Администратор, руководитель, инженер, сотрудник, исполнитель. Область доступа дополнительно ограничена назначенными зданиями. Все запросы проверяются серверными Policies.

## MVP
1. Здания и помещения, назначение пользователей на объекты.
2. Заявки: уникальный номер, здание, помещение, система, заголовок, описание, приоритет, статус, срок, заявитель и исполнитель.
3. Статусы: новая → назначена → в работе → ожидает проверки → закрыта; дополнительные: пауза, отклонена, повторно открыта.
4. Фото (приватное хранение), комментарии, история статусов, записи о работах и материалах.
5. Поиск, фильтры, статистика, просроченные заявки.
6. Автоматические тесты изоляции данных между зданиями.

## План базы данных
- users (id, name, email, password, is_active)
- roles (id, code, name), role_user (user_id, role_id)
- buildings (id, name, address, is_active), building_user (building_id, user_id)
- locations (id, building_id, parent_id, name, floor)
- systems (id, code, name)
- tickets (id, number, building_id, location_id, system_id, title, description, priority, status, reporter_id, assignee_id, due_at, completed_at, closed_at, timestamps)
- ticket_comments (id, ticket_id, user_id, body, timestamps)
- ticket_attachments (id, ticket_id, uploaded_by, path, original_name, mime_type, size, timestamps)
- ticket_status_history (id, ticket_id, user_id, old_status, new_status, created_at)
- ticket_work_logs (id, ticket_id, user_id, description, minutes_spent, timestamps)
- ticket_materials (id, ticket_id, name, quantity, unit, unit_price, timestamps)
- audit_logs (id, actor_id, action, entity_type, entity_id, changes JSONB, created_at)

Уникальные номера заявок создаются транзакционно. Помещение должно принадлежать выбранному зданию. Нужны индексы по зданию, статусу, исполнителю, сроку. Историю изменений нельзя удалять обычным пользователям.

## Безопасность
- Только HTTPS, хеширование паролей, CSRF, защита XSS, валидация ввода.
- Фотографии в приватном хранилище, контроль MIME/размера, серверная проверка скачивания.
- Секреты только в .env и менеджере секретов, не в Git.
- Ежедневное резервное копирование БД и вложений; тест восстановления.
- Не подключать управление физическим оборудованием на этапе MVP.

## Состояние
Документация добавлена; код Laravel и тесты ещё не созданы. Для разработки воспользуйтесь задачей в issue проекта и Codex.
