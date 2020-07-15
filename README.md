
# Mini-Projet Linear Regression

L'objectif de ce projet était de construire un modèle de régression linéaire en utilisant numpy.


```python
%matplotlib inline

#imports
from numpy import *
import matplotlib.pyplot as plt
```

#### Importer le data
Ici, nous utilisons un ensemble de données avec deux colonnes contenant respectivement le nombre d'heures étudiées et les résultats des étudiants.


```python
points = genfromtxt('data.csv', delimiter=',')

#Extraire des colonnes
x = array(points[:,0])
y = array(points[:,1])

#Tracer l'ensemble de données
plt.scatter(x,y)
plt.xlabel('Hours of study')
plt.ylabel('Test scores')
plt.title('Dataset')
plt.show()
```


![png](linear-regression-demo_files/linear-regression-demo_3_0.png)


#### Définition des hyperparamètres


```python
#hyperparamters
learning_rate = 0.0001
initial_b = 0
initial_m = 0
num_iterations = 10
```

#### Définir la fonction de coût


```python
def compute_cost(b, m, points):
    total_cost = 0
    N = float(len(points))
    
    #Compute sum of squared errors
    for i in range(0, len(points)):
        x = points[i, 0]
        y = points[i, 1]
        total_cost += (y - (m * x + b)) ** 2
        
    #Return average of squared error
    return total_cost/N
```

#### Définir les fonctions de descente de gradient


```python
def gradient_descent_runner(points, starting_b, starting_m, learning_rate, num_iterations):
    b = starting_b
    m = starting_m
    cost_graph = []

    #For every iteration, optimize b, m and compute its cost
    for i in range(num_iterations):
        cost_graph.append(compute_cost(b, m, points))
        b, m = step_gradient(b, m, array(points), learning_rate)

    return [b, m, cost_graph]

def step_gradient(b_current, m_current, points, learning_rate):
    m_gradient = 0
    b_gradient = 0
    N = float(len(points))

    #Calculate Gradient
    for i in range(0, len(points)):
        x = points[i, 0]
        y = points[i, 1]
        m_gradient += - (2/N) * x * (y - (m_current * x + b_current))
        b_gradient += - (2/N) * (y - (m_current * x + b_current))
    
    #Update current m and b
    m_updated = m_current - learning_rate * m_gradient
    b_updated = b_current - learning_rate * b_gradient

    #Return updated parameters
    return b_updated, m_updated
```

#### Run gradient_descent_runner() to get optimized parameters b and m


```python
b, m, cost_graph = gradient_descent_runner(points, initial_b, initial_m, learning_rate, num_iterations)

#Print optimized parameters
print ('Optimized b:', b)
print ('Optimized m:', m)

#Print error with optimized parameters
print ('Minimized cost:', compute_cost(b, m, points))
```

    Optimized b: 0.0296393478747
    Optimized m: 1.47741737555
    Minimized cost: 112.655851815


#### Plotting the cost per iterations


```python
plt.plot(cost_graph)
plt.xlabel('No. of iterations')
plt.ylabel('Cost')
plt.title('Cost per iteration')
plt.show()
```


![png](linear-regression-demo_files/linear-regression-demo_13_0.png)


Gradient descent converge vers le minimum local après 5 itérations

#### traçage la ligne du meilleur ajustement


```python
#Plot dataset
plt.scatter(x, y)
#Predict y values
pred = m * x + b
#Plot predictions as line of best fit
plt.plot(x, pred, c='r')
plt.xlabel('Hours of study')
plt.ylabel('Test scores')
plt.title('Line of best fit')
plt.show()
```


![png](linear-regression-demo_files/linear-regression-demo_16_0.png)

