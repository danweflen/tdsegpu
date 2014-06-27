tdsegpu

We have ~ 6 "classes/interfaces" (these might be structs in practice,
but I'm representing them as classes, "operator" might not exist in
anything other than comments.  it might also use the [curiously
recurring template patern](http://en.wikipedia.org/wiki/Curiously_recurring_template_pattern)
so it can be a base class but not incurr runtime polymorphism costs):

interface `Basis`:
  - Contains: the relevant sizes of the grid.
  - Implements: the physical quantity ( **r**, {n,l,m} ) to index function.
  - Will need to be implemented for different basis.

class `Wavefunction<Basis>`:
  - Contains: 
    - the vector (on the GPU) containing the wavefunction.
    - the `Basis`.  We may or may not need template specialization for every new basis.  We will see.

interface `Operator<Basis>`:
  - Implements:
    - a way to "operate" on the wavefunction.
    - a way to determine the time dependence.

class `Hamiltonian : Operator<Basis>`:
  - Contains:
    - the matrix that represents the hamiltonian.
  - Implements the `Operator` interface.

class `Observable : Operator<Basis>`:
  - Contains:
    - a list of times, or a timestep between observations.
    - something to store the results of the observation.  (could be a file to write them out...)

class `Propagator`:
  - Contains:
    - the current wavefunction
    - the current hamiltonian
    - a list of observables
    - the Basis? (might be necessary for a split operator method?)
  - Implements:
    - the propagation of the code (crank-nicholson, etc) for a single timestep
    - the propagation of the code for the full timestep.
