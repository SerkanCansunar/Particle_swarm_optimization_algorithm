## Bu projenin kodlarına [buraya tıklayarak](https://colab.research.google.com/drive/1u3lI6G9uR5-1yDqA-DenHNCbXGiqxUIG?usp=sharing) ulaşabilirsiniz.
# Particle_swarm_optimization_algorithm

```
import numpy as np
def rastrigin(x):
    A = 10
    return A * len(x) + sum([(xi ** 2 - A * np.cos(2 * np.pi * xi)) for xi in x])

class PSO:
    def __init__(self, func, dim, swarm_size, max_iter, bounds, w=0.5, c1=2, c2=2):
        self.func = func
        self.dim = dim
        self.swarm_size = swarm_size
        self.max_iter = max_iter
        self.bounds = bounds
        self.w = w
        self.c1 = c1
        self.c2 = c2
        self.swarm = [Particle(dim, bounds) for _ in range(swarm_size)]#sınıfı kullanılarak oluşturulan parçacıklardan oluşan bir listedir
        self.gbest_position = np.random.uniform(bounds[0], bounds[1], dim)#tüm parçacıklar arasındaki en iyi konumu tutar.
        self.gbest_value = float('inf')# bu konuma karşılık gelen en iyi fonksiyon değerini tutar.
        self.update_gbest()# metodu çağrılarak, sürünün en iyi konumu ve değeri güncellenir.

    def update_gbest(self):
        for particle in self.swarm:
            if particle.pbest_value < self.gbest_value:
                self.gbest_value = particle.pbest_value
                self.gbest_position = particle.pbest_position.copy()
                #global en iyi konumu günceller.

    def optimize(self):
        for _ in range(self.max_iter):
            for particle in self.swarm:
                particle.update_velocity(self.gbest_position, self.w, self.c1, self.c2)
                particle.update_position(self.bounds)
                particle.evaluate(self.func)
                if particle.pbest_value < self.gbest_value:
                    self.gbest_value = particle.pbest_value
                    self.gbest_position = particle.pbest_position.copy()
#max iterasyon kadar iterasyon yapılır.
#Her iterasyonda
#Parçacığın hızı güncellenir
#Parçacığın pozisyonu güncellenir.
class Particle:
    def __init__(self, dim, bounds):
        self.position = np.random.uniform(bounds[0], bounds[1], dim)
        self.velocity = np.random.uniform(-1, 1, dim)
        self.pbest_position = self.position.copy()
        self.pbest_value = float('inf')
         #parçacığın hızını rastgele başlatıyoruz.

    def evaluate(self, func):
        fitness = func(self.position)
        if fitness < self.pbest_value:
            self.pbest_value = fitness
            self.pbest_position = self.position.copy()
            #fitness değeri hesaplanır.

    def update_velocity(self, gbest_position, w, c1, c2):
        r1 = np.random.rand(len(self.position))
        r2 = np.random.rand(len(self.position))
        cognitive_velocity = c1 * r1 * (self.pbest_position - self.position)
        social_velocity = c2 * r2 * (gbest_position - self.position)
        self.velocity = w * self.velocity + cognitive_velocity + social_velocity
        #parçacığın hızını günceller


    def update_position(self, bounds):
        self.position += self.velocity
        self.position = np.clip(self.position, bounds[0], bounds[1])
        #yeni pozisyonu  günceller.
dim = 2  # Problem boyutu
swarm_size = 30  # Sürü büyüklüğü
max_iter = 100  # Maksimum iterasyon sayısı
bounds = (-5.12, 5.12)  # Çözüm uzayı sınırları
pso = PSO(rastrigin, dim, swarm_size, max_iter, bounds)
pso.optimize()

print(f"En iyi çözüm: {pso.gbest_position}")
print(f"En iyi çözüm değeri: {pso.gbest_value}")
```
