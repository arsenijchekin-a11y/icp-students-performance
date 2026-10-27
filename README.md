# Анализ факторов успеваемости учащихся

Цифровой проект по дисциплине ИЦП, 3 курс.

Автор: Чекин Арсений Витальевич, группа ОБ-АД-24, РАНХиГС

## Ноутбуки

| Этап | Ноутбук | Открыть |
|---|---|---|
| Этап 2. Аудит и подготовка данных | `notebooks/stage2_audit.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/arsenijchekin-a11y/icp-students-performance/blob/main/notebooks/stage2_audit.ipynb) |
| Этап 3. Часть 1 - Анализ данных | `notebooks/stage3_part1_eda.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/arsenijchekin-a11y/icp-students-performance/blob/main/notebooks/stage3_part1_eda.ipynb) |
| Этап 3. Часть 2 - Предобработка | `notebooks/stage3_part2_preprocessing.ipynb` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/arsenijchekin-a11y/icp-students-performance/blob/main/notebooks/stage3_part2_preprocessing.ipynb) |

## Датасет

**Students Performance in Exams** - Kaggle, автор spscientist
https://www.kaggle.com/datasets/spscientist/students-performance-in-exams

1000 учащихся, 8 столбцов: пол, этническая группа, образование родителей, тип питания, курс подготовки к экзамену и оценки по математике, чтению и письму. Целевая переменная - оценка по математике (`math score`).

## Структура репозитория

```
data/
  raw/StudentsPerformance.csv          - исходные данные (не изменяются)
  processed/students_clean.csv         - данные после аудита (Этап 2)
  processed/train_processed.csv        - обучающая выборка после предобработки (Этап 3)
  processed/test_processed.csv         - тестовая выборка после предобработки (Этап 3)
notebooks/
  stage2_audit.ipynb                   - Этап 2: аудит и подготовка данных
  stage3_part1_eda.ipynb               - Этап 3, часть 1: анализ данных и гипотезы
  stage3_part2_preprocessing.ipynb     - Этап 3, часть 2: масштабирование и предобработка
```

## Этап 2. Аудит и подготовка данных

Ноутбук `notebooks/stage2_audit.ipynb` проверяет:

- характеристику признаков (смысл, тип, правило);
- типы данных и формат значений;
- пропуски, включая особые маркеры;
- полные дубликаты;
- единообразие категорий;
- диапазоны (0-100), выбросы по IQR и аномальные записи;
- согласованность оценок между собой;
- баланс классов целевой переменной;
- единый профиль исходных данных (все проверки в одной таблице) и приоритеты.

**Главные результаты:** пропусков, дублей и значений вне диапазона нет. Найдены 12 строк с низкими выбросами (сохранены с флагом `is_outlier`), сильная корреляция между оценками по чтению и письму (0.95) и умеренный дисбаланс классов 68/32 при пороге «сдал» 60 баллов. Данные пригодны для дальнейшего анализа.

## Этап 3. Исследовательский анализ и признаки

**Часть 1 - Анализ данных** (`stage3_part1_eda.ipynb`):

- очистка от дубликатов, пропусков и логических ошибок с обоснованием;
- случайная выборка 25% (250 учеников) для анализа;
- графики, среднее и медиана по признакам;
- 3 гипотезы, сформулированные на 25% и проверенные t-тестом Уэлча на оставшихся 75%.

Все три гипотезы подтвердились (p < 0.05): подготовка к экзамену и стандартное питание связаны с более высокими баллами; мальчики в среднем лучше в математике, девочки - в чтении и письме.

**Часть 2 - Предобработка** (`stage3_part2_preprocessing.ipynb`):

- сравнение MinMaxScaler, StandardScaler и RobustScaler на `reading_score` с графиками до и после;
- выбран RobustScaler как устойчивый к выбросам;
- разделение train/test 80/20 со стратификацией и предобработка всех признаков без утечки данных: RobustScaler для оценок, порядковое кодирование образования родителей, one-hot для остальных категорий.

## Как запустить

Проще всего - нажать кнопку **Open in Colab** у нужного ноутбука. Ноутбук откроется с уже выполненными результатами; при запуске заново («Среда выполнения → Выполнить все») данные подгрузятся с GitHub автоматически.

Локально:

```
pip install pandas numpy matplotlib scipy scikit-learn jupyter
cd notebooks
jupyter notebook
```
