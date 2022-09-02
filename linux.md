## `grep`
### Use `grep` to search for the header file to include
Suppose that I find `AddContactMaterial` function from the documentation, I can determin which header file to include by using the powerful grep:
```
grep -nr --include \*.h -A2 "AddContactMaterial" ~/drake 
```
The output would be like:
```c++
/home/chenwang/drake/geometry/proximity_properties.h:94: * AddContactMaterial() adds general contact material properties to the given
/home/chenwang/drake/geometry/proximity_properties.h-95- * set of proximity `properties`. These are the properties required by the
/home/chenwang/drake/geometry/proximity_properties.h-96- * default point contact model. However, other contact models can opt to use
--
/home/chenwang/drake/geometry/proximity_properties.h:107:void AddContactMaterial(
/home/chenwang/drake/geometry/proximity_properties.h-108-    const std::optional<double>& dissipation,
/home/chenwang/drake/geometry/proximity_properties.h-109-    const std::optional<double>& point_stiffness,
--
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h:283:- AddContactMaterial()
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h-284-- AddRigidHydroelasticProperties()
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h-285-- AddCompliantHydroelasticProperties()
--
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h:290:AddContactMaterial() isn’t hydroelastic contact specific, but does provide a
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h-291-mechanism for setting the friction coefficients that hydroelastic and point
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h-292-contact models both use.
--
/home/chenwang/drake/multibody/plant/multibody_plant.h:1452:  ///   the function geometry::AddContactMaterial(), or
/home/chenwang/drake/multibody/plant/multibody_plant.h-1453-  /// - define it in an input URDF/SDFormat file as detailed here:
/home/chenwang/drake/multibody/plant/multibody_plant.h-1454-  ///   @ref tag_drake_hunt_crossley_dissipation.
```
Then I know that `AddContactMaterial` is defined in `proximity_properties.h`.
> Explanation to command <br>
> `grep -nr --include \*.h -A2 "AddContactMaterial" ~/drake ` <br>
> `-n`: Show relative line number in the file <br>
> `-r`: Recursively search subdirectories listed <br>
> `--include`: all `.h` files (escape with \ to avoid terminal autocompletion) <br>
> `-A`: ahead, also show lines after the search pattern (the search pattern is ahead by *N*) <br>
> `-B`: below, also show lines before the search pattern (the search pattern is below by *N*) <br>
> `-C`: context, include both lines after and below the search pattern by *N* <br>

## `find`
Check (`the missing semester`)[https://missing.csail.mit.edu/2020/shell-tools/]

```python
# Find all directories named src
find . -name src -type d
# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'
```

## input and output stream

1. `< file` and `> file`: rewire the input and output streams of a program to a file respectively
2. `>>` append to a file