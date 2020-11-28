# Packages



## Overview



### Library Packages

* Consumed by another package
* Name must match directory name
* Should provide **a focused set** of related features
*  

### Main Packages

* App entry point
* Contains a main() function
* can be in any directory
* Focus on app setup and initialization



## Working with Package

### Naming Packages

* Short and clear
* Lowercase
* No underscores
* Prefer nouns subjects not actions
* Abbreviate judiciously

### LIfecycle of a Package

* Import required packages
* Set variables to initial values
* Call init() function



You are not allowed to call `init` function explicitly. It handled as the package is initialized. 



## Preparing a package to be Used

member visibility

documenting packages

designing a package

Interface strategies

How to return the result



### Member visibility 

#### Public Scope

​	Capitalize member

​	Available to all consumers

#### Package Scope

​	Lowercase member

​	only available within package

#### Internal Package

* Can use public and package level members
* Scoped to parent package and its descendants

# How to structure your Go app