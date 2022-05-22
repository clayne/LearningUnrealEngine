2020-03-09_21:48:56

# UFUNCTION

The `UFUNCTION` decorator macro exposes a C++ function to the Unreal Engine reflection system.
It turns the function into a UFunction.

```c++
UCLASS()
class AMyActor : public AActor
{
    GENERATED_BODY()
public:
    UFUNCTION()
    void MyFunction();
}
```

UFunction does not support *overloading*, i.e., function member names must be unique.
UFunction does not support *template*, but there might be a work-around based on custom thunks and/or wildcards. More research needed.
UFunction does not support *reference return* types. 
UFunction does not support *FStruct pointer return* types.
UFunction does not support *UObject reference* parameters.
Some of the limitations may be for UFunctions exposed to Blueprint only.

[[2020-03-10_21:12:12]] [USTRUCT](./USTRUCT.md)  

A number of function specifiers can be added to set properties on the function.
- `BlueprintCallable`: The function can be called from a Blueprint Visual Script.
- `BlueprintPure`: The function can be called from a Visual Script, will note have an execution pin.
- `BlueprintImplementableEvent`: The function is only declared and not implemented in C++, the function definition is provided by a Blueprint class inheriting from the C++ class. I assume `Blueprintable` must be provided on the C++ class for this to make sense.
- `BlueprintNativeEvent`: Like `BlueprintImplementableEvent`, but with a fallback C++ definition in case no Blueprint implementation is made. A separate C++ function with the same name but with `_Implementation` at the end is declared automatically and this is where the C++ implementation should be written. I assume `Blueprintable` must be provided on the class for this to make sense.

A non-C++-`const` `BlueprintCallable` function will have an execution pin.
A C++-`const` `BlueprintCallable` function will not have an execution pin.

The only difference between `BlueprintPure` and `BlueprintCallable` is that `BlueprintCallable` only updates its outputs once when the execution runs through it. `BlueprintPure` will rerun the function and update the output for every single thing that tries to get a value from it.

Most types that can be used in Blueprints can be used for parameters and return types.
A parameter taken by reference becomes an output pin on the Visual Script node.
To create an output pin pass a by-reference parameter to the function.
If you want it to be an input pin instead then add `UPARAM(Ref)` before the parameter type.
This is useful for modifying the state of a UStruct variable.
(
Though be aware that the Blueprint VM will still copy the instance in and out of the VM's memory, so it only looks like a reference in that we can update the source struct.
This matters if the struct has members that aren't copied by the assignment operator / copy constructor / and/or whatever else the Blueprint VM may decide to do the copy with.
)
```cpp
UFUNCTION(BlueprintCallable, Category = "MyCategory")
void MyFunction(UPARAM(Ref) FMyType& InOutMyParameter);
```

`UPARAM(Ref)` Can also be put on the return value:
```cpp
UFUNCTION(BlueprintCallable, Category = "MyCategory")
UPARAM(Ref) FMyType& MyGetterFunction();
```

Using `UPARAM`we can also set the name of the output pin:
```cpp
UPARAM(DisplayName = "Num Widgets") int32 GetNumWidgets();
```

To return a pointer to a UObject, make a parameter that is a reference to a pointer.
```cpp
UFUNCTION(BlueprintCallable, Category = "MyCategory")
void MyFunction(UMyType*& OutReturn);
```


If you have a Function with an Array parameter that you often want to pass an empty array to then you can add the `AutoCreateRefTerm="MyParameter"` Meta Specifier to the function. Then that pin can be left unconnected in a Blueprint graph.
For example:
```cpp
UFUNCTION(BlueprintCallable, Category = "MyCategory", Meta=(AutoCreateRefTerm="MyArray")
float Sum(const TArray<float>& MyArray)
```

[[2021-03-13_11:48:31]] [Blueprint events in C++](./Blueprint%20events%20in%20C++.md)  
[[2020-03-09_21:34:05]] [UCLASS](./UCLASS.md)  
[[2020-03-09_21:43:36]] [UPROPERTY](./UPROPERTY.md)  
[[2020-03-10_21:12:12]] [USTRUCT](./USTRUCT.md)  
