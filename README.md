![Python](https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue)
![Pandas](https://img.shields.io/badge/Pandas-2C2D72?style=for-the-badge&logo=pandas&logoColor=white)
![Seaborn](https://shields.io)
![SciPy](https://img.shields.io/badge/SciPy-654FF0?style=for-the-badge&logo=SciPy&logoColor=white)

<!--![Plotly](https://img.shields.io/badge/Plotly-239120?style=for-the-badge&logo=plotly&logoColor=white)

-->

# Job Market Analysis

Проект для анализа рынка труда и вакансий. Находится в разработке.




<details>
<summary>📦 Шаг 1: Окружение и импорт библиотек</summary>

Для выполнения исследования в проекте используются библиотеки `pandas`, `seaborn`, `scipy` и `statsmodels`.

### Код импорта:
```python
# Загрузка, обработка таблиц и подготовка выборок данных
import pandas as pd

# Визуализация данных, построение графиков распределений и боксплотов
import seaborn as sns

# Модуль базовых статистических тестов библиотеки SciPy
from scipy import stats

# Построение линейной модели для ANOVA (дисперсионного анализа)
from statsmodels.formula.api import ols

# Расчет таблицы дисперсионного анализа (ANOVA) по построенной модели
from statsmodels.stats.anova import anova_lm

# Инструменты statsmodels (для построения графиков квантилей QQ-plot)
import statsmodels.api as sm

# Проведение критерия Тьюки (Post-hoc анализ для попарных сравнений)
from statsmodels.stats.multicomp import pairwise_tukeyhsd

# Функции для корреляционного анализа (коэффициенты Пирсона, Спирмена, Кендалла)
from scipy.stats import pearsonr, spearmanr, kendalltau
```

### Установка пакетов:
```bash
pip install pandas seaborn scipy statsmodels
```
</details>


<details>
<summary>❓ Шаг 2: Ключевые вопросы исследования</summary>

В рамках проекта необходимо проанализировать данные и ответить на следующие аналитические вопросы:

1. **Факторы зарплат:** Что сильнее всего влияет на зарплатную вилку вакансий в сфере Data Science?
2. **Распределение грейдов:** Как распределены позиции (грейды) сотрудников в компаниях разного размера? Какие выводы можно сделать на основе этого распределения?
3. **Выбор компании:** В компанию какого размера выгоднее устраиваться кандидатам уровней *Middle* и *Senior*?

---
### 🛠️ Методология анализа
Для поиска обоснованных ответов на эти вопросы в работе последовательно выполняются:
* **Описательный анализ данных** (расчет базовых статистик, мер центральной тенденции и разброса).
* **Корреляционный анализ** (оценка тесноты и направления связей между числовыми признаками).
* **Дисперсионный анализ (ANOVA)** (сравнение средних зарплат и выявление статистически значимых различий между группами факторов).
</details>

<details>
<summary>📊 Шаг 3: Загрузка и предобработка данных</summary>

На этом этапе выполняется загрузка датасета и очистка от технического столбца с индексами с помощью параметра `index_col=0`.

### Код загрузки:
```python
# Читаем файл и сразу убираем первый безымянный столбец с индексами
data = pd.read_csv("datas/ds_salaries.csv", index_col=0)

# Проверяем структуру загруженных данных перед тестами
data.head()
```

### Структура загруженных данных (`data.head()`):

| work_year | experience_level | employment_type | job_title | salary | salary_currency | salary_in_usd | employee_residence | company_location | company_size |
| :---: | :---: | :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| 2020 | MI | FT | Data Scientist | 70000 | EUR | 79833 | DE | DE | L |
| 2020 | SE | FT | Machine Learning Scientist | 260000 | USD | 260000 | JP | JP | S |
| 2020 | SE | FT | Big Data Engineer | 85000 | GBP | 109024 | GB | GB | M |
| 2020 | MI | FT | Product Data Analyst | 20000 | USD | 20000 | HN | HN | S |
| 2020 | SE | FT | Machine Learning Engineer | 150000 | USD | 150000 | US | US | L |

</details>

<details>
<summary>🔍 Шаг 4: Определение типов признаков (Классификация переменных)</summary>

На этом этапе проводится анализ структуры данных с помощью `data.info()`, проверяются уникальные значения и выполняется классификация каждого признака для корректного подбора статистических критериев (параметрических и непараметрических).

### Код анализа типов данных:
```python
# Получение общей информации о пропусках и типах данных в столбцах
data.info()

# Просмотр уникальных значений для классификации признаков
print("Года:", data['work_year'].unique())
print("Грейды:", data['experience_level'].unique())
print("Размеры компаний:", data['company_size'].unique())
print("Профессии (первые 5):", data['job_title'].unique()[:5])
```

### 📋 Классификация переменных в датасете:

* **Непрерывные переменные (Continuous):**
  * `salary`, `salary_in_usd` — уровень заработной платы (используется для оценки средних значений в ANOVA и корреляционного анализа).

* **Дискретные переменные (Discrete):**
  * `work_year` — год выплаты зарплаты (2020, 2021, 2022 и т.д.).

* **Порядковые переменные (Ordinal):**
  * `experience_level` — уровень квалификации сотрудника (EN, MI, SE, EX), где важен внутренний порядок.
  * `company_size` — масштаб компании (S, M, L), отражающий упорядоченный размер бизнеса.

* **Категориальные / Номинальные переменные (Categorical / Nominal):**
  * `job_title` — наименование вакансии / позиции.
  * `salary_currency` — валюта, в которой выплачивается зарплата (USD, EUR, GBP и др.).
  * `employment_type` — тип занятости (FT, PT и др.).
  * `employee_residence`, `company_location` — географическое положение.

</details>















для сохранения картинки 
plt.savefig('images/salary_chart.png', dpi=300, bbox_inches='tight')

для вставки картинки
![График зарплат](images/salary_chart.png)

> 💡 **Ключевой инсайт:** Вывод....



# Настраиваем минималистичный стиль и крупный читаемый шрифт
sns.set_theme(style="whitegrid", context="notebook", font_scale=1.1)

# Задаем свою стильную палитру (Глубокий синий, Серый, Коралловый акцент)
my_palette = ["#1a365d", "#718096", "#e53e3e"]
sns.set_palette(my_palette)

# Убираем рамки графика сверху и справа для чистоты
plt.rcParams['axes.spines.top'] = False
plt.rcParams['axes.spines.right'] = False

### 📊 Пример таблицы

| Грейд | Топ-1 Навык | Опыт работы | Формат работы |
| :--- | :--- | :--- | :--- |
| **Junior** | Python (Pandas) | 0–1 год | Чаще офис / Гибрид |
| **Middle** | SQL + Scipy | 1–3 года | Удаленка / Гибрид |
| **Senior** | Statsmodels (A/B) | 3+ лет | Полная удаленка |


<details>
<summary>🛠️ <b>Инструкция по развертыванию проекта (Нажмите, чтобы раскрыть)</b></summary>

1. Клонируйте репозиторий.
2. Создайте папку `images` в корне проекта.
3. Установите зависимости:
   ```bash
   pip install pandas seaborn scipy statsmodels
   ```
</details>
