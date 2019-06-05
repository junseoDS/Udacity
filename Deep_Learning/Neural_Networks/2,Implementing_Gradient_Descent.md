# Implementing Gradient Descent




## Mean Squared Error Function 

### Log-Loss vs Mean Squared Error
In the previous section, Luis taught you about the log-loss function. There are many other error functions used for neural networks. Let me teach you another one, called the mean squared error. As the name says, this one is the mean of the squares of the differences between the predictions and the labels. In the following section I'll go over it in detail, then we'll get to implement backpropagation with it on the same student admissions dataset.

And as a bonus, we'll be implementing this in a very effective way using matrix multiplication with NumPy!



## Gradient Descent 

### Gradient Descent with Squared Errors
We want to find the weights for our neural networks. Let's start by thinking about the goal. The network needs to make predictions as close as possible to the real values. To measure this, we use a metric of how wrong the predictions are, the error. A common metric is the sum of the squared errors (SSE) where y_hat is the prediction and y is the true value, and you take the sum over all output units j and another sum over all data points μ. This might seem like a really complicated equation at first, but it's fairly simple once you understand the symbols and can say what's going on in words.

First, the inside sum over j. This variable j represents the output units of the network. So this inside sum is saying for each output unit, find the difference between the true value y and the predicted value from the network y_hat , then square the difference, then sum up all those squares.

Then the other sum over μ is a sum over all the data points. So, for each data point you calculate the inner sum of the squared differences for each output unit. Then you sum up those squared differences for each data point. That gives you the overall error for all the output predictions for all the data points.

The SSE is a good choice for a few reasons. The square ensures the error is always positive and larger errors are penalized more than smaller errors. Also, it makes the math nice, always a plus.

Remember that the output of a neural network, the prediction, depends on the weights and accordingly the error depends on the weights.

We want the network's prediction error to be as small as possible and the weights are the knobs we can use to make that happen. Our goal is to find weights w_{ij} that minimize the squared error E. To do this with a neural network, typically you'd use gradient descent.



### Enter Gradient Descent 

With gradient descent, we take multiple small steps towards our goal. In this case, we want to change the weights in steps that reduce the error. Continuing the analogy, the error is our mountain and we want to get to the bottom. Since the fastest way down a mountain is in the steepest direction, the steps taken should be in the direction that minimizes the error the most. We can find this direction by calculating the gradient of the squared error.

Gradient is another term for rate of change or slope. If you need to brush up on this concept, check out Khan Academy's [great lectures](https://www.khanacademy.org/math/multivariable-calculus/multivariable-derivatives/gradient-and-directional-derivatives/v/gradient) on the topic.

To calculate a rate of change, we turn to calculus, specifically derivatives. A derivative of a function f(x) gives you another function f'(x) that returns the slope of f(x) at point xx. For example, consider f(x)=x^2. The derivative of x^2 is f'(x) = 2x. So, at x = 2, the slope is f'(2) = 4. Plotting this out, it looks like:


The gradient is just a derivative generalized to functions with more than one variable. We can use calculus to find the gradient at any point in our error function, which depends on the input weights. You'll see how the gradient descent step is derived on the next page.



At each step, you calculate the error and the gradient, then use those to determine how much to change each weight. Repeating this process will eventually find weights that are close to the minimum of the error function, the black dot in the middle.


### Caveats
Since the weights will just go wherever the gradient takes them, they can end up where the error is low, but not the lowest. These spots are called local minima. If the weights are initialized with the wrong values, gradient descent could lead the weights into a local minimum.

There are methods to avoid this, such as using [momentum](https://distill.pub/2017/momentum/).



## Gradient Descent : The Math 




Notes
Check out Khan Academy's [Multivariable calculus lessons](https://www.khanacademy.org/math/multivariable-calculus) if you are unfamiliar with the subject.



## Gradient Descent : The Code 

Now I'll write this out in code for the case of only one output unit. We'll also be using the sigmoid as the activation function f(h)f(h).


    # Defining the sigmoid function for activations
    def sigmoid(x):
        return 1/(1+np.exp(-x))

    # Derivative of the sigmoid function
    def sigmoid_prime(x):
        return sigmoid(x) * (1 - sigmoid(x))

    # Input data
    x = np.array([0.1, 0.3])
    # Target
    y = 0.2
    # Input to output weights
    weights = np.array([-0.8, 0.5])

    # The learning rate, eta in the weight step equation
    learnrate = 0.5

    # the linear combination performed by the node (h in f(h) and f'(h))
    h = x[0]*weights[0] + x[1]*weights[1]
    # or h = np.dot(x, weights)

    # The neural network output (y-hat)
    nn_output = sigmoid(h)

    # output error (y - y-hat)
    error = y - nn_output

    # output gradient (f'(h))
    output_grad = sigmoid_prime(h)

    # error term (lowercase delta)
    error_term = error * output_grad

    # Gradient descent step 
    del_w = [ learnrate * error_term * x[0],
              learnrate * error_term * x[1]]
    # or del_w = learnrate * error_term * x





### gradient.py


    import numpy as np

    def sigmoid(x):
        """
        Calculate sigmoid
        """
        return 1/(1+np.exp(-x))

    def sigmoid_prime(x):
        """
        # Derivative of the sigmoid function
        """
        return sigmoid(x) * (1 - sigmoid(x))

    learnrate = 0.5
    x = np.array([1, 2, 3, 4])
    y = np.array(0.5)

    # Initial weights
    w = np.array([0.5, -0.5, 0.3, 0.1])

    ### Calculate one gradient descent step for each weight
    ### Note: Some steps have been consolidated, so there are
    ###       fewer variable names than in the above sample code

    # TODO: Calculate the node's linear combination of inputs and weights
    h = np.dot(x,w)

    # TODO: Calculate output of neural network
    nn_output = sigmoid(h)

    # TODO: Calculate error of neural network
    error = y-nn_output

    # TODO: Calculate the error term
    #       Remember, this requires the output gradient, which we haven't
    #       specifically added a variable for.
    error_term = error*sigmoid_prime(h)

    # TODO: Calculate change in weights
    del_w = learnrate*error_term*x

    print('Neural Network output:')
    print(nn_output)
    print('Amount of Error:')
    print(error)
    print('Change in Weights:')
    print(del_w)



## Implenmenting Gradient Desecent 

Okay, now we know how to update our weights:

Δw_ij = η∗δ_j∗x_i ,

You've seen how to implement that for a single update, but how do we translate that code to calculate many weight updates so our network will learn?

As an example, I'm going to have you use gradient descent to train a network on graduate school admissions data (found at http://www.ats.ucla.edu/stat/data/binary.csv). This dataset has three input features: GRE score, GPA, and the rank of the undergraduate school (numbered 1 through 4). Institutions with rank 1 have the highest prestige, those with rank 4 have the lowest.

The goal here is to predict if a student will be admitted to a graduate program based on these features. For this, we'll use a network with one output layer with one unit. We'll use a sigmoid function for the output unit activation.


### Data cleanup
You might think there will be three input units, but we actually need to transform the data first. The rank feature is categorical, the numbers don't encode any sort of relative values. Rank 2 is not twice as much as rank 1, rank 3 is not 1.5 more than rank 2. Instead, we need to use [dummy variables](https://en.wikipedia.org/wiki/Dummy_variable_(statistics)) to encode rank, splitting the data into four new columns encoded with ones or zeros. Rows with rank 1 have one in the rank 1 dummy column, and zeros in all other columns. Rows with rank 2 have one in the rank 2 dummy column, and zeros in all other columns. And so on.

We'll also need to standardize the GRE and GPA data, which means to scale the values such that they have zero mean and a standard deviation of 1. This is necessary because the sigmoid function squashes really small and really large inputs. The gradient of really small and large inputs is zero, which means that the gradient descent step will go to zero too. Since the GRE and GPA values are fairly large, we have to be really careful about how we initialize the weights or the gradient descent steps will die off and the network won't train. Instead, if we standardize the data, we can initialize the weights easily and everyone is happy.


### Mean Square Error
We're going to make a small change to how we calculate the error here. Instead of the SSE, we're going to use the mean of the square errors (MSE). Now that we're using a lot of data, summing up all the weight steps can lead to really large updates that make the gradient descent diverge. To compensate for this, you'd need to use a quite small learning rate. Instead, we can just divide by the number of records in our data, mm to take the average. This way, no matter how much data we use, our learning rates will typically be in the range of 0.01 to 0.001. Then, we can use the MSE (shown below) to calculate the gradient and the result is the same as before, just averaged instead of summed.

Here's the general algorithm for updating the weights with gradient descent:

* Set the weight step to zero: Δw_i = 0
* For each record in the training data:
  * Make a forward pass through the network, calculating the output y^=f(∑_i w_i\*x_i )
  * Calculate the error term for the output unit,  δ=(y−y^)∗f′(∑_i w_i\*x_i )
  * Update the weight step  Δw_i =Δw_i +δx_i	 
* Update the weights  w_i =w_i+ηΔw_i/m where η is the learning rate and m is the number of records. Here we're averaging the weight steps to help reduce any large variations in the training data.
* Repeat for e epochs.

You can also update the weights on each record instead of averaging the weight steps after going through all the records.


### Implementing with NumPy

For the most part, this is pretty straightforward with NumPy.

First, you'll need to initialize the weights. We want these to be small such that the input to the sigmoid is in the linear region near 0 and not squashed at the high and low ends. It's also important to initialize them randomly so that they all have different starting values and diverge, breaking symmetry. So, we'll initialize the weights from a normal distribution centered at 0. A good value for the scale is 1/sqrt{n}  where nn is the number of input units. This keeps the input to the sigmoid low for increasing numbers of input units.

    weights = np.random.normal(scale=1/n_features**.5, size=n_features)

NumPy provides a function np.dot() that calculates the dot product of two arrays, which conveniently calculates hh for us. The dot product multiplies two arrays element-wise, the first element in array 1 is multiplied by the first element in array 2, and so on. Then, each product is summed.

    # input to the output layer
    output_in = np.dot(weights, inputs)

And finally, we can update Δw_i  and w_i by incrementing them with weights += ... which is shorthand for weights = weights + ....


### Efficiency tip!

You can save some calculations since we're using a sigmoid here. For the sigmoid function, f'(h) = f(h) (1 - f(h)). That means that once you calculate f(h)f(h), the activation of the output unit, you can use it to calculate the gradient for the error gradient.


### Programming exercise

Below, you'll implement gradient descent and train the network on the admissions data. Your goal here is to train the network until you reach a minimum in the mean square error (MSE) on the training set. You need to implement:

* The network output: output.
* The output error: error.
* The error term: error_term.
* Update the weight step: del_w +=.
* Update the weights: weights +=.

After you've written these parts, run the training by pressing "Test Run". The MSE will print out, as well as the accuracy on a test set, the fraction of correctly predicted admissions.

Feel free to play with the hyperparameters and see how it changes the MSE.

#### data_prep.py

        import numpy as np
        import pandas as pd

        admissions = pd.read_csv('binary.csv')

        # Make dummy variables for rank
        data = pd.concat([admissions, pd.get_dummies(admissions['rank'], prefix='rank')], axis=1)
        data = data.drop('rank', axis=1)

        # Standarize features
        for field in ['gre', 'gpa']:
            mean, std = data[field].mean(), data[field].std()
            data.loc[:,field] = (data[field]-mean)/std

        # Split off random 10% of the data for testing
        np.random.seed(42)
        sample = np.random.choice(data.index, size=int(len(data)*0.9), replace=False)
        data, test_data = data.ix[sample], data.drop(sample)

        # Split into features and targets
        features, targets = data.drop('admit', axis=1), data['admit']
        features_test, targets_test = test_data.drop('admit', axis=1), test_data['admit']




### gradient.py


    import numpy as np
    from data_prep import features, targets, features_test, targets_test


    def sigmoid(x):
        """
        Calculate sigmoid
        """
        return 1 / (1 + np.exp(-x))

    # TODO: We haven't provided the sigmoid_prime function like we did in
    #       the previous lesson to encourage you to come up with a more
    #       efficient solution. If you need a hint, check out the comments
    #       in solution.py from the previous lecture.

    # Use to same seed to make debugging easier
    np.random.seed(42)

    n_records, n_features = features.shape
    last_loss = None

    # Initialize weights
    weights = np.random.normal(scale=1 / n_features**.5, size=n_features)

    # Neural Network hyperparameters
    epochs = 1000
    learnrate = 0.5

    for e in range(epochs):
        del_w = np.zeros(weights.shape)
        for x, y in zip(features.values, targets):
            # Loop through all records, x is the input, y is the target

            # Note: We haven't included the h variable from the previous
            #       lesson. You can add it if you want, or you can calculate
            #       the h together with the output

            # TODO: Calculate the output
            output = sigmoid(np.dot(x,weights))

            # TODO: Calculate the error
            error = y-output

            # TODO: Calculate the error term
            error_term = error*output*(1-output)

            # TODO: Calculate the change in weights for this sample
            #       and add it to the total weight change
            del_w += error_term*x

        # TODO: Update weights using the learning rate and the average change in weights
        weights += learnrate*del_w/n_records

        # Printing out the mean square error on the training set
        if e % (epochs / 10) == 0:
            out = sigmoid(np.dot(features, weights))
            loss = np.mean((out - targets) ** 2)
            if last_loss and last_loss < loss:
                print("Train loss: ", loss, "  WARNING - Loss Increasing")
            else:
                print("Train loss: ", loss)
            last_loss = loss


    # Calculate accuracy on test data
    tes_out = sigmoid(np.dot(features_test, weights))
    predictions = tes_out > 0.5
    accuracy = np.mean(predictions == targets_test)
    print("Prediction accuracy: {:.3f}".format(accuracy))







## Multilayer Perceptrons 


### Implementing the hidden layer

##### Prerequisites

Below, we are going to walk through the math of neural networks in a multilayer perceptron. With multiple perceptrons, we are going to move to using vectors and matrices. To brush up, be sure to view the following:

1. Khan Academy's [introduction to vectors](https://www.khanacademy.org/math/linear-algebra/vectors-and-spaces/vectors/v/vector-introduction-linear-algebra.
2. Khan Academy's [introduction to matrices](https://www.khanacademy.org/math/precalculus/precalc-matrices).

##### Derivation

Before, we were dealing with only one output node which made the code straightforward. However now that we have multiple input units and multiple hidden units, the weights between them will require two indices: w_{ij}  where ii denotes input units and jj are the hidden units.



But now, the weights need to be stored in a matrix, indexed as w_ij. Each row in the matrix will correspond to the weights leading out of a single input unit, and each column will correspond to the weights leading in to a single hidden unit.


Be sure to compare the matrix above with the diagram shown before it so you can see where the different weights in the network end up in the matrix.

To initialize these weights in NumPy, we have to provide the shape of the matrix. If features is a 2D array containing the input data:

    # Number of records and input units
    n_records, n_inputs = features.shape
    # Number of hidden units
    n_hidden = 2
    weights_input_to_hidden = np.random.normal(0, n_inputs**-0.5, size=(n_inputs, n_hidden))

This creates a 2D array (i.e. a matrix) named weights_input_to_hidden with dimensions n_inputs by n_hidden. Remember how the input to a hidden unit is the sum of all the inputs multiplied by the hidden unit's weights.


To do that, we now need to use [matrix multiplication](https://en.wikipedia.org/wiki/Matrix_multiplication). If your linear algebra is rusty, I suggest taking a look at the suggested resources in the prerequisites section. For this part though, you'll only need to know how to multiply a matrix with a vector.

In this case, we're multiplying the inputs (a row vector here) by the weights. To do this, you take the dot (inner) product of the inputs with each column in the weights matrix. For example, to calculate the input to the first hidden unit, j = 1, you'd take the dot product of the inputs with the first column of the weights matrix,


And for the second hidden layer input, you calculate the dot product of the inputs with the second column. And so on and so forth.

In NumPy, you can do this for all the inputs and all the outputs at once using np.dot

    hidden_inputs = np.dot(inputs, weights_input_to_hidden)

You could also define your weights matrix such that it has dimensions n_hidden by n_inputs then multiply like so where the inputs form a column vector


__Note__: The weight indices have changed in the above image and no longer match up with the labels used in the earlier diagrams. That's because, in matrix notation, the row index always precedes the column index, so it would be misleading to label them the way we did in the neural net diagram. Just keep in mind that this is the same weight matrix as before, but rotated so the first column is now the first row, and the second column is now the second row. If we were to use the labels from the earlier diagram, the weights would fit into the matrix in the following locations


The important thing with matrix multiplication is that the dimensions match. For matrix multiplication to work, there has to be the same number of elements in the dot products. In the first example, there are three columns in the input vector, and three rows in the weights matrix. In the second example, there are three columns in the weights matrix and three rows in the input vector. If the dimensions don't match, you'll get this:

    # Same weights and features as above, but swapped the order
    hidden_inputs = np.dot(weights_input_to_hidden, features)
    ---------------------------------------------------------------------------
    ValueError                                Traceback (most recent call last)
    <ipython-input-11-1bfa0f615c45> in <module>()
    ----> 1 hidden_in = np.dot(weights_input_to_hidden, X)
    
    ValueError: shapes (3,2) and (3,) not aligned: 2 (dim 1) != 3 (dim 0)

The dot product can't be computed for a 3x2 matrix and 3-element array. That's because the 2 columns in the matrix don't match the number of elements in the array.



The rule is that if you're multiplying an array from the left, the array must have the same number of elements as there are rows in the matrix. And if you're multiplying the matrix from the left, the number of columns in the matrix must equal the number of elements in the array on the right.


### Making a column vector

You see above that sometimes you'll want a column vector, even though by default NumPy arrays work like row vectors. It's possible to get the transpose of an array like so arr.T, but for a 1D array, the transpose will return a row vector. Instead, use arr[:,None] to create a column vector:


    print(features)
    > array([ 0.49671415, -0.1382643 ,  0.64768854])

    print(features.T)
    > array([ 0.49671415, -0.1382643 ,  0.64768854])

    print(features[:, None])
    > array([[ 0.49671415],
           [-0.1382643 ],
           [ 0.64768854]])

Alternatively, you can create arrays with two dimensions. Then, you can use arr.T to get the column vector.


    np.array(features, ndmin=2)
    > array([[ 0.49671415, -0.1382643 ,  0.64768854]])

    np.array(features, ndmin=2).T
    > array([[ 0.49671415],
           [-0.1382643 ],
           [ 0.64768854]])

I personally prefer keeping all vectors as 1D arrays, it just works better in my head.


### Programming quiz

Below, you'll implement a forward pass through a 4x3x2 network, with sigmoid activation functions for both layers.

Things to do:

* Calculate the input to the hidden layer.
* Calculate the hidden layer output.
* Calculate the input to the output layer.
* Calculate the output of the network.



### multilayer.py


    import numpy as np

    def sigmoid(x):
        """
        Calculate sigmoid
        """
        return 1/(1+np.exp(-x))

    # Network size
    N_input = 4
    N_hidden = 3
    N_output = 2

    np.random.seed(42)
    # Make some fake data
    X = np.random.randn(4)

    weights_input_to_hidden = np.random.normal(0, scale=0.1, size=(N_input, N_hidden))
    weights_hidden_to_output = np.random.normal(0, scale=0.1, size=(N_hidden, N_output))


    # TODO: Make a forward pass through the network

    hidden_layer_in = np.dot(X,weights_input_to_hidden)
    hidden_layer_out = sigmoid(hidden_layer_in)

    print('Hidden-layer Output:')
    print(hidden_layer_out)

    output_layer_in = np.dot(hidden_layer_out,weights_hidden_to_output)
    output_layer_out = sigmoid(output_layer_in)

    print('Output-layer Output:')
    print(output_layer_out)





## Backpropagation

### Backpropagation
Now we've come to the problem of how to make a multilayer neural network learn. Before, we saw how to update weights with gradient descent. The backpropagation algorithm is just an extension of that, using the chain rule to find the error with the respect to the weights connecting the input layer to the hidden layer (for a two layer network).

To update the weights to hidden layers using gradient descent, you need to know how much error each of the hidden units contributed to the final output. Since the output of a layer is determined by the weights between layers, the error resulting from units is scaled by the weights going forward through the network. Since we know the error at the output, we can use the weights to work backwards to hidden layers.

For example, in the output layer, you have errors δ_k^o  attributed to each output unit kk. Then, the error attributed to hidden unit jj is the output errors, scaled by the weights between the output and hidden layers (and the gradient)

Then, the gradient descent step is the same as before, just with the new errors where w_ij are the weights between the inputs and hidden layer and x_i are input unit values. This form holds for however many layers there are. The weight steps are equal to the step size times the output error of the layer times the values of the inputs to that layer

Here, you get the output error, δ_output, by propagating the errors backwards from higher layers. And the input values, V_in  are the inputs to the layer, the hidden layer activations to the output unit for example.



### Implementing in NumPy
For the most part you have everything you need to implement backpropagation with NumPy.

However, previously we were only dealing with error terms from one unit. Now, in the weight update, we have to consider the error for each unit in the hidden layer, δ_j :

Δw_ij=ηδ_j x_i	 

Firstly, there will likely be a different number of input and hidden units, so trying to multiply the errors and the inputs as row vectors will throw an error:

    hidden_error*inputs
    ---------------------------------------------------------------------------
    ValueError                                Traceback (most recent call last)
    <ipython-input-22-3b59121cb809> in <module>()
    ----> 1 hidden_error*x

    ValueError: operands could not be broadcast together with shapes (3,) (6,) 

Also, w_ij is a matrix now, so the right side of the assignment must have the same shape as the left side. Luckily, NumPy takes care of this for us. If you multiply a row vector array with a column vector array, it will multiply the first element in the column by each element in the row vector and set that as the first row in a new 2D array. This continues for each element in the column vector, so you get a 2D array that has shape (len(column_vector), len(row_vector)).

    hidden_error*inputs[:,None]
    array([[ -8.24195994e-04,  -2.71771975e-04,   1.29713395e-03],
           [ -2.87777394e-04,  -9.48922722e-05,   4.52909055e-04],
           [  6.44605731e-04,   2.12553536e-04,  -1.01449168e-03],
           [  0.00000000e+00,   0.00000000e+00,  -0.00000000e+00],
           [  0.00000000e+00,   0.00000000e+00,  -0.00000000e+00],
           [  0.00000000e+00,   0.00000000e+00,  -0.00000000e+00]])

It turns out this is exactly how we want to calculate the weight update step. As before, if you have your inputs as a 2D array with one row, you can also do hidden_error*inputs.T, but that won't work if inputs is a 1D array.



### Backpropagation exercise

Below, you'll implement the code to calculate one backpropagation update step for two sets of weights. I wrote the forward pass - your goal is to code the backward pass.

Things to do

* Calculate the network's output error.
* Calculate the output layer's error term.
* Use backpropagation to calculate the hidden layer's error term.
* Calculate the change in weights (the delta weights) that result from propagating the errors back through the network.




### backprop.py


    import numpy as np


    def sigmoid(x):
        """
        Calculate sigmoid
        """
        return 1 / (1 + np.exp(-x))


    x = np.array([0.5, 0.1, -0.2])
    target = 0.6
    learnrate = 0.5

    weights_input_hidden = np.array([[0.5, -0.6],
                                     [0.1, -0.2],
                                     [0.1, 0.7]])

    weights_hidden_output = np.array([0.1, -0.3])

    ## Forward pass
    hidden_layer_input = np.dot(x, weights_input_hidden)
    hidden_layer_output = sigmoid(hidden_layer_input)

    output_layer_in = np.dot(hidden_layer_output, weights_hidden_output)
    output = sigmoid(output_layer_in)

    ## Backwards pass
    ## TODO: Calculate output error
    error = target - output

    # TODO: Calculate error term for output layer
    output_error_term = error*output*(1-output)

    # TODO: Calculate error term for hidden layer
    hidden_error_term = np.dot(output_error_term,weights_hidden_output)*hidden_layer_output*\
                        (1-hidden_layer_output)

    # TODO: Calculate change in weights for hidden layer to output layer
    delta_w_h_o = learnrate*output_error_term*hidden_layer_output

    # TODO: Calculate change in weights for input layer to hidden layer
    delta_w_i_h = learnrate*hidden_error_term*x[:,None]

    print('Change in weights for hidden layer to output layer:')
    print(delta_w_h_o)
    print('Change in weights for input layer to hidden layer:')
    print(delta_w_i_h)




## Implemeting Backpropagation

### Implementing backpropagation


For now we'll only consider a simple network with one hidden layer and one output unit. Here's the general algorithm for updating the weights with backpropagation:

* Set the weight steps for each layer to zero
    * The input to hidden weights Δw_ij=0
    * The hidden to output weights ΔW_j =0

* For each record in the training data:

    * Make a forward pass through the network, calculating the output y^ 
    * Calculate the error gradient in the output unit, δ = (y−y^ )f′ (z) where z = \sum_j W_j a_jz=∑_j W_j a_j , the input to the output unit.
    * Propagate the errors to the hidden layer δ_j^h = δ^o W_j f′(h_j )
    * Update the weight steps:
     * ΔW_j	 = ΔW_j +δ^o a_j	 
     * Δw_ij = Δw_ij +δ_j^h	 a_i	 

* Update the weights, where \etaη is the learning rate and mm is the number of records:

W_j = W_j + ηΔW_i /m
w_{ij} = w_{ij} + ηΔW_{ij} / m

* Repeat for ee epochs.


### Backpropagation exercise

Now you're going to implement the backprop algorithm for a network trained on the graduate school admission data. You should have everything you need from the previous exercises to complete this one.


Your goals here:

* Implement the forward pass.
* Implement the backpropagation algorithm.
* Update the weights



### backprop.by


    import numpy as np
    from data_prep import features, targets, features_test, targets_test

    np.random.seed(21)

    def sigmoid(x):
        """
        Calculate sigmoid
        """
        return 1 / (1 + np.exp(-x))


    # Hyperparameters
    n_hidden = 2  # number of hidden units
    epochs = 900
    learnrate = 0.005

    n_records, n_features = features.shape
    last_loss = None
    # Initialize weights
    weights_input_hidden = np.random.normal(scale=1 / n_features ** .5,
                                            size=(n_features, n_hidden))
    weights_hidden_output = np.random.normal(scale=1 / n_features ** .5,
                                             size=n_hidden)

    for e in range(epochs):
        del_w_input_hidden = np.zeros(weights_input_hidden.shape)
        del_w_hidden_output = np.zeros(weights_hidden_output.shape)
        for x, y in zip(features.values, targets):
            ## Forward pass ##
            # TODO: Calculate the output
            hidden_input = np.dot(x, weights_input_hidden)
            hidden_output = sigmoid(hidden_input)

            output = sigmoid(np.dot(hidden_output,
                                    weights_hidden_output))

            ## Backward pass ##
            # TODO: Calculate the network's prediction error
            error = y - output

            # TODO: Calculate error term for the output unit
            output_error_term = error * output * (1 - output)

            ## propagate errors to hidden layer

            # TODO: Calculate the hidden layer's contribution to the error
            hidden_error = np.dot(output_error_term, weights_hidden_output)

            # TODO: Calculate the error term for the hidden layer
            hidden_error_term = hidden_error * hidden_output * (1 - hidden_output)

            # TODO: Update the change in weights
            del_w_hidden_output += output_error_term * hidden_output
            del_w_input_hidden += hidden_error_term * x[:, None]

        # TODO: Update weights
        weights_input_hidden += learnrate * del_w_input_hidden / n_records
        weights_hidden_output += learnrate * del_w_hidden_output / n_records

        # Printing out the mean square error on the training set
        if e % (epochs / 10) == 0:
            hidden_output = sigmoid(np.dot(x, weights_input_hidden))
            out = sigmoid(np.dot(hidden_output,
                                 weights_hidden_output))
            loss = np.mean((out - targets) ** 2)

            if last_loss and last_loss < loss:
                print("Train loss: ", loss, "  WARNING - Loss Increasing")
            else:
                print("Train loss: ", loss)
            last_loss = loss

    # Calculate accuracy on test data
    hidden = sigmoid(np.dot(features_test, weights_input_hidden))
    out = sigmoid(np.dot(hidden, weights_hidden_output))
    predictions = out > 0.5
    accuracy = np.mean(predictions == targets_test)
    print("Prediction accuracy: {:.3f}".format(accuracy))




### Further Reading 


Backpropagation is fundamental to deep learning. TensorFlow and other libraries will perform the backprop for you, but you should really really understand the algorithm. We'll be going over backprop again, but here are some extra resources for you:

* From Andrej Karpathy: [Yes, you should understand backprop](https://medium.com/@karpathy/yes-you-should-understand-backprop-e2f06eab496b)

* Also from Andrej Karpathy, [a lecture from Stanford's CS231n course](https://www.youtube.com/watch?v=59Hbtz7XgjM)








