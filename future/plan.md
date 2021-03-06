Milestones
==========

## Handler rewrite

Aiming at a complete rewrite of the Handler mechanism

##### What are Handlers:

Handlers are the classes which realize the logic/action described by each XML element in the config. There is a Handler for `<VTK ...>` and for `<Solve ...>`

##### What are the problems now

- The handlers are created and initiated when the xml element is encountered. 
This means that if there is an error in an element at the end of XML file, it will crash the simulation after going for a long time
- All the Handlers are realized in one C++ file `Handlers.cpp`
- The Handlers cannot 'register' or 'broadcast' any behavior prior to being initialized.
This for instance prevents allocating enough memory at the start of the simulation to accomodate some behavior in the future.

##### What should be done:

- Handlers will be registered in a C++ Factory Pattern
- All handlers will be created and initiated when loading the xml file.

##### What would  be nice to do

- It would be nice if the factory registration and attribute check would use the same data that would be used by GUI - a preliminary version is at [doc/elements.yaml](https://github.com/CFD-GO/TCLB/blob/develop/doc/elements.yaml)

##### Q & A
- Do we want/need full DOM? Do we want to build DOM (register handlers etc) then execute? Or do we stay with read-next-when-done?
- Maybe we could create LBM-DOM library, that will handle XML file in and out. It could have API and be free of direct LBM deps and could be used for GUI handling, python bindings for reading/writing config etc
- Do we want to implement some syntax verification, that are avalible in XML? If so, that should be created and implemented BEFORE new Hanlers
  - http://www.oracle.com/technetwork/articles/srivastava-namespaces-092580.html
  - http://www.adrianmouat.com/bit-bucket/2013/11/xml-schema-validation/

## [Chimera support](https://github.com/CFD-GO/TCLB/milestones/Chimera)

Aiming at a full multi-level multi-overlapping-mesh support

##### What Chimera is

Chimera is method of CFD simulation on overlapping meshed. The meshes can be fine and coarse, can move and can intersect in wierd ways.

##### What are the problems now

There is no support for multiple meshes. There is no mesh refinement.

##### What should be done

- Calculation on several meshes (done)
- Load balance between meshes (done)
- Refinement of the time step between meshes
- Interpolation and data transfer between meshes

##### What would be nice to do

- Auto adaptation (creation/movement of mesh wrt. some criterion)

##### Q & A

- Should we maybe introduce [Thrust](http://docs.nvidia.com/cuda/thrust/#axzz4BMnH2J5A) to help with this?

## Discreet Element Method

Aiming at the support for discrete matter simulation support

##### What DEM is

DEM is the tracking and simulation of discrete matter particles immersed in the fluid. It deals both with fluid-structure interaction and contact mechanics etc.

##### What are the problems now

There is no support whatsoever for DEM in TCLB.

##### What should be done

- Introduce structures to keep the particle data
- Calculate the impact of the particles on the fluid in some fast and local way
- Exchange the particle data accross mesh partition between MPI/GPUS

##### What would be nice to do

- Load balance the interparticle calculations between GPUs or CPUs
 

## Others
 - Collide until some criterium is reached, without streaming. It would be usefull in some models 
 - NAMING CONVENTION, for physical values especially. For example: multiphase config file should be reusable in different models. That is close to be OK
 - Clean up and update in some models, for example d3q19_heat* series


