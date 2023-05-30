# Поиск одинаковых товаров на маркетплейсах

Решения от команды DeviAⁱnts для соревнования [ЛИДЕРЫ ЦИФРОВОЙ ТРАНСФОРМАЦИИ](https://leaders2023.innoagency.ru)

## Использованные модели
Наше решение использует следующие модели:
* `RuBert`
* `XLM-RoBERTa`
* `Distilbert`
* `CatBoost`

`Rubert` и `XLM-RoBERTa` используются для бинароной классификации товаров на дубликаты / разные товары. На вход им передаются тексты обоих товаров.

`Distilbert` используется для задачи `sentence similarity`. С помощью модели создаются эмбединги обоих текстов и ищется расстояние между ними. Была взята модель предобученная на датасете `Quora Duplicate Questions`, в котором была поставлена задачи находить дупликаты вопросов на популярной платформе `Quora`

`CatBoost` использовался для стакинга моделей. На вход модели подавались результаты 
предсказания других моделей: результаты пердсказаний `Rubert` и `XLM-RoBERTa`, расстояния `Distilbert`, а также расстояния между эмбедингами главных изображений товаров.

## Препроцессинг данных
Мы использовали следующие стратегии для препроцессинга данных

#### Полный текст
Объединение всех атрибутов товара в один текст, включая названия, категории и цвета

#### Различия
Взятие только различающихся атрибутов при сраввнении товаров. Идея в том, чтобы обучать модель только на разнице в товарах, так как одинаковые атрибуты не приносят большого количества информации

#### Полный текст с различиями
Полный текст, но различающиеся атрибуты помечены, а также вынесены выше, чтобы избежать обрезания текста во время токенизации

## Файлы


[`Working_with_Dataset.ipynb`](Working_with_Dataset.ipynb) - Первоначальная обработка данных, сбор полных текстов  <a target="_blank" href="https://colab.research.google.com/github/mathewpolonsky/Marketplace-Item-Matching/blob/main/Adding_Text_Difference.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

[`Adding_Text_Difference.ipynb`](Adding_Text_Difference.ipynb) - Добавления разниц текстов для датасета <a target="_blank" href="https://colab.research.google.com/github/mathewpolonsky/Marketplace-Item-Matching/blob/main/Working_with_Dataset.ipynb"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>
