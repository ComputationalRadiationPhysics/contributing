
# General C++ Style Guide

## Namings
  - oc-token = open close toke such as parenthesis `[,]`, `(,)`, `{,}` and `<,>`
  - prefix tag = all characters before a *oc-token*
  - operator = math operator symbols like `+`, `-`, `/` ...
  - regex = regular expression to indicate where spaces must be placed
    - `{...}` = optional definition of `...`
    - `[:...:]` = explicit placement of `...`

## 1. Files and Encoding
  - files **must** be encoded in ASCII
  - LF ( line feed, `\n` , `0x0A` ) **must** be used for new lines
  - each file must contain
    - copyright notice
      - the corresponding license header, author(s) + current year ( if changed - and a commit of a file means a change )
      - you **never remove** an author or a year, you *only add to them*
    - a `#pragma once` as guard to prevent double includes ( do **not** use `#ifdef` guards )
  - includes:
    - order from the most specific/specialized to the most general one
    - objects used from an include **can** be added in a comment but should **at most** be used for stable third party includes (can be used for includes from `C++ STL`, `Boost`, ...)
```C++
#include "myproject/myTypes.hpp"
#include "myproject/unStableApiTraits.hpp" // is_const<>, is_trait<> -> this should be avoided
#include <boost/mpl/vector.hpp>
#include <boost/type_traits.hpp> // is_void<>, is_union<>
```


## 2. Project
  - each project must have a global namespace which covers all the code but **not** includes
  - the namespace structure
    - is equal to the folder structure
    - a file can have *additional* namespaces which are not represented by folders
  - the `include` folder contains as a first folder the project name e.g., `include/picongpu`, `include/haseongpu`
  - folder names must be written in camel case and begin with a lower case letter
  - the project name **must** begin with but **can** be fully composed of lower case letters (for folders and namespaces);
    since this is a simplifcation for coding, the official spelling might differ


## 3. Global Rules
  - wrap a line after column 80
  - **no TABS**, use four (**4**) spaces per indentation level
  - no code alignment (only indention)
  - names are always written in camel case syntax e.g., `ThisIsCamelCase`, `thisIsAlsoCamelCase` ( except: macros and defines )
  - if there is no content between `{ }`, `< >`, `[ ]` or `( )`
    - the *oc-token* token **must** be on the same line
    - one space between both *oc-tokens* ( regex: `<[:space:]>` )
  - *oc-token* `{ }`
    - `{` is always placed on a new line ( same indentation level than the line before ) and followed by a new line
    - `}` is always placed on the same indention level than the opening token
  - *oc-token* `( )`
    - for function / method signatures
      - `(` is placed after a *prefix tag* without a space before e.g., `method( );`
      - `(` must followed by a new line ( except: if `( )` empty )
      - `)` is always placed on the same indention level than the opening token ( except: if `( )` is empty )
    - for expressions e.g., `( a + 1 ) * 5;`
      - can placed on one line
      - after 80 characters a new line **must** follow
      - after first new line start indention, see section [Operators](cpp.md#8-Operators)
    - for function calls e.g., `foo( this );`
      - for one function parameter use one line
      - for more than one function parameter place each indented on a new line see section [Function Calls](cpp.md#9-Function Calls)
  - usage of end of line spaces is **not** allowed
  - it is not allowed to place a space before `;`
  - semicolon `;` is always followed by a new line


## 4. Comments
  - multi line comments
    - begin with `/*` ( or `/**` for doxygen) followed by a space ( regex: `/*[:space:]` )
    - end with `*/` on a **new line**
    - each line between begin and end starts with ` *` ( regex: `[:indentation:][:space:]*{[:space:]comment}` )
  - single line comments begin with `//` followed by a space ( regex: `//[::space:]` )
  - it is not allowed to format comments with long symbol repetition e.g., `// ################################`
```C++
/*#################################################
 *#   @brief block formating this is not allowed  #
 *#                                               #
 *#   text text                                   #
 *#################################################
 */
```
  - doxygen inside comments is highly encouraged see section [doxygen](cpp.md#5-Doxygen-Comments)
```C++
/* this is a multi line comment
 * where the end tag is on a new line
 */

// this is a single line comment
```


## 5. Doxygen Comments
   - multi line comments **must** be used for doxygen comments
   - brief
     - begins with `/**`
     - should be a single line comment
   - long description
     - is separated with a empty line
     - is not aligned with the brief line
   - use `@` for doxygen commands e.g., `@param`, `@return`, `@{`
   - commands which should only rarely be used e.g., for code parts that are included from (license compatible!) third party projects (use the copyright header for full files or files that mainly consist of the external code)
     - `@author`
     - `@date`
     - `@copyright`
   - use `@f[` / `@f]` or other formula commands to describe equations in latex
```C++
/** this is allowed
 *
 * text text
 */
```
```C++
/** brief description
 *
 * long description is not aligned with brief line
 * @bug always zero is returned
 *
 * @tparam T_Foo is our first template parameter
 * @param a is our first parameter
 * @param b is our second parameter
 * @return sum of input parameters
 */
template<
    class T_Foo
>
HDINLINE static
int
functionName(
    T_Foo a,
    double b
)
{
    return 0;
}
```


## 6. Namespaces
  - namespaces begin with a **lower case letter**
  - it is not allowed to indent nested namespaces
  - each namespace is defined on a separate line
  - the closing parenthesis `}` of a namespace **must** be placed on a new line together with a namespace name as comment
  ( regex: `}[::space://[:space:]namespace namespaceName`)`
  - code inside the deepest namespace is indented
```C++
namespace clair
{
namespace bob
{
namespace alice
{
    // this one gets intended
    struct FooMe;

} // namespace alice
} // namespace bob
} // namespace clair
```


##  7. Preprocessor
  - macros are written `ALL_UPPERCASE` and can use `_`
  - `#` is not indented
  - `#` is interpreted as one of the four (**4**) spaces for indentation
  - indent nested preprocessor commands
  - macros with newlines: align `\` symbol to column 80
```c++
#if( VARIABLE_BOB == 1 )
#   if( VARIABLE_ALICE == 1 )
#       define HPC +1
#   endif
#endif
```
  - `#ifdef` **should not** be used to check own definitions ( use `#if( MYDEFINEDPARAM == 1 )` instead )
  - avoid preprocessor macros and use C++11 features if possible
  - comment preprocessor macros
```
#define MY_MACRO( )                                                           \
    template<                                                                 \
        // typename T0, ..., typename TN                                      \
        BOOST_PP_ENUM_PARAMS( N, typename T )                                 \
    >                                                                         \
    HDINLINE                                                                  \
    void                                                                      \
    //         ( const T0, ... , const TN           )                         \
    operator( )( BOOST_PP_ENUM_PARAMS( N, const T ) ) const
```


## 8. Operators
  - unary operators are placed directly before or behind the corresponding object e.g., `i++`, `++i`
  - binary operators
    - are surrounded by spaces e.g., `x = a + b;`
    - comma `,` ( d: in function signatures ) is **always** followed by a new line with same indent
  - operators **except** the comma operator do **not** require to be followed by a newline
  - after 80 characters a new line must follow
      - use the binary operator as the last code of the previous line
      - the first new line starts a new indentation block
```C++
void
method(
    int & bob,
    const int & alice = 5
)
{
    int bob( 10 );
    int alice( 10 * bob );
    bob = bob * alice + 12 * bob * bob * bob * bob++ * --alice +
        bob -
        alice;
    alice = charge * charge / (
        6.0 * PI * EPS0 * c2 * c2 * SPEED_OF_LIGHT * mass * mass
    );
}
```
  - alternative tokens for operators `&&` ( and ), `|` ( bitor ), ... are not allowed


## 9. Function Calls
  - for **one** function parameter
    - use one line e.g., `method( 2 );`
    - parameter is surrounded by spaces ( regex: `functionName([:space:]param[:space:])` )
  - for **more than one** function parameter place each indented on a new line
  - `( ... )` are part of the *caller* (see above), no space to that caller
  - `if`, `while` and `switch` follows the same rules
```C++
void
foo( )
{
    method( globalBob );
    method(
        globalBob,
        10
    );
}
```


## 10. Type Definitions
  - the type qualifier `const` is placed right hand of the type that shall be const
  - `&` (reference) and `*` (pointer) **must** be surrounded by **one** space
```C++
int const * byte;  // pointer to const int value
int const * const byte; // const pointer to const int value
int * const byte; // const pointer to int value
int * byte; // pointer to int value
int* byte; // NOT ALLOWED by the coding guide lines (missing spaces around `*`)
```


## 11. Type Instantiations
  - if possible do not use the assignment operator
```C++
float foo = 1; // should be avoided
float foo( 1 ); // should be preferred, if possible
```
  - use of initialization lists in C++11
```C++
void
foo( )
{
    float fooo{ 1 };
    float * ptr = new T_Type{
        1,
        2
    };
}
```


## 12. Function and Method Definitions
  - functions and methods are named camel case with a beginning lower case letter
  - the function/method prefix **must** be placed on a separate line e.g., `inline`, `static` , `__device__`
  - the result type
    - **must** be placed on a separate line (sometimes the result is very long)
    - in C++11 result is always `auto` and [trailing return type definition](http://en.cppreference.com/w/cpp/language/function) is used
    - trailing return type definition is on a new line
```C++
auto
size( ) const
-> std::size_t;
```
  - function / method name must placed on a new line followed by the *oc-token* `(`
  - *oc-token* `(` must be followed by a new line
  - *oc-token* `)` is on a new line with the same indention level than `(`
  - method type qualifier are placed after `)`
  - code between `{` and `}` is indented
  - the function / method body `{ }` is followed by an empty line
```C++
__device__ static
auto
incrementValue(
    int & a
)
-> int
{
    return a++;
} // followed by an empty line

int globalA;
```


## 13. if Conditions

- omit curly `{ }` braces for one-liners
- indent the body
```c++
if( a == b )
   myFunction( c );

somethingElse( );
```
  - braces on new lines `{`
```c++
if( a == b )
{
     // do something
}
```
  - `else` is placed together with `if` on a line
```c++
if( a == b )
{
    // do something
}
else if( b == 1 )
    function( );
else
{
    foo( );
}
```


## 14. Switch Case
  - `case` is indented
  - new line after `:`
```C++
switch( value )
{
    case 'A':
        bob++;
        break;
    case 'a':
    {
        alice++;
        bob++;
    }
       break;
    default:
        moreBobs++;
}
```


## 15. Loops
  - complex parameter **can** be placed on separate lines
```C++
for( int i = 0; i <= i + 1; ++i )
{
    // this is a simple loop
}

for(
    std::vector<bool>::iterator it = v.begin();
    it != v.end();
    ++it
)
{
    // this is a loop with long type names in it's parameters
}
```
  - while loop
```C++
while( i != endOfLoop )
{
    // add your code here
}

while(
    i != endOfLoop &&
    d == 5
)
{
    // add your code here
}
```
  - do while loop
```C++
do
{
    // add your code here
}
while( i != endOfLoop );
```


## 16. Classes, Structs and Type Definitions
  - use camel case names that start with an **upper case letter** e.g., `ClassBob`, `TypeNameBob`
  - semicolon `;` **must** be placed after the closing parenthesis `}`
  - code between `{` and `};`
    - visibility definitions like `public:`, `private:` and `protected:` are not indented
    - all other code is indented
  - fixed prefix for each class is not allowed e.g., `QApplication`, `QWidget` (use namespaces)
  - inheritance
    - `final` specifier is placed inline with the class/struct name and surrounded by spaces
    - `:` is placed after the class name (regex: `ClassName[:space:]:`)
    - parent classes are indented and placed on a new line
    - each parent class is placed on a separate line
    - visibility definitions like `public`, `private` and `protected` must placed inline with the parent class
  - in C++11 `using` shall be preferred over `typedef`
```C++
struct BobClass final :
    public BaseClass1,
    private BaseClase2
{
    BobClass( )
    { }
    
private:
    // C++11
    using OneOfMyBaseClasses = BaseClass1;
    // C++98
    typedef BaseClass2 TheOtherBaseClass;
};
```
  - constructor
      - `:` is placed after the constructor name and followed by a new line
      - member initialization is indented
      - each member is placed on a separate line
```C++
struct BobClass
{
    BobClass( ) :
        a( 1 ),
        b( 2 )
    { }
    
    int a;
    int b;
};

```


## 17. Template Class / Struct and Type Definitions
  - avoid the keyword `class` inside template definitions
```C++
template<
    typename T_Foo,
    class T_FooBar // wrong keyword, should be `typename`
>
class Bob;
```
  - keyword `class` is allowed for template template specialization
```
template<
    typename T_Foo,
    template<
        typename, 
        int
    > class T_FooBar // keyword `class` is allowed
>
class Foo;
```
  - for types ( class, struct and native C types ) template arguments shall be prefixed with T_ followed by an **upper case letter** camel case
```C++
template<
    typename T_Foo,
    typename T_FooBar
>
class Bob;
```
  - for values, template arguments shall prefixed with T_ followed by a **lower case letter** and do also in camel case
  - template type `T_Type` **can** be used without type renaming with `using` or `typedef`
```C++
template<
    std::size_t T_fooSize,
    bool T_fooBool
>
class Bob;
```
  - for C++11 `using` new line after assign `=` mandatory ( definition is indented )
```C++
// C++11
template<
    std::size_t T_fooSize
>
using BobWithoutBool = Bob<
    fooSize,
    true
>;
```
  - each template argument is on a separate line
  - the *oc-token* `>` is on the same indentation level as the opening line
```C++
template<
    typename T_Bar,
    typename T_Foo
>
auto
foo(
    T_Bar const & extent,
    T_Buf & buf
)
-> mem::view::foo::detail::Bar<
    T_Bar,
    T_Foo
>; // this is not aligned with mem

// template function specialization
template< >
auto
foo<
    int,
    float
>(
    int const & extent,
    float & buf
)
-> mem::view::foo::detail::Bar<
    int,
    float
>
{
    return mem::view::foo::detail::Bar<
        int,
        float
    >( );
}
```


## 18. Template Functions and Classes as Expression
  - for one template parameter
    - use one line e.g., `method< 2 >( );`, `Foo< int >( )`
    - parameter is surrounded by spaces ( regex: `name<[:space:]param[:space:]>( )` )
  - for more than one template parameter place each indented on a new line
  - `< ... >` are part of the *function*, no space to the function name
```C++
void
foo( )
{
    method< int >( globalBob );
    method<
        int,
        float
    >(
        globalBob,
        10
    );
    
    FooClass< int >( );
    FooClass<
        int,
        float
    >( );
    
    // C++11
    using type = FooClass< int >;
    using type2 = FooClass<
        int,
        float
    >;
        
    // C++98
    typedef FooClass< int > type3;
}
```


## 19. Nameing for Embedded Types
  - for result of a type trait `::type` must be used
  - to get the type of an embedded value `::value_type`
  - to access embedded values in types `::value`
```C++
template<
    typename T_EmbeddedType,
    T_EmbeddedType T_embeddedValue
>
struct IntegralConstant
{
    static constexpr T_EmbeddedType value = T_embeddedValue;
    using value_type = T_EmbeddedType;
};

namespace traits
{
    template<
        typename T_Type
    >
    struct GetValueType
    {
        using type = typename T_Type::value_type;
    };
} // namespace traits
```


## 20. Short Access to `::type` in C++11
  - add suffix `_t` to the trait name for short access to `::type` ( [same as remove_pointer_t in C++14](http://en.cppreference.com/w/cpp/types/remove_pointer) )
```C++
template<
    typename T_Type
>
using GetValueType_t = typename traits::GetValueType< T_Type >::type;
```
