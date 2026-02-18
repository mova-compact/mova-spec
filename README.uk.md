# MOVA 5.0.0 — Машинно-операбельні вербальні дії (базова специфікація)

Англійська версія: [README.md](README.md)

## Статус і призначення
- Канонічний GitHub-репозиторій: `https://github.com/mova-compact/mova-spec`.
- Репозиторій зберігає канонічну специфікацію MOVA 5.0.0: JSON-схеми, нормативні тексти та приклади.
- Джерело істини для red-core сутностей (`ds.*`, `env.*`, `global.*`) міститься в `schemas/` і `docs/`.
- Тут немає виконуваного коду: це каталог контрактів, а не платформа чи агенти.
- Репозиторій підтримується як source-of-truth для схем/доків (без публічного npm-publish workflow).
- Поточна версія — 5.0.0; архів 4.0.0 у `docs/archive/4.0.0/` залишено для історії.
- Приклади вхідних і вихідних документів у `examples/` допомагають зрозуміти структуру даних.
- Валідність схем перевіряється локально через `npm test` (Ajv 2020-12); локальна перевірка перед комітом обов'язкова.
- Імена файлів документації з префіксом `mova_4.1.1_*` збережені для стабільності шляхів, але канонічна версія продукту — 5.0.0.
- README дає навігацію; нормативні тексти розміщені в `docs/`.
- Примітки до мажорного оновлення: `docs/MOVA_5.0.0_RELEASE_NOTES.md`.
- Агентні контракти винесені в окремий профіль: `https://github.com/mova-compact/mova-agent-profile`.
- Локальний валідатор і схеми знаходяться в цьому репозиторії; це не SDK і не рантайм.
- Зворотний зв'язок і зміни — через Issues/PR; ядро залишається під контролем автора.

## Швидкий старт
1. Перегляньте цей README, щоб зрозуміти мету та склад репозиторію.
2. Відкрийте `docs/mova_4.1.1_core.md` і `docs/mova_4.1.1_global_and_verbs.md` для базової моделі та каталогу дієслів.
3. Подивіться схеми в `schemas/` і відповідні приклади в `examples/`, щоб побачити фактичні JSON-структури.
4. Запустіть `npm test`, щоб підтвердити валідність схем у своєму середовищі.
5. Для історії ознайомтеся з архівом `docs/archive/4.0.0/` (не змінюйте його вміст).

## Перевірка JSON

Скористайтеся локальним CLI цього репозиторію для валідації документів за схемами MOVA:

```bash
# Валідація прикладу конверта за $id схеми
node bin/mova-validate.mjs --schema https://mova.dev/schemas/env.instruction_profile_publish_v1.schema.json examples/env.instruction_profile_publish_v1.example.json

# Валідація за іншим $id схеми
node bin/mova-validate.mjs --schema https://mova.dev/schemas/ds.mova_episode_core_v1.schema.json examples/env.security_event_store_v1.example.json

# Валідація за локальним файлом схеми
node bin/mova-validate.mjs --schema schemas/ds.mova_schema_core_v1.schema.json examples/mova4_core_catalog.example.json
```

### Посилання
- [docs/](docs/)
- [schemas/](schemas/)
- [examples/](examples/)
- [docs/MOVA_4.1.1_RELEASE_NOTES.md](docs/MOVA_4.1.1_RELEASE_NOTES.md)
