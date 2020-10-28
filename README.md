# pneAsync

## A handy tool to promisify nested arrays

***Usage :***

Suppose args is an array of objects which define some category, each object having categoryId as a property.
Suppose someAsyncFunction takes categoryId of elements of the above array and returns an array of sub category objects, each having subCategoryId as a  
property.
Suppose anotherAsyncFunction takes subCategoryId of each element of subcategory array and return an array of products.

If we want to get all subcategories and products corresponding to each category, we can run a for loop inside another with awaits before each async function 
call
and collect the results. However, this will make the code synchronous.

To resolve this, use pneAsync.

Arguments of pneAsync are : 
1. array to iterate over.
2. factorList - it is an array each of whose element is an object of the form - 
              {
                arg: // argument to pass to async function for each element,
                foo: // async function to call with arg,
                typeOfArg: // if it is === 'value' then the arg value will be used as the argument of foo else elem[arg] will be used
              }
              Length of factor list decides the depth of the nesting. So, each element of the factorList is an object containing arg, foo and typeOfArg for
              increasing depths of nesting

    ```
      const list = [{id: 1, name: 'a'}, {id: 2, name: 'b'}, ...];
      const factors = [{arg: 'id', typeOfArg: 'property', foo: someAsyncFoo}, {arg: 'subCatId', typeOfArg: 'property', foo: anotherAsyncFoo}];
      
      async function anyFunction(arr){
        return await pneAsync(list, factors)
      }
    ```
                  
