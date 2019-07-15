# UE4 Material Override Issue Fix

## Disclaimer
This engine fix is considered experimental and may be modified in future releases. It is not recommended to use them in production yet. Use this modification at your own risk.

## Overview

This engine update fixes material overriding issue that you can encounter in Cascade. We found two scenario how to reproduce this bug.

Scenario 1

Steps to reproduce:
1. Create simple material (Mat1).
2. Create particle system with two emitters in such order: Particle Emitter (Emitter 1) and Mesh Emitter (Emitter 2).
3. Spawn particle.
4. Call SetMaterial(0, Mat1) for our particle system. // Emitter 1 will use Mat1. 
5. Call SetMaterial(1, Mat1) for our particle system. // This function fails. Emitter 2 will not use Mat1.
6. To fix this issue I found a workaround: Emitter 2 -> Required -> Add Dummy Named Material Overrides.
7. Repeat step 5. // Now Emitter 2 will use Mat1.

Note: Number of Named Material Overrides must correspond to the number of clusters in mesh, otherwise, Emitter1 update only part of materials in clusters.

Scenario 2

Steps to reproduce:
1. Create two simple material (Mat1 and Mat2).
2. Create particle system with two emitters in such order: Particle Emitter (Emitter 1) and Mesh Emitter (Emitter 2). Mesh Emitter must have two clusters.
3. Setup NamedMaterialOverrides for Emitter 1 to “test1”
4. Setup NamedMaterialOverrides for Emitter 2 to “test2” and “test3”
5. Setup Named Material Slots in such order:
		“test1” - Mat1
		“test2” - Mat1
		“test3” - Mat1
6. Spawn particle
7. Call SetMaterialByName(“test1”, Mat2) // Success
8. Call SetMaterialByName(“test2”, Mat2) // Success
9. Call SetMaterialByName(“test2”, Mat2) // Failed: Second cluster of Emitter 2 still has Mat1
10. Setup Named Material Slots in reverse order: “test3”, “test2”, “test1”.
11. Repeat steps 7-9 // in this case Emitter1 still has Mat1


## Getting Started

Just download UnrealEngine-4.22.2 folder from repository and move/replace its contents to your engine folder. You will also have to **recompile** your engine afterwards because Unreal Build Tool can incorrectly track changes of replaced files. 

### Prerequisites

You have to run you project on UE-4.22.2 compiled from sources. All prerequisites are the same as for running the usual engine. Merging with later versions of the engine was not tested, however may be possible. Please let us know about your experience in the comments. 

## Contributing

Please read [CONTRIBUTING.md](Documentation/CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/fracturedbyte/UE4-MaterialBlending/tags). 

## Authors

* **Gleb Bulgakov** - *Coding* - [FracturedByte](https://github.com/BulgakovGleb)

See also the list of [contributors](https://github.com/fracturedbyte/UE4-MaterialBlending/contributors) who participated in this project.

## Acknowledgments

* Many thanks for [PurpleBooth](https://gist.github.com/PurpleBooth/) for making templates of [CONTRIBUTION.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) and [README.md](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2) that we've used for this repo
* Inspired by the needs of UE4 developers

