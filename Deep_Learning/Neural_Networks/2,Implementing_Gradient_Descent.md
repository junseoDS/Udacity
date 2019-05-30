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
            output = None

            # TODO: Calculate the error
            error = None

            # TODO: Calculate the error term
            error_term = None

            # TODO: Calculate the change in weights for this sample
            #       and add it to the total weight change
            del_w += 0

        # TODO: Update weights using the learning rate and the average change in weights
        weights += 0

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



















