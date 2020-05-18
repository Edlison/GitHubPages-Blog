# KNN-鸢尾花数据集


```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn import datasets
```

#### 使用sklearn本地数据集中的鸢尾花数据集


```python
iris = datasets.load_iris()
iris.keys()
```




    dict_keys(['data', 'target', 'target_names', 'DESCR', 'feature_names', 'filename'])




```python
X = iris.data
```


```python
Y = iris.target
```


```python
X.shape
```




    (150, 4)




```python
Y.shape
```




    (150,)



# 手动分离测试数据


```python
shuffled_indexes = np.random.permutation(len(X))
shuffled_indexes
```




    array([ 47,  10, 144,  57, 135,  58,  36, 136,  73, 109,  64,  15, 104,
           143, 108,  83,  23, 126, 125, 131, 137,  22,  16,  29, 118,  31,
            33,  52,  32, 132,  45,  38,  78, 139,  30,  37,  61,  97, 122,
            56, 107,  66, 114,  87,  43,  76,  84,  79, 142,  70,  77,  42,
             7, 138, 141, 120, 129,  44,  24,  53, 116,  13,  91, 119,  93,
             6,  60,  50,  67,  20,  54,  71,  89,  68,  21, 133, 148,  81,
            25,  48, 130, 127,  28,  90,  82, 146, 100, 105,  80,  94,  14,
            55, 111, 106, 101, 103,  35,  99,   3,  26,  69, 124,  95,  96,
           140,  46,  19,  34,  75,  59,   1, 117, 121,  49, 110,   0, 115,
            72,   9,  18, 149,  40, 145,  92, 123,  51,   4,  11,  39,  85,
            62, 147, 102,  74,   2,  86,   8,  17,   5,  27, 112, 128,  12,
            65,  41, 134,  63,  88, 113,  98])




```python
test_ratio = 0.2
test_size = int(len(X) * test_ratio)
```


```python
test_indexes = shuffled_indexes[:test_size]
train_indexes = shuffled_indexes[test_size:]
```


```python
X_train = X[train_indexes]
Y_train = Y[train_indexes]

X_test = X[test_indexes]
Y_test = Y[test_indexes]
```


```python
print(X_train.shape);print(Y_train.shape);print(X_test.shape);print(Y_test.shape)
```

    (120, 4)
    (120,)
    (30, 4)
    (30,)


# 使用sklearn分离


```python
from sklearn.model_selection import train_test_split
train_test_split?
```


    [0;31mSignature:[0m [0mtrain_test_split[0m[0;34m([0m[0;34m*[0m[0marrays[0m[0;34m,[0m [0;34m**[0m[0moptions[0m[0;34m)[0m[0;34m[0m[0;34m[0m[0m
    [0;31mDocstring:[0m
    Split arrays or matrices into random train and test subsets
    
    Quick utility that wraps input validation and
    ``next(ShuffleSplit().split(X, y))`` and application to input data
    into a single call for splitting (and optionally subsampling) data in a
    oneliner.
    
    Read more in the :ref:`User Guide <cross_validation>`.
    
    Parameters
    ----------
    *arrays : sequence of indexables with same length / shape[0]
        Allowed inputs are lists, numpy arrays, scipy-sparse
        matrices or pandas dataframes.
    
    test_size : float, int or None, optional (default=None)
        If float, should be between 0.0 and 1.0 and represent the proportion
        of the dataset to include in the test split. If int, represents the
        absolute number of test samples. If None, the value is set to the
        complement of the train size. If ``train_size`` is also None, it will
        be set to 0.25.
    
    train_size : float, int, or None, (default=None)
        If float, should be between 0.0 and 1.0 and represent the
        proportion of the dataset to include in the train split. If
        int, represents the absolute number of train samples. If None,
        the value is automatically set to the complement of the test size.
    
    random_state : int, RandomState instance or None, optional (default=None)
        If int, random_state is the seed used by the random number generator;
        If RandomState instance, random_state is the random number generator;
        If None, the random number generator is the RandomState instance used
        by `np.random`.
    
    shuffle : boolean, optional (default=True)
        Whether or not to shuffle the data before splitting. If shuffle=False
        then stratify must be None.
    
    stratify : array-like or None (default=None)
        If not None, data is split in a stratified fashion, using this as
        the class labels.
    
    Returns
    -------
    splitting : list, length=2 * len(arrays)
        List containing train-test split of inputs.
    
        .. versionadded:: 0.16
            If the input is sparse, the output will be a
            ``scipy.sparse.csr_matrix``. Else, output type is the same as the
            input type.
    
    Examples
    --------
    >>> import numpy as np
    >>> from sklearn.model_selection import train_test_split
    >>> X, y = np.arange(10).reshape((5, 2)), range(5)
    >>> X
    array([[0, 1],
           [2, 3],
           [4, 5],
           [6, 7],
           [8, 9]])
    >>> list(y)
    [0, 1, 2, 3, 4]
    
    >>> X_train, X_test, y_train, y_test = train_test_split(
    ...     X, y, test_size=0.33, random_state=42)
    ...
    >>> X_train
    array([[4, 5],
           [0, 1],
           [6, 7]])
    >>> y_train
    [2, 0, 3]
    >>> X_test
    array([[2, 3],
           [8, 9]])
    >>> y_test
    [1, 4]
    
    >>> train_test_split(y, shuffle=False)
    [[0, 1, 2], [3, 4]]
    [0;31mFile:[0m      /Applications/anaconda3/lib/python3.7/site-packages/sklearn/model_selection/_split.py
    [0;31mType:[0m      function




```python
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=1)
```


```python
print(X_train.shape)
print(Y_train.shape)
print(X_test.shape)
print(Y_test.shape)
```

    (120, 4)
    (120,)
    (30, 4)
    (30,)


# 使用sklearn中的KNN分离器测试


```python
from sklearn.neighbors import KNeighborsClassifier
KNeighborsClassifier?
```


    [0;31mInit signature:[0m
    [0mKNeighborsClassifier[0m[0;34m([0m[0;34m[0m
    [0;34m[0m    [0mn_neighbors[0m[0;34m=[0m[0;36m5[0m[0;34m,[0m[0;34m[0m
    [0;34m[0m    [0mweights[0m[0;34m=[0m[0;34m'uniform'[0m[0;34m,[0m[0;34m[0m
    [0;34m[0m    [0malgorithm[0m[0;34m=[0m[0;34m'auto'[0m[0;34m,[0m[0;34m[0m
    [0;34m[0m    [0mleaf_size[0m[0;34m=[0m[0;36m30[0m[0;34m,[0m[0;34m[0m
    [0;34m[0m    [0mp[0m[0;34m=[0m[0;36m2[0m[0;34m,[0m[0;34m[0m
    [0;34m[0m    [0mmetric[0m[0;34m=[0m[0;34m'minkowski'[0m[0;34m,[0m[0;34m[0m
    [0;34m[0m    [0mmetric_params[0m[0;34m=[0m[0;32mNone[0m[0;34m,[0m[0;34m[0m
    [0;34m[0m    [0mn_jobs[0m[0;34m=[0m[0;32mNone[0m[0;34m,[0m[0;34m[0m
    [0;34m[0m    [0;34m**[0m[0mkwargs[0m[0;34m,[0m[0;34m[0m
    [0;34m[0m[0;34m)[0m[0;34m[0m[0;34m[0m[0m
    [0;31mDocstring:[0m     
    Classifier implementing the k-nearest neighbors vote.
    
    Read more in the :ref:`User Guide <classification>`.
    
    Parameters
    ----------
    n_neighbors : int, optional (default = 5)
        Number of neighbors to use by default for :meth:`kneighbors` queries.
    
    weights : str or callable, optional (default = 'uniform')
        weight function used in prediction.  Possible values:
    
        - 'uniform' : uniform weights.  All points in each neighborhood
          are weighted equally.
        - 'distance' : weight points by the inverse of their distance.
          in this case, closer neighbors of a query point will have a
          greater influence than neighbors which are further away.
        - [callable] : a user-defined function which accepts an
          array of distances, and returns an array of the same shape
          containing the weights.
    
    algorithm : {'auto', 'ball_tree', 'kd_tree', 'brute'}, optional
        Algorithm used to compute the nearest neighbors:
    
        - 'ball_tree' will use :class:`BallTree`
        - 'kd_tree' will use :class:`KDTree`
        - 'brute' will use a brute-force search.
        - 'auto' will attempt to decide the most appropriate algorithm
          based on the values passed to :meth:`fit` method.
    
        Note: fitting on sparse input will override the setting of
        this parameter, using brute force.
    
    leaf_size : int, optional (default = 30)
        Leaf size passed to BallTree or KDTree.  This can affect the
        speed of the construction and query, as well as the memory
        required to store the tree.  The optimal value depends on the
        nature of the problem.
    
    p : integer, optional (default = 2)
        Power parameter for the Minkowski metric. When p = 1, this is
        equivalent to using manhattan_distance (l1), and euclidean_distance
        (l2) for p = 2. For arbitrary p, minkowski_distance (l_p) is used.
    
    metric : string or callable, default 'minkowski'
        the distance metric to use for the tree.  The default metric is
        minkowski, and with p=2 is equivalent to the standard Euclidean
        metric. See the documentation of the DistanceMetric class for a
        list of available metrics.
        If metric is "precomputed", X is assumed to be a distance matrix and
        must be square during fit. X may be a :term:`Glossary <sparse graph>`,
        in which case only "nonzero" elements may be considered neighbors.
    
    metric_params : dict, optional (default = None)
        Additional keyword arguments for the metric function.
    
    n_jobs : int or None, optional (default=None)
        The number of parallel jobs to run for neighbors search.
        ``None`` means 1 unless in a :obj:`joblib.parallel_backend` context.
        ``-1`` means using all processors. See :term:`Glossary <n_jobs>`
        for more details.
        Doesn't affect :meth:`fit` method.
    
    Attributes
    ----------
    classes_ : array of shape (n_classes,)
        Class labels known to the classifier
    
    effective_metric_ : string or callble
        The distance metric used. It will be same as the `metric` parameter
        or a synonym of it, e.g. 'euclidean' if the `metric` parameter set to
        'minkowski' and `p` parameter set to 2.
    
    effective_metric_params_ : dict
        Additional keyword arguments for the metric function. For most metrics
        will be same with `metric_params` parameter, but may also contain the
        `p` parameter value if the `effective_metric_` attribute is set to
        'minkowski'.
    
    outputs_2d_ : bool
        False when `y`'s shape is (n_samples, ) or (n_samples, 1) during fit
        otherwise True.
    
    Examples
    --------
    >>> X = [[0], [1], [2], [3]]
    >>> y = [0, 0, 1, 1]
    >>> from sklearn.neighbors import KNeighborsClassifier
    >>> neigh = KNeighborsClassifier(n_neighbors=3)
    >>> neigh.fit(X, y)
    KNeighborsClassifier(...)
    >>> print(neigh.predict([[1.1]]))
    [0]
    >>> print(neigh.predict_proba([[0.9]]))
    [[0.66666667 0.33333333]]
    
    See also
    --------
    RadiusNeighborsClassifier
    KNeighborsRegressor
    RadiusNeighborsRegressor
    NearestNeighbors
    
    Notes
    -----
    See :ref:`Nearest Neighbors <neighbors>` in the online documentation
    for a discussion of the choice of ``algorithm`` and ``leaf_size``.
    
    .. warning::
    
       Regarding the Nearest Neighbors algorithms, if it is found that two
       neighbors, neighbor `k+1` and `k`, have identical distances
       but different labels, the results will depend on the ordering of the
       training data.
    
    https://en.wikipedia.org/wiki/K-nearest_neighbor_algorithm
    [0;31mFile:[0m           /Applications/anaconda3/lib/python3.7/site-packages/sklearn/neighbors/_classification.py
    [0;31mType:[0m           ABCMeta
    [0;31mSubclasses:[0m     




```python
KNNClf = KNeighborsClassifier(n_neighbors=3)
KNNClf.fit(X_train, Y_train)
```




    KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski',
                         metric_params=None, n_jobs=None, n_neighbors=3, p=2,
                         weights='uniform')




```python
res = KNNClf.predict(X_test)
res
```




    array([0, 1, 1, 0, 2, 1, 2, 0, 0, 2, 1, 0, 2, 1, 1, 0, 1, 1, 0, 0, 1, 1,
           1, 0, 2, 1, 0, 0, 1, 2])




```python
Y_test
```




    array([0, 1, 1, 0, 2, 1, 2, 0, 0, 2, 1, 0, 2, 1, 1, 0, 1, 1, 0, 0, 1, 1,
           1, 0, 2, 1, 0, 0, 1, 2])




```python
acc = sum(res == Y_test)
```


```python
print('accuracy ', acc / len(Y_test))
```

    accuracy  1.0



```python

```
