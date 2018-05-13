# src

This directory contains source codes for the papers , written in C#. This code runs on CPUs.

We have reworked the code. The code can train MLP models for MNIST using meProp, or meSimp. All that needs to be done is to modify the configuration file. 

We coded a simple framework for neural networks. It is similar to the dynamic computation graph method. The operations in the forward propagation is recorded, and in back propagation the corresponding gradient operations are applied.



The structure of the codes and some comments.

```
csharp\
-- nnmnist\
       Config.cs: configurations for the training
       default.json: an example of the configuration files
---- Application\
       the main training loop is in Mnist.cs
---- Common\
       codes for utilities: logger, RNG, timer, topn
---- Data\
       codes for loading the MNIST data
---- Networks\ 
       the codes for the neural network models
       MLP.cs: baseline
       MLPTop.cs: meProp
       MLPVar.cs: meSimp
       MLPRand.cs: random k selection of meProp
------ Graph\
         the basics for building neural networks
         Matrix.cs: a two-dimensional array for efficiency
         Tensor.cs: has weight, gradient, and the gradient history
         Flow.cs: the operations, and the record of the applied operations    
------ Inits\
         weight initialization methods
------ Opts\
         optimizers
------ Units\
         modules, e.g., the fully-connected layer
       
```


- To see the general training procedure, please refer to [Mnist.cs](,/csharp/nnmnist/Application/Mnist.cs).
- To see the implementation of meProp:
  - [TopNHeap.cs](./csharp/nnmnist/Common/TopNHeap.cs) contains the code for extracting the top-k elements.
  - StepTopK methods of the classes in the [Units](./csharp/nnmnist/Networks/Units/) show the forward propagation procedure.
  - MultiplyTop methods in [Flow.cs](./csharp/nnmnist/Networks/Graph/Flow.cs) show the computation of the forward propagation and backward propagation involving sparse multiplication of two matrices.
  - [MLPTop.cs](./csharp/nnmnist/Networks/MLP.cs) defines the model of meProp.
- To see the implementation of meSimp:
  - [Mnist.cs](,/csharp/nnmnist/Application/Mnist.cs) contains the cycle mechanism.
  - [DenseMaskedUnit.cs](./csharp/nnmnist/Networks/Units/DenseMaskedUnit.cs) shows an equivalent way to remove the neurons by masking.
  - [Record.cs](./csharp/nnmnist/Networks/Units/Record.cs) keeps the activeness of the neurons.
  - MultiplyTopRecord methods in [Flow.cs](./csharp/nnmnist/Networks/Graph/Flow.cs) shows how the activeness is collected.
  - [MLPVar.cs](./csharp/nnmnist/Networks/MLPVar.cs) defines the model of meSimp.
- To see the neural network framework, please refer to the [Graph](./csharp/nnmnist/Networks/Graph/) directory.
  - [Flow.cs](./csharp/nnmnist/Networks/Graph/Flow.cs) defines the operations with the forward computation, and the backward gradient computation.
  - [Tensor.cs](./csharp/nnmnist/Networks/Graph/Tensor.cs) defines the object of the operations in [Flow.cs](./csharp/nnmnist/Networks/Graph/Flow.cs). Tensor has its value, its gradient, and the gradient history if required by the optimizer, e.g., Adam.

