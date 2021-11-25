# 3 Ways to Implement Factory Pattern

{% embed url="https://youtu.be/xxYPFqGfImc" %}

```python
# Use if-else
import abc

class Car(object):
  @staticmethod
  @abc.abstractmethod
  def calculate_cost(tank_size):
    return

class HybridCar(Car):

  def calculate_cost(tank_size):
    return 5 * 0.5 * tank_size + 2 * 0.5 * tank_size;

class GasCar(Car):

  def calculate_cost(tank_size):
    return 5 * tank_size;

def get_concrete_classes(car_type):
    if car_type == "GasCar":
      return GasCar
    elif car_type == "HybridCar":
      return HybridCar

print(get_concrete_classes(car_type = "GasCar").calculate_cost(12))

print(get_concrete_classes(car_type = "HybridCar").calculate_cost(12))
```

```python
# Use Dictionary
import abc

class Car(object): 
  @staticmethod
  @abc.abstractmethod
  def calculate_cost(tank_size):
    return
  

class HybridCar(Car):
  
  def calculate_cost(tank_size):
    return 5 * 0.5 * tank_size + 2 * 0.5 * tank_size;

class GasCar(Car):

  def calculate_cost(tank_size):
    return 5 * tank_size;

REGISTRY = {"GasCar":GasCar, "HybridCar" : HybridCar}

print(REGISTRY["GasCar"].calculate_cost(12))

print(REGISTRY["HybridCar"].calculate_cost(12))
```

```python
# Use __init_subclass()
import abc
class Car(object):
  _registry = dict()

  def __init_subclass__(cls, car_type, **kwargs):
    cls._registry[car_type] = cls
  
  @staticmethod
  @abc.abstractmethod
  def calculate_cost(tank_size):
    return
  
  @classmethod
  def get_concrete_classes(cls, car_type):
      return cls._registry[car_type]

class GasCar(Car, car_type = "GasCar"):

  def calculate_cost(tank_size):
    return 5 * tank_size;

print(Car.get_concrete_classes(car_type = "GasCar").calculate_cost(12))

class HybridCar(Car, car_type = "HybridCar"):

  def calculate_cost(tank_size):
    return 5 * 0.5 * tank_size + 2 * 0.5 * tank_size;

print(Car.get_concrete_classes(car_type = "HybridCar").calculate_cost(12))
```

