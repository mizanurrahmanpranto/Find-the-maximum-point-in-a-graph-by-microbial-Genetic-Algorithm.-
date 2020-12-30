# Find-the-maximum-point-in-a-graph-by-microbial-Genetic-Algorithm.-

A genetic algorithm is a search heuristic that is inspired by Charles Darwin’s theory of natural evolution. This algorithm reflects the process of natural selection where the fittest individuals are selected for reproduction in order to produce offspring of the next generation.
In computer science and operations research, a genetic algorithm (GA) is a metaheuristic inspired by the process of natural selection that belongs to the larger class of evolutionary algorithms (EA). Genetic algorithms are commonly used to generate high-quality solutions to optimization and search problems by relying on biologically inspired operators such as mutation, crossover and selection.



Microbial Genetic Algorithm

Microbial Genetic Algorithm (MGA) is a simple variant of genetic algorithm and is inspired by bacterial conjugation for evolution. Here, I have discussed and analyzed variants of this less exploited algorithm on known benchmark testing functions to suggest a suitable choice of mutation operator.

I am going to analyze how the conventional Genetic Algorithm can be stripped down and reduced to its basics. I present a minimal, modified version that can be interpreted in terms of horizontal gene transfer, as in bacterial conjugation. Whilst its functionality is effectively similar to the conventional version, it is much easier to program, and recommended for both teaching purposes and practical applications. Despite the simplicity of the core code, it effects Selection, (variable rates of) Recombination, Mutation, Elitism and Geographical Distribution.



Darwinian evolution assumes a population of replicating individuals, with three properties: Heredity, Variation, and Selection. If we take it that the population size neither shrinks to zero, nor shoots off to infinity -- and in artificial evolution such as a genetic algorithm we typically keep the population size constant -- then individuals will come and go, whilst the makeup of the population changes over time. Heredity means that new individuals are similar to the old individuals, typically through the offspring inheriting properties from their parents. Variation means that new individuals will not be completely identical to those that they replace. Selection implies some element of direction or discrimination in the choice of which new individuals replace which old ones. In the absence of selection, the population will still change through random genetic drift, but Darwinian evolution is typically directed: in the natural world through the natural differential survival and mating capacities of varied individuals in the environment, in artificial evolution through the fitness function that reflects the desired goal of the program designer. Any system that meets the three basic requirements of Heredity, Variation and Selection will result in evolution. Aim here is to work out the most minimal framework in which that can be achieved, for two reasons: firstly theoretical, to see what insights can be gained when the evolutionary method is stripped down to its bare bones; secondly didactic, to demystify the Genetic Algorithm (GA) and present a version that is trivially easy to implement. 

GAs Stripped to the Minimum The most creative and challenging parts of programming a GA are usually the problem-specific aspects. 


The Microbial Genetic Algorithm 127 genotypic expression of potential solutions or phenotypes, and the genotype-phenotype mapping.h

Though this may be the most interesting part, it is outside the remit of our focus here on those evolutionary mechanisms that can be generic and largely independent of the specific nature of any problem. So we will assume that the programmer has found some suitable form of genotype expression, such that the genetic material of a population can be maintained and updated as the GA progresses; for the purposes of illustration we shall assume that there is a population size P of binary genotypes length N. We assume that all the hard problem-specific work of translating any specific genotype in the population into the corresponding phenotype, and evaluating its fitness, has been done for us already. Given these premises, we wish to examine the remaining GA framework to see how it can be minimized.
genotypic expression of potential solutions or phenotypes, and the genotype-phenotype mapping.
 Though this may be the most interesting part, it is outside the remit of our focus here on those evolutionary mechanisms that can be generic and largely independent of the specific nature of any problem. So we will assume that the programmer has found some suitable form of genotype expression, such that the genetic material of a population can be maintained and updated as the GA progresses; for the purposes of illustration we shall assume that there is a population size P of binary genotypes length N. We assume that all the hard problem-specific work of translating any specific genotype in the population into the corresponding phenotype, and evaluating its fitness, has been done for us already. Given these premises, we wish to examine the remaining GA framework to see how it can be minimized. 

Generational and Steady State GAs Traditionally GAs were first presented in generational form. Given the current population of size P, and their evaluated fitness, a complete new generation of the same size was created via some selective and reproductive process, to replace the former generation in one fell swoop. A GA run starts with an initialized population, and then loops through this process for many successive generations. This would roughly correspond to some natural species that has just one breeding season, say once a year, and after breeding the parents die out without a second chance. There are many natural species that do not have such constraints, with birth and death events happening asynchronously across the population, rather than to the beat of some rhythm. Hence the Steady State GA, which in its simplest form has as its basic event the replacement of just one individual from P by a single new one (a variant version that we shall not consider further would replace two old by two new). The repetition of this event P times is broadly equivalent to a single generation of the Generational GA, with some subtle differences. In the Generational version, no individual survives to the next generation, although some will pass on genetic material. In the Steady State version, any one individual may, through luck or through being favoured by the selective process, survive unchanged for many generation-equivalents. Others, of course may disappear much earlier; the average lifetime of each individual will be P birth/death events, as in the generational case, but there will be more variance in these lifetimes. There are pragmatic considerations for the programmer. The core piece of code in the Steady State version, will be smaller, although looped through P times more, than in the Generational GA. The Generational version needs to be coded so that, in effect, P birth/death events occur in parallel, although on a sequential machine this will have to be emulated by storing a succession of consequences in a temporary array; typically two arrays will be needed for this-generation and next-generation. If the programmer does have access to a cluster of processors working in parallel, then a Steady State version can farm out the evaluation of each individual (usually the computational bottleneck) to a different processor.    

As we said at the beginning, we have some population, every time in the evolution, we will randomly draw 2 DNA from the pop, and then compare their fitness, we defined fitness high as winner, We won't touch any winner's DNA, only the loser will be tampered with, such as cross over and mutate.
Through this process, we do not have to worry about sometimes mutation mutation, those original good pop loss, with the MGA algorithm, winner will always be retained. The Elitism problem in GA is cleverly solved in this way.

This time, we will interpret the MGA algorithm by finding the largest point in the curve. The structural framework in Class remains basically the same, except for the lack of select functionality. Because we're going to write the select function in evolve. It'll be easier.

class MGA:
    def crossover(self, loser_winner):

    def mutate(self, loser_winner):

    def evolve(self, n):

In this function, two hands go into the bag to grab the DNA out of the bag n times, and then evaluate each time the fitness of the grabbed DNA. So we can define which of these two is the winner and which is the loser.For the loser, we cross over part of the winner's DNA to the loser and expect the loser to get a little better with the winner. Then the loser goes through a fraction of the odds to mutate. So the algorithm in evolve is written like this:

class MGA:
    def evolve(self, n):    # nature selection wrt pop's fitness
        for _ in range(n):  # random pick and compare n times
            sub_pop_idx = np.random.choice(np.arange(0, self.pop_size), size=2, replace=False)
            sub_pop = self.pop[sub_pop_idx]             # pick 2 from pop
            product = F(self.translateDNA(sub_pop))
            fitness = self.get_fitness(product)
            loser_winner_idx = np.argsort(fitness)
            loser_winner = sub_pop[loser_winner_idx]    # the first is loser and second is winner
            loser_winner = self.crossover(loser_winner)
            loser_winner = self.mutate(loser_winner)
            self.pop[sub_pop_idx] = loser_winner


Both Crossover and mutate follow the above statement, only in winner_loser. Because the DNA here is encoded in binary. If you don't understand what's going on here, see the instructions in this tutorial.

class MGA:
    def crossover(self, loser_winner):      # crossover for loser
        cross_idx = np.empty((self.DNA_size,)).astype(np.bool)
        for i in range(self.DNA_size):
            cross_idx[i] = True if np.random.rand() < self.cross_rate else False  # crossover index
        loser_winner[0, cross_idx] = loser_winner[1, cross_idx]  # assign winners genes to loser
        return loser_winner

    def mutate(self, loser_winner):         # mutation for loser
        mutation_idx = np.empty((self.DNA_size,)).astype(np.bool)
        for i in range(self.DNA_size):
            mutation_idx[i] = True if np.random.rand() < self.mutate_rate else False  # mutation index
        # flip values in mutation points
        loser_winner[0, mutation_idx] = ~loser_winner[0, mutation_idx].astype(np.bool)
        return loser_winner


Finally, put on the training cycle, and we're done.


ga = MGA(...)
for generation in range(N_GENERATIONS):
    ga.evolve(n=5)

