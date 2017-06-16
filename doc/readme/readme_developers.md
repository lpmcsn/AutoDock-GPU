This is the documentation for developers: source code structure, build instructions, tools required, etc.

# Source-code structure

## Main structure
```
.
+-- bin
+-- common
+-- device
+-- doc
+-- host
+-- input
+-- wrapcl


```

**bin**: binary files.

| File                     | Description                                                                   |
|--------------------------|-------------------------------------------------------------------------------|
| `ocladock_<type>_<N>wi`  | Binary file for `<type>` (cpu, gpu) device with `<N>` (16, 32, 64) work items |


**common**: common header files for host and device.

| File                | Description                                                                       |
|---------------------|-----------------------------------------------------------------------------------|
| [calcenergy_basic.h](common/calcenergy_basic.h)  | Basic defines and macros for energy calculation      |
| [defines.h](common/defines.h)           | Basic defines for energy calculation and kernels optimization |


**device**: source files containing kernels.

| File                 | Description                                                                   |
|----------------------|-------------------------------------------------------------------------------|
| [auxiliary_genetic.cl](device/auxiliary_genetic.cl) | Auxiliary functions for energy calculation     |
| [calcenergy.cl](device/calcenergy.cl)   | Main function for energy calculation                       |
| [kernel1.cl](device/kernel1.cl) | `gpu_calc_initpop`: calculates the energy of initial population    |
| [kernel2.cl](device/kernel2.cl) | `gpu_sum_evals`: sums the evaluation counter states                |
| [kernel3.cl](device/kernel3.cl) | `perform_LS`: performs the local search                            |
| [kernel4.cl](device/kernel4.cl) | `gpu_gen_and_eval_newpops`: performs the genetic generation        |

**doc**: documentation files.

**host**:

| File                 | Description                                                                   |
|----------------------|-------------------------------------------------------------------------------|
| calcenergy{[.h](host/inc/calcenergy.h)}{[.cpp](host/src/calcenergy.cpp)} | Auxiliary functions for energy calculation     |


```
.
+-- host
|   +-- inc
    |   +-- calcenergy.h
    |   +-- getparameters.h
    |   +-- miscellaneous.h
    |   +-- performdocking.h
    |   +-- processgrid.h
    |   +-- processligand.h
    |   +-- processresult.h
|   +-- src
    |   +-- calcenergy.cpp
    |   +-- getparameters.cpp
    |   +-- main.cpp
    |   +-- miscellaneous.cpp
    |   +-- performdocking.cpp
    |   +-- processgrid.cpp
    |   +-- processligand.cpp
    |   +-- processresult.cpp
```

### input

```
>
+-- input
|   +-- 1stp
|   +-- 3ce3
```


### wrapcl

```
.
+-- wrapcl
|   +-- inc
    |   +-- BufferObjects.h
    |   +-- CommandQueues.h
    |   +-- commonMacros.h
    |   +-- Contexts.h
    |   +-- Devices.h
    |   +-- ImportBinary.h
    |   +-- ImportSource.h
    |   +-- Kernels.h
    |   +-- listAttributes.h
    |   +-- Platforms.h
    |   +-- Programs.h
|   +-- src
|   +-- BufferObjects.cpp
|   +-- CommandQueues.cpp
|   +-- Contexts.cpp
|   +-- Devices.cpp
|   +-- ImportBinary.cpp
|   +-- ImportSource.cpp
|   +-- Kernels.cpp
|   +-- listAttributes.cpp
|   +-- Platforms.cpp
|   +-- Programs.cpp
```

# Compilation

## Basic
```zsh
make DEVICE=<TYPE> NUMWI=<NWI>
```
`<TYPE>` : CPU, GPU.

`<NUMWI>` : 16, 32, 64

After successful compilation, the host binary `ocladock_<type>_<N>wi` is placed under [bin](./bin).

`type` denotes the accelerator chosen: `cpu` or `gpu`.

`N` denotes the OpenCL work-group size: `16`, `32`, or `64`.

## All available options
```zsh
make DEVICE=<TYPE> NUMWI=<NWI> CONFIG=<CFG> DOCK_DEBUG=<Y/N> REPRO=<Y/N>
```
| Argument    | Description                           | Possible values          |
|-------------|---------------------------------------|--------------------------|
| DEVICE      | OpenCL device type                    | `<TYPE>`: CPU, GPU       |
| NUMWI       | Number of work items per work group   | `<NWI>` : 16, 32, 64     |
| CONFIG      | Host configuration                    | `<CFG>` : DEBUG, RELEASE |
| DOCK_DEBUG  | Enable debug info from host & device  | `<Y/N>` : YES, NO        |
| REPRO       | Reproduce results (remove randomness) | `<Y/N>` : YES, NO        |

## Configuration file
Check the configurations in the project's [Makefile](../Makefile).

# Requirements