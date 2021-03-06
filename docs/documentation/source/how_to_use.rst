How to use ``Anno Domini``
=======================================

How to Install
--------------

**Internal Note: How to Publish to Pip**

.. code-block:: bash

    $ python setup.py sdist
    $ twine upload dist/*

**Install via Pip:**

.. code-block:: bash

    pip install AnnoDomini

**Install in a Virtual Environment:**

.. code-block:: bash

    $ pip install virtualenv # If Necessary
    $ virtualenv venv
    $ source venv/bin/activate
    $ pip install numpy
    $ pip install AnnoDomini
    $ python
    >>> import AnnoDomini.AutoDiff as AD
    >>> x = AD.AutoDiff(3.0)
    >>> print(x)
    ====== Function Value(s) ======
    3.0
    ===== Derivative Value(s) =====
    1.0
    >>> quit()
    $ deactivate

.. note:: For using additional features, SciPy and tqdm packages are also required.

Basic Demos
------------

1. Single Variable, Single Function (:math:`\mathbb{R}^1 \rightarrow \mathbb{R}^1`)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Suppose we want to find the derivative of :math:`x^2+2x+1`. We can utilize the AnnoDomini package as follows:

.. code-block:: python

    >>> x = AD.AutoDiff(1.5)
    >>> print(x)
    ====== Function Value(s) ======
    1.5
    ===== Derivative Value(s) =====
    1.0
    >>> f = x**2 + 2*x + 1
    >>> print(f)
    ====== Function Value(s) ======
    6.25
    ===== Derivative Value(s) =====
    5.0

We can access only the value or derivative component as follows:

.. code-block:: python

    >>> print(f.val)
    6.25
    >>> print(f.der)
    5.0

Other elementary functions can be used in the same way.  For instance, we may evaluate the derivative of :math:`log_{2}(x)+arctan(3x+5)` at :math:`x = 10.0` as follows:

.. code-block:: python

    >>> x = AD.AutoDiff(10.0)
    >>> f = x.log(2) + np.arctan(3 * x + 5)
    >>> print(f)
    ====== Function Value(s) ======
    4.864160763843499
    ===== Derivative Value(s) =====
    0.14671648614436125

.. note:: For the single variable case, we do not need to input the scalar number in the form of a list (i.e. using brackets); the AutoDiff class is smart enough to handle the scalar form as appropriate.

2. Multiple Variables, Single Function (:math:`\mathbb{R}^m \rightarrow \mathbb{R}^1`)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Consider the case where the user would like to input the function,
:math:`f = xy`. Then, the derivative of this would be represented in a Jacobian matrix,
:math:`J = [\frac{df}{dx}, \frac{df}{dy}] = [y,x]`.

.. code-block:: python

    >>> x = AD.AutoDiff(3., [1., 0.])
    >>> y = AD.AutoDiff(2., [0., 1.])
    >>> f = x*y
    >>> print(f)
    ====== Function Value(s) ======
    6.0
    ===== Derivative Value(s) =====
    [2. 3.]

3. Single Variable, Multiple Functions (:math:`\mathbb{R}^1 \rightarrow \mathbb{R}^n`)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Consider the case where the user would like to input the two functions,
:math:`F = [x^2, 2x]`. Then, the derivative of this would be represented in a Jacobian matrix,
:math:`J = [\frac{df_1}{dx}, \frac{df_2}{dx}] = [2x,2]`.

.. code-block:: python

    >>> x = AD.AutoDiff(3., 1.)
    >>> f1 = x**2
    >>> f2 = 2*x
    >>> print(AD.AutoDiff([f1, f2]))
    ====== Function Value(s) ======
    [9. 6.]
    ===== Derivative Value(s) =====
    [6. 2.]

.. note:: For evaluating multiple functions, the AutoDiff class expects the functions to be input as a Python list (i.e. using brackets); other data structures (e.g., NumPy array) are not supported.

4. Multiple Variables, Multiple Functions (:math:`\mathbb{R}^m \rightarrow \mathbb{R}^n`)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Consider the case where the user would like to input the two functions,
:math:`F = [x+y, xy]`. Then, the derivative of this would be represented in a Jacobian matrix,
:math:`J = [[\frac{df_1}{dx}, \frac{df_1}{dy}],[\frac{df_2}{dx}, \frac{df_2}{dy}]] = [[1, 1], [y, x]]`.

.. code-block:: python

    >>> x = AD.AutoDiff(3., [1., 0.])
    >>> y = AD.AutoDiff(2., [0., 1.])
    >>> f1 = x+y
    >>> f2 = x*y
    >>> print(AD.AutoDiff([f1, f2]))
    ====== Function Value(s) ======
    [5. 6.]
    ===== Derivative Value(s) =====
    [[1. 1.]
     [2. 3.]]
