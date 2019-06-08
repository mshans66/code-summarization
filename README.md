# code-summarization
The aim of the project is to  build a tool that can automatically produce a natural language summary of source code written in Python

## Directories & descriptions

| Directory | Description |
|-----------|-------------|
| data | Contains the processed dataset of function declaration+body, description, and generated ASTs |
| utils | Helper methods for preprocessing tasks |
| V2 | V2/parallel is the initial extracted function declaration, description & body. V2/repo_split contains the splits used in the original paper|
| backtranslations-corpus| A corpus of docstrings automatically generated from the code-only corpus using Neural Machine Translation, used in the original paper (may/may not be relevant) |
| parallel-corpus | The raw corpus used in the original paper (may/may not be relevant) |
| model | The models generated for python dataset |


## Data preprocessing description

The Python3 source codes, obtained from GitHub repositories, were preprocessed by removing the comments, removing semantically irrelevant spaces or new lines. An example of an extracted function is as follows:

### Python function
```python
def _interceptdot (w, X, y):

  ””” 
  Computes  y∗np . dot (X, w).
  It takes into consideration whether  the  intercept  should  be  fit.
  Parameters
  −−−−−−−−−−
  w : ndarray ,  ndarray ,  shape  ( n_features , )  or  ( n_features +  1 , )
    Coefficient  vector .
  [ . . . ]

  ”””

  c =  0
  if w.size  == X. shape [ 1 ]  +  1:
    c = w[−1]
    w = w[:−1]
    z =  safe_sparse_dot (X, w)  + c
    yz = y∗z
  return w,  c ,  yz
  ```
### Extracted declaration 
```def _interceptdot(w, X,  y):``` 

### Extracted docstring 
``` ’ Computes  y∗np . dot (X, w) . DCNL It  takes  into  consideration  if  the  intercept  should be  fit  or  not. DCNL P arameters DCNL w :  ndarray ,  shape  ( n_features , )  or  (n_features  +  1 , ) DCNL Coefficient  vector . DCNL  [ . . . ] ’ ```

### Extracted function-body
``` DCSP c = 0.0 DCNL DCSP  if  (w.size == (X.shape [1] + 1) ): DCNL DCSP  DCSP c = w[(−1)] DCNL DCSP  DCSP w = w[:(−1)] DCNL DCSP z =(safe_sparse_dot(X, w) + c) DCNL DCSP yz = (y∗z) DCNL DCSP  return w,  c ,  yz ```
### Extracted repository metadata

```github/scikit−learn/scikit−learn/sklearn/linearmodel/logistic.py 39```

## References 
[1] Barone, Antonio Valerio Miceli, and Rico Sennrich. "A parallel corpus of Python functions and documentation strings for automated code documentation and code generation." arXiv preprint arXiv:1707.02275 (2017).

# Data

## Java
Due to the rather large size of the datasets and the 100MB limitation Github enforces, we've provided a [link](https://drive.google.com/open?id=1mJRuS_Z0MMYmwWO09LrSXC5idqdgCVCJ) to the preprocessed Java datasets. Files prefixes (eg. src and tgt) indicate the OpenNMT flags to use with each. 

NOTE: preprocessed here means:
- The methods are split between method name and method body AND both have been tokenized and subtokenized
- The method name has been stripped of parameters
- The methody body has been converted to an AST with several paths sampled between terminal nodes

### Inputs (X)
- src.train.txt
- src.val.txt
- src.test.txt

### Targets (Y)
- tgt.train.txt
- tgt.val.txt
- tgt.test.txt

Source: [code2seq](https://github.com/tech-srl/code2seq#datasets)
