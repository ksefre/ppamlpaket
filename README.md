# Определение настроения твита

## Задача

**Бизнес-задача:** нужно научиться определять эмоциональный окрас текста: негативный, нейтральный, позитивный.

**ML-задача:** в датасете [sentiment140](https://www.kaggle.com/datasets/kazanova/sentiment140)
1.6 миллионов твитов, каждому из которых проставлена метка класса, необходимо сделать классификацию.


![5.png](https://github.com/ksefre/ppamlpaket/blob/main/image/5.png)

Класса получается всего 2, поэтому мы будем делать бинарную классификацию. Заменим метку класса 4 на 1.

![2.png](https://github.com/ksefre/ppamlpaket/blob/main/image/2.png)
![1.png](https://github.com/ksefre/ppamlpaket/tree/main/image)
![3.png](https://github.com/ksefre/ppamlpaket/blob/main/image/3.png)
![4.png](https://github.com/ksefre/ppamlpaket/blob/main/image/4.png)

У разных классов самые частотные слова по большей части совпадают, а средняя длина твитов почти не отличается, поэтому ориентироваться на эти параметры при классификации не стоит.

Сведем эмбеддинги твитов в 2d-плоскость:
![img.png](https://github.com/ksefre/ppamlpaket/blob/main/image/img.png)
Можно видеть, что классы перемешаны и линейно неразделимы.

## Метрики

Можно использовать классические для бинарной классификации метрики: accuracy, precision, recall, f1.
Больше ориентироваться нужно на accuracy, так как мы хотим максимизировать правильную классификацию и позитивных, и
негативных твитов. 
> Если бы мы классифицировали отзывы на продукт, то нам было бы важнее находить все отрицательные
отзывы, поэтому мы бы максимизировали recall, но в данном случае accuracy будет достаточно.

## Результаты

[//]: # (В ноутбуке [sentiment_classification.ipynb]&#40;sentiment_classification.ipynb&#41; находится препроцессинг данных, преобразование текста в эмбеддинги, а также запуск константного классификатора и логистической регрессии.)

[//]: # ()
[//]: # (В ноутбуке [random_forest.ipynb]&#40;random_forest.ipynb&#41; производится подбор гиперпараметров для случайного леса.)

Для ускорения перебора параметров для случайного леса бралась обрезанная до 10000 трейн выборка.

| Классификатор                    | Accuracy | 
|----------------------------------|----------|
| DummyClassifier                  | 0.5      |
| LogisticRegression               | 0.6      |
| RandomForest HalvingGridSearchCV | 0.69     |
| RandomForest optuna              | 0.7      |

Так как мы работали с эмбеддингами, то интерпретировать модель сложно: нельзя узнать по каким признакам происходит классификация.
