![Python](https://shields.io)
![Pandas](https://shields.io)
![Seaborn](https://shields.io)

# Job Market Analysis

Проект для анализа рынка труда и вакансий. Находится в разработке.

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
