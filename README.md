# Поиск одинаковых товаров на маркетплейсах

Решение команды DeviAⁱnts для соревнования [ЛИДЕРЫ ЦИФРОВОЙ ТРАНСФОРМАЦИИ](https://leaders2023.innoagency.ru)

## Препроцессинг данных
Мы использовали следующие стратегии для препроцессинга данных:

#### Полный текст о товаре
Объединение всех атрибутов товара в один текст, включая названия, категории и цвета.

#### Различия полного текста
Взятие только различающихся атрибутов при сравнении товаров. Идея в том, чтобы обучать модель только на разнице товаров, так как одинаковые атрибуты не приносят большого количества информации.

#### Получение полного текста с выделенными различиями
Полный текст, но различающиеся атрибуты помечены, а также вынесены выше, чтобы избежать обрезания текста во время токенизации.

## Использованные модели
Наше решение использует следующие модели:
* `RuBERT Base`
* `XLM-RoBERTa Base`
* `Quora DistilBERT`
* `CatBoost`

`RuBERT` и `XLM-RoBERTa` используются для бинарной классификации товаров на дубликаты/разные товары. На вход им передаются тексты обоих товаров. (Здесь представлено решение, давшее наибольший скор, в нём `XLM-RoBERTa` была обучена на задачу регрессии на различиях полного текста).

`DistilBERT` используется для задачи `Sentence Similarity`. С помощью этой модели создаются эмбеддинги двух текстов, расстояние между которым мы ищем. Была взята модель, предобученная на датасете `Quora Duplicate Questions`, в котором была поставлена задачи находить дупликаты вопросов на популярной платформе `Quora`.

`CatBoost` использовался как stacking model. На вход модели подавались результаты предсказания других моделей: предсказания `RuBERT` и `XLM-RoBERTa`, расстояния между эмбеддингами `DistilBERT`, а также расстояния между эмбеддингами главных изображений товаров.


## Файлы

[`Working_with_Dataset.ipynb`](Working_with_Dataset.ipynb) - Первоначальная обработка данных, сбор полных текстов  <a target="_blank" href="https://colab.research.google.com/github/mathewpolonsky/Marketplace-Item-Matching/blob/main/Adding_Text_Difference.ipynb"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

[`Adding_Text_Difference.ipynb`](Adding_Text_Difference.ipynb) - Добавления разниц текстов для датасета <a target="_blank" href="https://colab.research.google.com/github/mathewpolonsky/Marketplace-Item-Matching/blob/main/Working_with_Dataset.ipynb"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

[`Training_Distilbert_Duplicates.ipynb`](Training_Distilbert_Duplicates.ipynb) - Обучение `DistilBERT` и получение расстояний между эмбеддингами для обучения `CatBoost` <a target="_blank" href="https://colab.research.google.com/github/mathewpolonsky/Marketplace-Item-Matching/blob/main/Training_Distilbert_Duplicates.ipynb"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

[`Training_XLM_Roberta_Regression_on_Differences.ipynb`](Training_XLM_Roberta_Regression_on_Differences.ipynb) - Обучение `XLM-RoBERTa` на задачу регрессии и получение предсказаний для обучения `CatBoost` <a target="_blank" href="https://colab.research.google.com/github/mathewpolonsky/Marketplace-Item-Matching/blob/main/Training_XLM_Roberta_Regression_on_Differences.ipynb"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

[`Training_ruBert_Classification.ipynb`](Training_ruBert_Classification.ipynb) - Обучение `RuBERT` на задачу классификации и получение предсказаний для обучения `CatBoost` <a target="_blank" href="https://colab.research.google.com/github/mathewpolonsky/Marketplace-Item-Matching/blob/main/Training_ruBert_Classification.ipynb"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

[`Training_CatBoost.ipynb`](Training_CatBoost.ipynb) - Обучение `CatBoost` на предсказаниях `RuBERT` и `XLM-RoBERTa`, расстояниях между эмбеддингами `DistilBERT`, расстояниях между эмбеддингами главных изображений товаров <a target="_blank" href="https://colab.research.google.com/github/mathewpolonsky/Marketplace-Item-Matching/blob/main/Training_ruBert_Classification.ipynb"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

[`Making_Submit.ipynb`](Making_Submit.ipynb) - Получение сабмита <a target="_blank" href="https://colab.research.google.com/github/mathewpolonsky/Marketplace-Item-Matching/blob/main/Making_Submit.ipynb"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

[`Adding_Differerences2FullText.ipynb`](Adding_Differerences2FullText.ipynb) - Получение полного текста с выделенными различиями <a target="_blank" href="https://colab.research.google.com/github/mathewpolonsky/Marketplace-Item-Matching/blob/main/Adding_Differerences2FullText.ipynb"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>
