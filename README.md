<img src="https://svgshare.com/i/6GL.svg" width="430"> 

  [![NPM version][npm-image]][npm-url]
  [![build status][travis-image]][travis-url]
  [![npm download][download-image]][download-url]
  [![GitHub license][licence-image]](https://github.com/robin-rpr/gpu-matrix/blob/master/LICENSE)

A GPU Accelerated Machine-Learning Matrix manipulation and computation library.

> **Note:** If you get lost, the [API Documentation](https://mljs.github.io/matrix/) will save you.

## Installation

```
npm install gpu-matrix --save
```

#### Node.js:
```js
const { Matrix } = require('gpu-matrix');
```

## Usage

### As an ES6 module

```js
import { Matrix } from 'gpu-matrix';

let myMatrix = Matrix.ones(5, 5);
```

### As a CommonJS module - Nodejs

```js
const { Matrix } = require('matrixjs');

let matrix = Matrix.ones(5, 5);
```

## Examples 

### Standard operations

``` js
const { Matrix } = require('gpu-matrix');

var A = new Matrix([[1, 1], [2, 2]]);
var B = new Matrix([[3, 3], [1, 1]]);
var C = new Matrix([[3, 3], [1, 1]]);

/*
 * Operations with the Matrix
 * @Introduction: https://machinelearningmastery.com/matrix-operations-for-machine-learning/
 */

// Normal operations

const addition = Matrix.add(A, B); // Addition = Matrix [[4, 4], [3, 3], rows: 2, columns: 2]
const substraction = Matrix.sub(A, B); // Substraction = Matrix [[-2, -2], [1, 1], rows: 2, columns: 2]
const multiplication = A.mmul(B); // Multiplication = Matrix [[4, 4], [8, 8], rows: 2, columns: 2]
const mulByNumber = Matrix.mul(A, 10); // MulByNumber = Matrix [[10, 10], [20, 20], rows: 2, columns: 2]
const divByNumber = Matrix.div(A, 10); // DivByNumber = Matrix [[0.1, 0.1], [0.2, 0.2], rows: 2, columns: 2]
const modulo = Matrix.mod(B, 2); // Modulo = Matrix [[ 1, 1], [1, 1], rows: 2, columns: 2]
const maxMatrix = Matrix.max(A, B); // Max = Matrix [[3, 3], [2, 2], rows: 2, columns: 2]
const minMatrix = Matrix.min(A, B); // Min = Matrix [[1, 1], [1, 1], rows: 2, columns: 2]


// Inplace operations (consider that Cinit = C before all the operations below)

C.add(A); // => C = Cinit + A
C.sub(A); // => C = Cinit
C.mul(10); // => C = 10 * Cinit
C.div(10); // => C = Cinit
C.mod(2); // => C = Cinit % 2

// Standard Math operations (abs, cos, round, etc.)

var A = new Matrix([[1, 1], [-1, -1]]);
var expon = Matrix.exp(A); // expon = Matrix [[Math.exp(1), Math.exp(1)], [Math.exp(-1), Math.exp(-1)], rows: 2, columns: 2]. 
var cosinus = Matrix.cos(A); // cosinus = Matrix [[Math.cos(1), Math.cos(1)], [Math.cos(-1), Math.cos(-1)], rows: 2, columns: 2]. 
var absolute = Matrix.abs(A); // expon = absolute [[1, 1], [1, 1], rows: 2, columns: 2]. 

// You can use 'abs', 'acos', 'acosh', 'asin', 'asinh', 'atan', 'atanh', 'cbrt', 'ceil', 'clz32', 'cos', 'cosh', 'exp', 'expm1', 'floor', 'fround', 'log', 'log1p', 'log10', 'log2', 'round', 'sign', 'sin', 'sinh', 'sqrt', 'tan', 'tanh', 'trunc'
// Note: you can do it inplace too as A.abs()

/*
 * Manipulation of the matrix
 * @Introduction: http://ee263.stanford.edu/notes/notes-matrix-primer.pdf
 */

var numberRows = A.rows; // A has 2 rows
var numberCols = A.columns; // A has 2 columns
var firstValue = A.get(0, 0); // get(rows, columns)
var numberElements = A.size; // 2 * 2 = 4 elements
var isRow = A.isRowVector(); // false because A has more that 1 row
var isColumn = A.isColumnVector(); // false because A has more that 1 column
var isSquare = A.isSquare(); // true, because A is 2 * 2 matrix
var isSym = A.isSymmetric(); // false, because A is not symmetric
// remember : A = Matrix [[1, 1], [-1, -1], rows: 2, columns: 2]
A.set(1, 0, 10); // A = Matrix [[1, 1], [10, -1], rows: 2, columns: 2]. We have change the second row and the first column
var diag = A.diag(); // diag = [1, -1], i.e values in the diagonal.
var m = A.mean(); // m = 2.75
var product = A.prod(); // product = -10, i.e product of all values of the matrix
var norm = A.norm(); // norm = 10.14889156509222, i.e Frobenius norm of the matrix
var transpose = A.transpose(); // tranpose = Matrix [[1, 10], [1, -1], rows: 2, columns: 2]

/*
 * Instanciation of matrix
 */

var myMatrix = Matrix.zeros(3, 2); // z = Matrix [[0, 0], [0, 0], [0, 0], rows: 3, columns: 2]
var myMatrix = Matrix.ones(2, 3); // z = Matrix [[1, 1, 1], [1, 1, 1], rows: 2, columns: 3]
var myMatrix = Matrix.eye(3, 4); // Matrix [[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0], rows: 3, columns: 4]. there are 1 only in the diagonal
```

### Maths

```js
const {
    Matrix,
    inverse,
    solve,
    linearDependencies,
    QrDecomposition,
    LuDecomposition,
    CholeskyDecomposition
} = require('gpu-matrix');


/*
 * Inverse and pseudo-inverse
 */

var A = new Matrix([[2, 3, 5], [4, 1, 6], [1, 3, 0]]); 
var inverseA = inverse(A);
var B = A.mmul(inverseA); // B = A * inverse(A), so B ~= Identity 

// if A is singular, you can use SVD :
var A = new Matrix([[1, 2, 3], [4, 5, 6], [7, 8, 9]]); // A is singular, so the standard computation of inverse won't work (you can test if you don't trust me^^)
var inverseA = inverse(A, useSVD = true); // inverseA is only an approximation of the inverse, by using the Singular Values Decomposition
var B = A.mmul(inverseA); // B = A * inverse(A), but inverse(A) is only an approximation, so B doesn't really be identity. 

// if you want the pseudo-inverse of a matrix :
var A = new Matrix([[1, 2], [3, 4], [5, 6]]);
var pseudoInverseA = A.pseudoInverse();
var B = A.mmul(pseudoInverseA).mmul(A); // with pseudo inverse, A*pseudo-inverse(A)*A ~= A. It's the case here


/*
 * Least square
 */

// Least square is the following problem : We search x, such as A.x = b (A, x and b are matrix or vectors).
// Below, how to solve least square with our function

// If A is non singular :
var A = new Matrix([[3, 1], [4.25, 1], [5.5, 1], [8, 1]]);
var b = Matrix.columnVector([4.5, 4.25, 5.5, 5.5]);
var x = solve(A, b); 
var error = Matrix.sub(b, A.mmul(x)); // The error enables to evaluate the solution x found. 

// If A is non singular :
var A = new Matrix([[1, 2, 3], [4, 5, 6], [7, 8, 9]]);
var b = Matrix.columnVector([8, 20, 32]);
var x = solve(A, b, useSVD = true); // there are many solutions. x can be [1, 2, 1].transpose(), or [1.33, 1.33, 1.33].transpose(), etc. 
var error = Matrix.sub(b, A.mmul(x)); // The error enables to evaluate the solution x found. 


/*
 * Decompositions
 */

// QR Decomposition

var A = new Matrix([[2, 3, 5], [4, 1, 6], [1, 3, 0]]);  
var QR = QrDecomposition(A);
var Q = QR.orthogonalMatrix; 
var R = QR.upperTriangularMatrix;
// So you have the QR decomposition. If you multiply Q by R, you'll see that A = Q.R, with Q orthogonal and R upper triangular

// LU Decomposition

var A = new Matrix([[2, 3, 5], [4, 1, 6], [1, 3, 0]]);  
var LU = LuDecomposition(A);
var L = LU.lowerTriangularMatrix; 
var U = LU.upperTriangularMatrix;
var P = LU.pivotPermutationVector;
// So you have the LU decomposition. P includes the permutation of the matrix. Here P = [1, 2, 0], i.e the first row of LU is the second row of A, the second row of LU is the third row of A and the third row of LU is the first row of A.

// Cholesky Decomposition

var A = new Matrix([[2, 3, 5], [4, 1, 6], [1, 3, 0]]);  
var cholesky = CholeskyDecomposition(A);
var L = cholesky.lowerTriangularMatrix; 


/*
 * Others
 */

// Linear dependencies

var A = new Matrix([[2, 0, 0, 1], [0, 1, 6, 0], [0, 3, 0, 1], [0, 0, 1, 0], [0, 1, 2, 0]]);  
var dependencies = linearDependencies(A); // dependencies is a matrix with the dependencies of the rows. When we look row by row, we see that the first row is [0, 0, 0, 0, 0], so it means that the first row is independent, and the second row is [ 0, 0, 0, 4, 1 ], i.e the second row = 4 times the 4th row + the 5th row.

```

## License

  [MIT](./LICENSE)

[npm-image]: https://img.shields.io/npm/v/gpu-matrix.svg?style=flat-square
[npm-url]: https://npmjs.org/package/gpu-matrix
[travis-image]: https://img.shields.io/travis/robin-rpr/gpu-matrix/master.svg?style=flat-square
[travis-url]: https://travis-ci.org/robin-rpr/gpu-matrix
[download-image]: https://img.shields.io/npm/dm/gpu-matrix.svg?style=flat-square
[download-url]: https://npmjs.org/package/gpu-matrix
[licence-image]: https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square
