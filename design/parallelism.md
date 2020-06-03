# Dissolve Parallel Tech Spec

## Introduction

### Purpose
This document lays out the current and future technical requirements for the parallel architecture of the Dissolve project.

### Background

Dissolve is an atomistic simulation code for the interrogation and analysis of neutron scattering data. Since the simulation boxes (collections of atoms) that it produces must be representative of the physical systems measured experimentally, particle numbers of 1 million and beyond must be treatable. This is not practical on a single-core machine, or even a modestly powerful workstation, and so suitable parallel technology must be implemented.

### Current Implementation

At present (v0.7.0) Dissolve utilises the widely-available OpenMPI standard to achieve parallelism across multiple CPU cores over multiple physical processing enclosures. Each slave process maintains its own copy of all simulation data (replicated data strategy) leading to high memory requirements.

Dissolve's original design was to allow partitioning of processors into sub-groups in order to permit:

1. Fine-grained control over partitioning of workload in order to achieve balance between processing time and communication overhead.
2. Forking of the main loop to allow simultaneous calculations targetting different atomic configurations or modules

1) is particularly important in the implementation of parallel Monte Carlo techniques, where trial moves must be distributed throughout the simulation box in such a way that they are independent of each other. This is achievable, but heavily system dependent (size, molecule type, etc.) and requires adaptive partitioning of the available processes. As a consequence of 2), each main loop iteration requires a broadcast of all simulation data between processes to ensure that each process has up-to-date information, forcing the requirement for T::broadcast() and T::equality() (enforced by inheriting from GenericItemBase) for each class object that needs to be distributed.

## Requirements

### Code

- A single codebase must be maintained for serial, GUI and parallel versions, with parallelism built-in when requested through the build generator (CMake).
- The GUI version will remain as a serial code, as its scope covers only the set-up and analysis of simulations, and the running of small tutorial systems that do not require to be run in parallel.

### General Approach

- The ability to partition available processes into task-groups for certain algorithms is beneficial.
- Scaling over CPU- and GPU-based multi-core systems should be considered.
- Replicated data strategies over CPU cores should not be employed - such approaches are not feasible for systems of 1 million particles and beyond.

### System Architecture

- Parallelisation of the Linux non-GUI version is essential. 
- Parallelisation of the OSX non-GUI version is desirable.
- Parallelisation of the Windows non-GUI version is beneficial, but not essential. 
- Parallelisation of the GUI version is desirable, but not essential.
