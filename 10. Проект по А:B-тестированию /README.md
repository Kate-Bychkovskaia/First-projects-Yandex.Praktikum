# Проект по А/В тестированию
## Описание проекта:
- В интернет-магазине был проведен А/В тест изменений, связанных с внедрением улучшенной рекомендательной системы. Группа А - контрольная, группа В-экспериментальная (новая платежная воронка). В нашем распоряжении есть датасет с действиями пользователей, техническое задание и 3 вспомогательных датасета. Необходимо оценить корректность проведения теста и проанализировать его результаты.
- Показатель положительного эффекта - за 14 дней с момента регистрации в системе пользователи группы В покажут улучшение каждой метрики не менее, чем на 10% по сравнению с группой А. Метрики для оценки - конверсии в просмотр товара/корзины/покупку.
## Цель исследования:
Оценить эффективность новой улучшенной рекомендательной системй (группа В) по сравнению со старой (группа А). 
## Ход исследования:
1. Загрузка датасетов и обзор данных
2. Подготовка данных к анализу (поиск дубликатов, пропусков, выбросов)
3. Оценка корректности проведения теста
4. Исследовательский анализ данных
5. Оценка результатов A/B-тестирования
6. Вывод
## Результаты исследования:
**Выявленные несоответствия ТЗ**

В тесте менее 15% пользователей из региона EU, а если точнее 13,17%
Пользователей 5575 вместо 6000
Не хватает логов по событиям, вместо 4 января, в наших логах данные до 30.12
Тест совпадает с маркетинговым событием Christmas&New Year Promo, которое шло с 25 декабря. Влияния на наш тест замечено не было, но пересечение с маркетинговым событием для чистоты теста не очень хорошо.
Регистрации по группам распределены равномерно в обеих группах, есть всплески в обеих группах 7, 14 и 21 декабря
Лайфтайм пользователя: пользователи совершают целевое действие меньше чем за 8 дней, чаще в первые 3 дня.

**Аудитории 2х групп А и В**

Распределения по группам не равномерные, если учитывать регистрации в группе А - 3195, В - 2380. Но это не критично. Критично то, что 70% пользователей из группы В зарегистрировалились и не совершили не одного события. И получается, что по анализу воронки, из наших групп отваливается большой процент неактивных пользователей. В итоге если считать только тех пользователей, которые совершили хотя бы какое - то действие по группам сильно снижается в тестовой группе. Распределение получается следующее: группа А - 2279, группа В - 761. А это уже сильная неравномерность.
Во время нашего теста проводился другой тест interface_eu_system, 1602 пользователя (24%) пользователей нашего теста одновремеенно оказались в конкурирующем тесте, при этом группы А и В пересеклись между собой. В ходе обработки из группы А нашего теста были удалены пользователи группы В конкурирующего теста, и из нашей В - удалены пересечения с А другого теста.

**Результаты исследовательского анализа**

Если считать по очищенному от неактивных пользователей датасету, то разница по среднему числу событий на 1 пользователя у А и В - небольшая. 7 в А, 6 в В
Распредление числа событий не равномерное. Группа В общее к-во событий 4785, в группе А 15726.
Количество событий/пользователя в группе В вариьируется от 30 до 150. Спад после 21 декабря. У группы А от 100 до 750 событий на пользователя. В период с 7 по 13 декабря группы А и В шли на равне, но потом пользователи группы В остались на своем уровне, а у А показатели резко выросли
А если считать конверсию из шага в шаг, то воронка В показывает лучше результат конверсии из просмотра страницы продукта в покупку. Результаты CR из просмотра продукта в заказ у группы В - 51% у группы А - 32%. CR из логина в заказ у В - 28%, группы А-32%
Если смотреть конверсию из регистрации (login), то у группы А 71% пользователей пошли дальше по воронке, у группы В-31%. До заказа дошли у А 23% из зарегистрировавшихся, а у В- только 9%. Если смотреть конверсию из логина, то группа А показывает лучше результаты с разницей от 0,1 до 8%, по сравнению с В. Самая большая разница на этапе product_page - у А cr выше на 8%, чем у В.

**Результаты статистического теста**

Статистический тест показал, что статистически значимых различий между группами А и В есть только на этапе product_page. При анализе-у группы А по этому этапу был самый лучший показатель по сравнению с группой В - 8%.
Но доверять тесту не стоит, так как он не соответсвует условиям ТЗ и самое главное в тестовой группе В недостаточно пользователей для полноценной оценки. Поэтому мощность теста низкая.
Тест необходимо дополнить догами с 30 декабря по 4 января, так как у группы В большая часть пользователей зарегистрировалась и не показала конверсий дальше, поэтому возможно с данными за нехватающие дни удастся набрать достаточное количество пользователей, либо проверить техническое состояние, возможно пользователи отвалились после регистрации по техническим причинам. Если и в логах с 30.12 по 4 января не будет достаточного количества, и не найдено технических ошибок, тогда тест можно продолжить, так группа В показала рост более чем в 10% по CR из шага в шаг и низкую конверсию и средняя стоимость ее заказов выше, чем у группы А. Без этого на результаты теста полагаться нельзя.

**Заключение и рекомендации**

Выявлено множество несоответствий условий проведения теста, поэтому тест признать успешным нельзя. Тестирование проведено не корректно.
Необходимо, чтобы все логи с 30 декабря до 4 января загрузились, может быть события пользователей с 0 конверсий могут быть в этих датах, иначе мощность теста с такими выборками сильно падает (из-за недостаточности пользователей, из-за сильно маленькой группы В)
