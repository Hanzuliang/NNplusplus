# NN++

A short, self-contained, and easy-to-use deep neural net implementation for C++. It includes the neural net implementation and a Matrix class for basic linear algebra operations. This project is mostly for learning purposes, but preliminary testing results over the MNIST dataset show some promise.

## Getting Started

These instructions will get you a copy of the net up and running on your local machine for development and testing purposes.

### Prerequisites

Any compiler that can handle C++11.

### Installing

1. Download `Matrix.hpp`, `Matrix.cpp`, `NeuralNet.hpp`, and `NeuralNet.cpp` and place them in your project's working directory.
2. Include the headers in your main driver program (e.g. `main.cpp`).

```
#include "Matrix.hpp"
#include "NeuralNet.hpp"
```

3. Start training your net!

## Example Code
### The Matrix Class
First you need to know how to use the Matrix class.
Matrix objects are basically 2D-vectors with built-in linear algebra operations.

#### Matrix Initialization

```
Matrix A;       // Initializes a 0x0 matrix.
Matrix B(2,2)   // Initializes a 2x2 matrix with all zeros. Values are doubles.
Matrix C(2,1)   // Initializes a 2x1 matrix.
```
#### Element Access
To access/modify a value in a matrix, use `operator()`, NOT `operator[]`:

```
B(0,0) = 1; B(0,1) = 2; B(1,0) = 3; B(1,1) = 4;   // [1    2]
                                                  // [3    4]

C(0,0) = 1; C(1,0) = 2;                           // [1]
                                                  // [2]
```

#### Matrix Term-by-Term Addition/Subtraction/Multiplication
```
// Commutative property is supported for addition
Matrix D = B+B;       // D = [2   4]
                             [6   8]
                             
Matrix E = B-B;       // E = [0   0]
                             [0   0]
                             
// Commutative property is supported for multiplication                             
Matrix F = B*B        // F = [1   4]
                             [9  16]
                             
// Mismatching matrix dimensions in term-by-term operations
// is illegal and therefore an empty matrix object is returned.
Matrix G = B+C;       // G = []
Matrix G = B-C;       // G = []
Matrix G = B*C;       // G = []
```

#### Matrix and Scalars
```
// Commutative property is supported for addition
Matrix BplusTwo = B+2;  // (== 2+B)   BplusTwo = [3   4]
                                                 [5   6]

Matrix CminusTwo = C-2; //           CminusTwo = [-1]
                                                 [ 0]
                                                 
Matrix TwominusB = 2-C; //           TwominusB = [ 1]
                                                 [ 0]
                                                 
// Commutative property is supported for multiplication
Matrix BtimesThree = B*3; // (== 3*B) BtimesThree = [3    6]
                                                    [9   12]
```

#### Matrix Multiplication (Dot Product)
```
Matrix BB = B.dot(B);     // BB = [ 7  10]
                                  [15  22]
                                  
Matrix BC = B.dot(C);     // BC = [ 5]
                                  [11]

// Mismatching the number of columns in the left-hand-side vector
// with the number of rows in the right-hand-side vector is illegal
// An empty matrix object is returned.
Matrix CB = C.dot(B);     // CB = [];
```

#### Matrix Transpose
```
Matrix B_T = B.T();   // B_T = [1   3]
                               [2   4]
                                 
Matrix C_T = C.T();   // C_T = [1   2]
```

#### An Example of Populating a 4x3 Matrix
```
int m = 4;
int n = 3;

Matrix mtrx(m,n);

int count = 1;
for (int i = 0; i < mtrx.getNumOfRows(); ++i) {
    for (int j = 0; j < mtrx.getNumOfCols(); ++j) {
        mtrx(i,j) = count;
        ++count;
    }
}
```
This will result with `mtrx` ==
```
[ 1     2     3]
[ 4     5     6]
[ 7     8     9]
[10    11    12]
```

### The NeuralNet Class
Now that you know how to work with the Matrix class, let's train a neural net!

#### Neural Net Initialization (The Parameters)
When initialized, a net takes in five parameters:
1. Number of input nodes.  
2. Number of nodes per hidden layer.  
3. Number of output nodes.  
4. Number of hidden layers.  
5. The learning rate.  

```
NeuralNet NN(2, 3, 1, 1, 0.1);
```
This neural net has 2 input nodes, 1 hidden layer with 3 nodes, 1 output node, and has a learning rate of 0.1.  
New neural nets' weights are initialized with values drawn from a normal distribution centered at 0, with standard deviation that is equal to `1/sqrt(number_of_inputs_to_nodes_in_next_layer)`. In other words, relatively small negative and positive values that are proportional to the size of the net.

#### A Training Cycle
Once the net is initialized, it is ready to do work.  
ONE training cycle == one feed forward and one back propagation with weight adjustments.  
  
To train one cycle, the input data must be parsed into a Matrix object with dimensions: `1xnumber_of_input_nodes` (1x2 in our case), and the target data must be parsed into a Matrix object with dimensions: `1xnumber_of_output_nodes` (1x1).  
```
Matrix input(1,2);
input(0,0) = 0.3;
input(0,1) = -0.1;

Matrix target(1,1);
target(0,0) = 0.5;
```
Then, simply execute the training cycle on the data as follows:
```
NN.trainingCycle(input, target);
```
#### A Training Cycle
Once the training phase is complete, you can query it as follows:
(Technically speaking, you can query it right after initialization).

1. Parse the query into a Matrix like parsed the training instance:
```
Matrix query(1,2);
input(0,0) = 0.5;
input(0,1) = -0.2;
```

2. Query the net and catch the result:
```
Matrix prediction = NN.queryNet(query);   // Will return a 1x1 Matrix object with net's prediction
```
AND THAT'S IT!

## Authors

* **Gil Dekel** - *Initial implementation* - [stagadish](https://github.com/stagadish)

See also the list of [contributors](https://github.com/stagadish/NNplusplus/contributors) who participated in this project.

## License

This project is licensed under the GNU GENERAL PUBLIC LICENSE V3 - see the [LICENSE.md](LICENSE.md) file for details