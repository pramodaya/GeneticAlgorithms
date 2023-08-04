
# Flocking Algorithm: Simulating Collective Behavior in Nature-inspired Systems


The Flocking Algorithm is a computational model inspired by collective behavior observed in nature, such as birds flying in formations or fish swimming in schools. It simulates emergent behavior in groups of entities by applying three principles: alignment, cohesion, and separation.


![flocking-behavior-rules](https://github.com/pramodaya/Article-List/assets/19555470/6bc6190f-43c9-4ffc-b0d0-3cdf6a5cacbe)

Each entity adjusts its movement based on its neighbors’ average direction, moves towards the center of mass of its neighbors, and avoids getting too close to others. This simple set of rules leads to visually captivating and fluid flocking patterns, finding applications in computer graphics, artificial intelligence, and robotics, where it enables realistic animations, cooperative decision-making, and the development of autonomous robotic swarms for various tasks.


https://www.youtube.com/watch?v=V4f_1_r80RY&ab_channel=NationalGeographic


![Untitled](https://github.com/pramodaya/GeneticAlgorithms/assets/19555470/dbf4d5e4-e6c0-4651-9f13-f255b07c8240)



## Implementation

```
import pygame
import random
import math

pygame.init()

# Screen dimensions
WIDTH, HEIGHT = 800, 600

# Colors
BLACK = (0, 0, 0)

# Flocking parameters
NUM_ENTITIES = 100
MAX_SPEED = 2
PERCEPTION_RADIUS = 50
SEPARATION_DISTANCE = 25


# Entity class
class Entity:
    def __init__(self, x, y):
        self.position = pygame.Vector2(x, y)
        self.velocity = pygame.Vector2(random.uniform(-1, 1), random.uniform(-1, 1))
        self.velocity.scale_to_length(MAX_SPEED)

    def update(self, flock):
        self.position += self.velocity

        self.wrap_edges()

        average_velocity = pygame.Vector2(0, 0)
        average_position = pygame.Vector2(0, 0)
        average_separation = pygame.Vector2(0, 0)
        num_neighbors = 0

        for other in flock:
            if other == self:
                continue

            distance = self.distance_to(other)

            if distance < PERCEPTION_RADIUS:
                average_velocity += other.velocity
                average_position += other.position

                if distance < SEPARATION_DISTANCE:
                    diff = self.position - other.position
                    diff.scale_to_length(1 / distance)
                    average_separation += diff

                num_neighbors += 1

        if num_neighbors > 0:
            average_velocity /= num_neighbors
            average_position /= num_neighbors
            average_separation /= num_neighbors

        self.velocity += average_velocity * 0.02
        self.velocity += average_position * 0.01
        self.velocity -= average_separation * 0.03

        self.velocity.scale_to_length(MAX_SPEED)

    def wrap_edges(self):
        if self.position.x < 0:
            self.position.x = WIDTH
        if self.position.y < 0:
            self.position.y = HEIGHT
        if self.position.x > WIDTH:
            self.position.x = 0
        if self.position.y > HEIGHT:
            self.position.y = 0

    def distance_to(self, other):
        return math.sqrt((self.position.x - other.position.x) ** 2 + (self.position.y - other.position.y) ** 2)


# Create the flock
flock = [Entity(random.randint(0, WIDTH), random.randint(0, HEIGHT)) for _ in range(NUM_ENTITIES)]

# Initialize the screen
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Flocking Simulation")


def main():
    running = True
    clock = pygame.time.Clock()

    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        screen.fill(BLACK)

        for entity in flock:
            entity.update(flock)
            pygame.draw.circle(screen, (255, 255, 255), (int(entity.position.x), int(entity.position.y)), 3)

        pygame.display.flip()
        clock.tick(60)

    pygame.quit()


if __name__ == "__main__":
    main()

```
Run the simulation: Save your Python script and run it. A window will pop up displaying the flocking simulation with entities moving around and exhibiting flocking behavior.

And that’s it! You now have a basic flocking simulation implemented using Python and Pygame. You can further refine and customize the simulation by adjusting the flocking parameters and adding more behaviors.


Medium Article Link - https://medium.com/@pramodayajayalath/flocking-algorithm-simulating-collective-behavior-in-nature-inspired-systems-dc6d7fb884cc


