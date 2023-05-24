# rx-template-upload-data-ui
Репозиторий с шаблоном разработки «Загрузка данных из xlsx-файлов с обложки».

## Описание
Шаблон позволяет перенести данные в Directum RX. 
В качестве источника данных для переноса используются книги Excel с расширением XLSX.
Чтобы произвести создание записей справочников в Directum RX, достаточно заполнить специально сформированные шаблоны Excel и загрузить данные из обложки.
Шаблон отличается от утилиты https://github.com/DirectumCompany/rx-util-importdata-net-core тем, что позволяет загружать записи с обложки модуля любым сотрудником (если есть права на справочники). Также есть различия в перечне загружаемых справочников. 
После каждой загрузки строиться отчет об успешно/неуспешно загруженных записях.

### На текущий момент реализована возможность импорта записей следующих справочников:
1. Федеральный классификатор обращений граждан (разделы, темы, тематики, темы, вопросы, подвопросы).
2. Номенклатура дел (сроки хранения, номенклатура дел).
3. Орг. структура.

   3.1. Учетные записи.
   
   3.2. Персоны.
   
   3.3. Организации.
   
   3.4. Наши организации.
   
   3.5. Подразделения.
   
   3.6. Должности.
   
   3.7. Работники.
   
5. Населенные пункты с ГУИД ФИАС (из ФИАС (ГАР)). 

<b>Примечание</b>. Для того, чтобы в справочник населенные пункты загрузился ГУИД ФИАС, необходимо перекрыть (если он еще не перекрыт) справочник City модуля Commons и добавить новое свойство:
![image](https://github.com/DirectumCompany/rx-template-uploaddata-from-cover/assets/87016932/cbeb9c73-3ee4-4ca2-940c-e4c9f8b6d8d8)

### Общий вид:
![image](https://github.com/DirectumCompany/rx-util-uploaddata-from-cover/assets/87016932/01146610-7894-4e88-9939-748e1688cc36)

### Шаблоны для загрузки записей справочников (кроме шаблона для загрузки населенных пунктов)
Шаблоны можно найти в папке [Шаблоны](https://github.com/DirectumCompany/rx-template-upload-data-ui/tree/main/Template).

### Порядок загрузки населенных пунктов из ФИАС (ГАР).
1.	По ссылке https://fias.nalog.ru/Updates на вкладке ГАР БД ФИАС необходимо выполнить экспорт последней полной версии XML, находящейся в правом столбце, в формате .zip.
![image](https://github.com/DirectumCompany/rx-util-uploaddata-from-cover/assets/87016932/113a89d7-c0b5-4ef5-aceb-df86c2b237b4)
2. В экспортируемой папке находим по региону область/республику, в которую планируется импорт. Например, код Владимирской области – 33.
![image](https://github.com/DirectumCompany/rx-util-uploaddata-from-cover/assets/87016932/183e15f7-11bd-475c-8f2f-59e22341ee7c)
3.	Находящийся в папке документ в формате XML, наименование которого начинается с «AS_ADDR_OBJ_<год><месяц><день><гуид>», необходимо переместить из архивированной папки (например, на рабочий стол).
![image](https://github.com/DirectumCompany/rx-util-uploaddata-from-cover/assets/87016932/99d6aacb-2fb3-4b0c-9722-f5342cb226b5)
4. После перемещения документ необходимо открыть с помощью Excel, выбрав способ открытия файла XML-таблица.
![image](https://github.com/DirectumCompany/rx-util-uploaddata-from-cover/assets/87016932/37a2ca81-41ab-4d05-bca4-f4a99543223f)
5.	В сформировавшейся таблице в столбце Level выбираются уровни:
  
  • 5 - город;
  
  • 6 - населенный пункт.
  
<b>Примечания</b>. Отмеченные уровни импортируются в систему. Остальные уровни с шаблона необходимо удалить. Если шаблон большой, то рекомендуется разбить его на несколько примерно по 5000 записей. данное ограничение связано с там, что для веб-клиента в системе Directum RX стоит таймаут - 2 мин.
![image](https://github.com/DirectumCompany/rx-util-uploaddata-from-cover/assets/87016932/3d3a6ac1-474b-4527-8fa5-e643570d5f6c)

6.	Получившийся документ необходимо пересохранить в формате xlsx.

7.	В системе в модуле <b>Загрузка данных</b> по гиперссылке <i>Загрузить населенные пункты</i> в полях:
![image](https://github.com/DirectumCompany/rx-util-uploaddata-from-cover/assets/87016932/63296f5c-1439-4d59-a08b-115d2e2e15fd) 
  
  • Файл* – добавляется документ с населенными пунктами;
  
  • Страна* – Российская Федерация;
  
  • Регион* – регион заказчика (например, Владимирская область).
  
7.	В модуле <b>Загрузка данных</b> по гиперссылке Населенные пункты в соответствующем регионе появятся импортированные данные.

<b>Примечание</b>. Сейчас в модуле <b>Загрузка данных</b> в справочнике Населенные пункты часть населенных пунктов внесена в систему, поэтому дубль необходимо удалить. Если населенный пункт уже использовался в Системе, то необходимо из импортированного населенного пункта скопировать ИД в ФИАС, добавить его в аналогичное поле населенного пункта, который был уже использован, после чего удалить импортированный дубль из системы.

## Порядок установки
Для работы требуется:
1. Установленный Directum RX версии 4.4 и выше.
2. Установленное решение "Обращения граждан" ( если требуется загрузка справочников федерального классификатора обращений граждан).

### Установка для ознакомления
1. Склонировать репозиторий https://github.com/DirectumCompany/rx-template-upload-data-ui.git в папку.
2. Указать в _ConfigSettings.xml DDS:
```xml
<block name="REPOSITORIES">
  <repository folderName="Base" solutionType="Base" url="" /> 
  <repository folderName="<Папка из п.1>" solutionType="Work" 
     url="https://github.com/DirectumCompany/rx-template-upload-data-ui.git" />
</block>
```
### Установка для использования на проекте

Возможные варианты:

#### A. Fork репозитория.
1. Сделать fork репозитория rx-template-upload-data-ui для своей учетной записи.
2. Склонировать созданный в п. 1 репозиторий в папку.
3. Указать в _ConfigSettings.xml DDS:
``` xml
<block name="REPOSITORIES">
  <repository folderName="Base" solutionType="Base" url="" /> 
  <repository folderName="<Папка из п.2>" solutionType="Work" 
     url="<Адрес репозитория gitHub учетной записи пользователя из п. 1>" />
</block>
```
#### B. Подключение на базовый слой.
Вариант не рекомендуется, так как при выходе версии шаблона разработки не гарантируется обратная совместимость.
1. Склонировать репозиторий https://github.com/DirectumCompany/rx-template-upload-data-ui.git в папку.
2. Указать в _ConfigSettings.xml DDS:
```xml
<block name="REPOSITORIES">
  <repository folderName="Base" solutionType="Base" url="" /> 
  <repository folderName="<Папка из п.1>" solutionType="Base" 
     url="https://github.com/DirectumCompany/rx-template-upload-data-ui.git" />
  <repository folderName="<Папка для рабочего слоя>" solutionType="Work" 
     url="<Адрес репозитория для рабочего слоя>" />
</block>
```

#### C. Копирование репозитория в систему контроля версий.
Рекомендуемый вариант для проектов внедрения.
1. В системе контроля версий с поддержкой git создать новый репозиторий.
2. Склонировать репозиторий https://github.com/DirectumCompany/rx-template-upload-data-ui.git в папку с ключом --mirror.
3. Перейти в папку из п. 2.
4. Импортировать клонированный репозиторий в систему контроля версий командой:
git push –mirror <Адрес репозитория из п. 1>
